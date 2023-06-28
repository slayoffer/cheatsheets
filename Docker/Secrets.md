### Using Swarm

To create a secret, we need to first initialize Docker Swarm. You can do so using the following command:

```bash
docker swarm init
```

Once the service is initialized, we can use the `docker secret create` command to create the secret:

```bash
# want to make secret of ssh key
ssh-keygen -t rsa -b 4096 -N "" -f mykey.key

# create secret
docker secret create my_secret_key mykey,key

# delete ssh key
rm mykey.key

# add secret to Compose
version: '3.7'
services:
  myapp:
    image: mydummyapp:latest
    secrets:
      - my_secret
    volumes:
      - type: bind
        source: my_secret_key
        target: /run/secrets/my_secret
        read_only: true
secrets:
  my_secret:
    external: true
```

In the example compose file, the secrets section defines a secret named `my_secret_key` (discussed earlier). The `myapp` service definition specifies that it requires `my_secret_key` , and mounts it as a file at `/run/secrets/my_secret` in the container.

### Using file

Docker Compose is a powerful tool for defining and running multi-container applications with Docker. A stack is defined by a `docker-compose` file allowing you to define and configure the services that make up your application, including their environment variables, networks, ports, and volumes. With Docker Compose, it is easy to set up an application in a single configuration file and deploy it quickly and consistently across multiple environments.

Docker Compose provides an effective solution for managing secrets for organizations handling sensitive data such as passwords or API keys. You can read your secrets from an external file (like a TXT file). But be careful not to commit this file with your code:

```yaml
version: '3.7'
services:
  myapp:
    image: myapp:latest
    secrets:
      - my_secret
secrets:
  my_secret:
    file: ./my_secret.txt
```
  
### Using a Sidecar Container

A typical strategy for maintaining and storing secrets in a Docker environment is to use sidecar containers. Secrets can be sent to the main application container via the sidecar container, which can also operate a secrets manager or another secure service.

Let’s understand this using a Hashicorp Vault sidecar for a MongoDB container:

-   First, create a Docker Compose (`docker-compose.yml`) file with two services: `mongo` and `secrets`.
-   In the `secrets` service, use an image containing your chosen secret management tool, such as a vault.
-   Mount a volume from the secrets container to the `mongo` container so the `mongo` container can access the secrets stored in the secrets container.
-   In the `mongo` service, use environment variables to set the credentials for the MongoDB database, and reference the secrets stored in the mounted volume.

Here is the example compose file:

```yaml
version: '3.7'

services:
  mongo:
    image: mongo
    volumes:
      - secrets:/run/secrets
    environment:
      MONGO_INITDB_ROOT_USERNAME_FILE: /run/secrets/mongo-root-username
      MONGO_INITDB_ROOT_PASSWORD_FILE: /run/secrets/mongo-root-password
  secrets:
    image: vault
    volumes:
      - ./secrets:/secrets
    command: ["vault", "server", "-dev", "-dev-root-token-id=myroot"]
    ports:
      - "8200:8200"
      
volumes:
  secrets:
```

### Using Mozilla SOPS

Mozilla SOPS (Secrets Ops) is an open-source platform that provides organizations with a secure and automated way to manage encrypted secrets in files. It offers a range of features designed to help teams share secrets in code in a safe and practical way. The following assumes you are already familiar with SOPS, if that’s not the case, start [here](https://dzone.com/articles/a-comprehensive-guide-to-sops-managing-your-secret).

Here is an example of how to use SOPS with `docker-compose.yml`:

```yaml
version: '3.7'

services:
  myapp:
    image: myapp:latest
    environment:
      API_KEY: ${API_KEY}
    secrets:
      - mysecrets

  sops:
    image: mozilla/sops:latest
    command: ["sops", "--config", "/secrets/sops.yaml", "--decrypt", "/secrets/mysecrets.enc.yaml"]
    volumes:
      - ./secrets:/secrets
    environment:
      # Optional: specify the path to your PGP private key if you encrypted the file with PGP
      SOPS_PGP_PRIVATE_KEY: /secrets/myprivatekey.asc

secrets:
  mysecrets:
    external: true
```

In the above, the `myapp` service requires a secret called `API_KEY`. The `secrets` section uses a secret called `mysecrets`, which is expected to be stored in an external key/value store, such as Docker Swarm secrets or HashiCorp Vault.

The `sops` service uses the official SOPS Docker image to decrypt the `mysecrets.enc.yaml` file, which is stored in the local `./secrets` directory. The decrypted secrets are mounted to the `myapp` service as environment variables.

**_Note_**: Make sure to create the secrets directory and add the encrypted `mysecrets.enc.yaml` file and the `sops.yaml` configuration file (with SOPS configuration) in that directory.

## Scan for Secrets in Your Docker Images

Hard coding secrets in Docker is a significant security risk, making them vulnerable to attackers. We have seen different best practices to avoid hard-coding secrets in plain text in your Docker images, but security doesn’t stop there.

### **You Should Also Scan Your Images for Secrets**

All Dockerfiles start with a `FROM` directive that defines the base image. It’s important to understand when you use a base image, especially from a public registry like Docker Hub, you are pulling external code that may contain hardcoded secrets. More information is exposed than visible in your single Dockerfile. Indeed, it’s possible to retrieve a plain text secret hard-coded in a previous layer starting from your image.

In fact, many public Docker images are concerned: in 2021, we estimated that ****7% of the** Docker Hub **images contained at least one secret.****

Fortunately, you can easily detect them with `ggshield` (GitGuardian CLI). For example:

```bash
ggshield secret scan docker ubuntu:22.04
```