### Detailed state show

```bash
terraform state show local_file.file
```

### List states

```bash
# all
terraform state list

# specific
terraform state list aws_s3_bucket.finance-2020922
```

### Move (rename)

```bash
# rename "state-locking" database to "state-locking-db"
terraform state mv aws_dynamodb_table.state-locking aws_dynamodb_table.state-locking-db
# DONT FORGET TO MAKE SAME CHANGES IN CONFIG FILE AFTERWARDS!
```

### Show remote state contents

```bash
terraform state pull

# filtered pull
terraform state pull | jq '.resources[] | select(.name == "state-locking-db")|.instances[].attributes.hash_key'
# will pull hash_key value from db
```

### Remove

```bash
terraform state rm aws_s3_bucket.finance-2020922
# DONT FORGET TO MAKE SAME CHANGES IN CONFIG FILE AFTERWARDS!
```

