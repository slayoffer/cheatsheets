### Logs

```bash
# set logging env var mode
export TF_LOG=TRACE # most detailed way
export TF_LOG=INFO
export TF_LOG=ERROR
export TF_LOG=WARNING
export TF_LOG=DEBUG

# copy logs to file
export TF_LOG_PATH=/tmp/terraform.log

# disable loging
unset export TF_LOG
unset export TF_LOG_PATH
```

