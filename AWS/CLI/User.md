### Create user

```bash
aws iam create-user --user-name lucy
```

### Lists users

```bash
aws iam list-users
```

### Attach policy

```bash
# to user
aws --endpoint http://aws:4566 iam attach-user-policy --user-name mary --policy-arn arn:aws:iam::aws:policy/AdministratorAccess

# to group
aws --endpoint http://aws:4566 iam attach-group-policy --group-name project-sapphire-developers --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess
```

### Create group

```bash
aws --endpoint http://aws:4566 iam create-group --group-name project-sapphire
```

### Add to group

```bash
aws --endpoint http://aws:4566 iam add-user-to-group --user-name jack --group-name project-sapphire-developers
```

### Check user / group policies

```bash
aws --endpoint http://aws:4566 iam list-attached-group-policies --group-name project-sapphire-developers

# for user
aws --endpoint http://aws:4566 iam list-attached-user-policies --user-name jack
```

