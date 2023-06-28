```bash
[[runners]]
  executor = "docker"
  [runners.docker]
    tls_verify = false
    image = "docker:latest"
    privileged = true
    disable_cache = false
    volumes = ["/var/run/docker.sock:/var/run/docker.sock", "/cache"]
    extra_hosts = ["dockerhost:<Docker host IP address>"]

```

### TLS

```bash
[[runners]]
  ...
  [runners.docker]
    ...
    tls_cert_path = "/path/to/cert.pem"
    tls_key_path = "/path/to/key.pem"
    tls_ca_file = "/path/to/ca.pem"

```