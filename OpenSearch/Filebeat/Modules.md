Filebeat 7.13.4 supports modules. In fact, the modules in Filebeat are designed to simplify the process of collecting/logs for a particular technology stack such as application servers, databases, and other applications. Modules in Filebeat come with pre-built configurations for popular services, which makes it easy to get started with logging for those services.

To enable a module in Filebeat, you just need to uncomment the module configuration in the `modules.d` directory and configure any required options. Once you enable a module, Filebeat will start collecting data for that technology stack and send it to the configured output.

Here’s an example to enable the Apache module:

```
# vi filebeat.yml

#-------------------------- Modules ---------------------------
filebeat.config.modules:
  enabled: true
  path: ${path.config}/modules.d/*.yml

#-------------------------- Apache ---------------------------
- module: apache
  access:
    enabled: true
    var.paths: ["/var/log/httpd/access_log*"]
  error:
    enabled: true
    var.paths: ["/var/log/httpd/error_log*"]
```

Copy Code

Once you have made the changes to the configuration file, restart Filebeat for the changes to take effect. You can also enable or disable modules using the Filebeat CLI tool by running the following command:

```
filebeat modules enable apache
```

Copy Code

With modules, you can easily collect and parse logs from various systems such as Apache, MySQL, Nginx, among others.