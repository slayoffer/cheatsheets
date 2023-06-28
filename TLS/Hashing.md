### Md5

```bash
echo -n "hello" | md5sum # -n to get rid of new line in the end of string
```

### Sha

```bash
echo -n "hello" | sha1sum
```

### Bcrypt

```bash
bcrypt my_password # This will generate a bcrypt hash for the password "my_password". The output will look something like this (the actual hash value will be different):
$2a$10$tOeUuaB6NlJ6U1D6UQycjuX9T/cktwF/IYJ/036kRo6QsipU6yvU6 # The prefix "$2a$" indicates that this is a bcrypt hash.
```

### Hash SSH key

```bash
ssh-keygen -lf ~/.ssh/rubius_rsa_key.pub -E sha256 # the `-lf` option is used to specify the path to the public key file and print its fingerprint
```