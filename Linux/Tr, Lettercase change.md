### Convert lowercase to uppercase

```bash
tr a-z A-Z < filename.txt
```

### Remove trailing line

```bash
export PRE_RELEASE=$(echo "$(preRelease)" | tr -d '\n')

# Here is an example with sed:
var=$(echo "$var" | sed -e ':a' -e 's/\n$//g;')

# And here is an example with awk:
var=$(echo "$var" | awk '{printf "%s", $0}')

# for Windows
export versionNumber=${APP_VERSION//$'\r'/}

# This command uses Parameter Expansion in bash to create a new environment variable `versionNumber` which is a copy of `APP_VERSION` with any carriage return (`\r`) characters removed
```