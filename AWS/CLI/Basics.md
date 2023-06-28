### Check version

```bash
aws --version
```

### Help

```bash
aws iam help

# or for subcmd
aws iam create-user help
```

### Config user to use CLI

```bash
aws configure
# will ask for access key from AWS

# AWS Access Key ID [None]: AKIASTKK2UQ6E553TM6F
# AWS Secret Access Key [None]: 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
# Default region name [None]: us-east-1
# Default output format [None]: json

# this will create two files in user dir
 cat ~/.aws/config
 cat ~/.aws/credentials
```

### Check AWS user id and account id

```bash
aws sts get-caller-identity
```

### List instances

```bash
aws ec2 describe-instances
```

### Manual

```bash
https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html
```

### Endpoint

```bash
aws --endpoint http://aws:4566 iam list-users
```

