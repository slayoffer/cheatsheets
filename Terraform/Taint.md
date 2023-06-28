### Taint

```bash
# this will roll server back to its original config and get rid of config drift
terraform taint aws_instance.webserver # marks server as tainted
terraform apply

# to untaint (unmark) server
terraform untaint aws_instance.webserver
```

