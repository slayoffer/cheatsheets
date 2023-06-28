To configure TLS (Transport Layer Security) on Filebeat and Logstash, you would generally follow these steps:

1. Generate or obtain the necessary SSL certificate and private key files. These will be used to encrypt the communication between Filebeat and Logstash.

2. Configure Filebeat to use TLS when sending data to Logstash.

3. Configure Logstash to use TLS when receiving data from Filebeat.

Here are more detailed instructions:

**Filebeat Configuration:**

In the `filebeat.yml` configuration file, you would update the output section to something like this:

```yaml
output.logstash:
  hosts: ["logstash-host:5044"]
  ssl.certificate_authorities: ["/etc/pki/root/ca.pem"]
  ssl.certificate: "/etc/pki/client/cert.pem"
  ssl.key: "/etc/pki/client/cert.key"
```

Here:

- `logstash-host` is the hostname or IP address of your Logstash server.
- `5044` is the port number on which Logstash is listening.
- The `ssl.certificate_authorities` field should point to the path of the root CA.
- The `ssl.certificate` and `ssl.key` fields should point to the paths of the Filebeat server's certificate and private key files.

**Logstash Configuration:**

In the `logstash.yml` configuration file, you would update the input section to something like this:

```yaml
input {
  beats {
    port => 5044
    ssl => true
    ssl_certificate => "/etc/pki/logstash/logstash.pem"
    ssl_key => "/etc/pki/logstash/logstash.key"
  }
}
```

Here:

- `5044` is the port number on which Logstash is listening for incoming Beats connections.
- The `ssl_certificate` and `ssl_key` fields should point to the paths of the Logstash server's certificate and private key files.

Please replace the paths and filenames with those of your actual certificate and key files.

Please note that you need to restart the Filebeat and Logstash services after making these changes for them to take effect.

Also, remember that using self-signed certificates could lead to trust issues. You might need to add the certificates to the truststore of the operating system if they are not already trusted.

Lastly, ensure that the necessary ports are open on any firewalls between your Filebeat and Logstash instances.