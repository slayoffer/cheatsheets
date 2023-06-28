### Set vault password keyfile

```bash
# can be automatically set when running ansible-vault create

# or can be manually set with file - each file has its own password!

# to never type password create main vault file with your password inside (will be used as encryption key)
vim ~/.vault_key

# mod rights
chmod 600 ~/.vault_key

# add to gitignore
echo '.vault_key' >> .gitignore

# use "--vault-password-file ~/.vault_key" flag when creating secrets
```

### Reset vault key

```bash
ansible-vault rekey <filename> --vault-password-file ~/.vault_key
```

### Pull method with vault key

```bash
sudo ansible-pull --vault-password-file ~/.vault_key https://github.com/jlacroix82/ansible_pull_tutorial.git
```

### Encryption with or without key

```bash
# create file and encrypt it
ansible-vault create secret.yml
# then add var with password to this file and it will be encrypted
password: "wassup"

# encrypt existing file
ansible-vault encrypt file.txt
# or without entering password
ansible-vault encrypt --vault-password-file ~/.vault_key file.txt

# decrypt file
ansible-vault decrypt --vault-password-file ~/.vault_key file.txt

# edit file
ansible-vault edit --vault-password-file ~/.vault_key file.txt

# show content of encrypted file
ansible-vault view vault.yml
```

### Reading the Password File Automatically

To avoid having to provide a flag at all, you can set the `ANSIBLE_VAULT_PASSWORD_FILE` environment variable with the path to the password file:

```bash
export ANSIBLE_VAULT_PASSWORD_FILE=./.vault_pass
```

You should now be able to execute the command without the `--vault-password-file` flag for the current session:

```bash
ansible -bK -m copy -a 'src=secret_key dest=/tmp/secret_key mode=0600 owner=root group=root' localhost
```

To make Ansible aware of the password file location across sessions, you can edit your `ansible.cfg` file.

Open the local `ansible.cfg` file we created earlier:

```bash
nano ansible.cfg
```

In the `[defaults]` section, set the `vault_password_file` setting. Point to the location of your password file. This can be a relative or absolute path, depending on which is most useful for you:

ansible.cfg

```
[defaults]
. . .
vault_password_file = ./.vault_key
```

Now, when you run commands that require decryption, you will no longer be prompted for the vault password. As a bonus, `ansible-vault` will not only use the password in the file to decrypt any files, but it will apply the password when creating new files with `ansible-vault create` and `ansible-vault encrypt`.

### Reading the Password from an Environment Variable

You may be worried about accidentally committing your password file to your repository. While Ansible has an environment variable to point to the location of a password file, it does not have one for setting the password itself.

However, if your password file is executable, Ansible will run it as a script and use the resulting output as the password.

Open up your `.vault_pass` file in your editor:

```bash
nano .vault_pass
```

Replace the contents with the following script:

.vault_pass

```python
#!/usr/bin/env python3

import os
print os.environ['VAULT_PASSWORD']
```

Make the file executable by typing:

```bash
chmod +x .vault_pass
```

You can then set and export the `VAULT_PASSWORD` environment variable, which will be available for your current session:

```
export VAULT_PASSWORD=my_vault_password
```

You will have to do this at the beginning of each Ansible session, which may sound inconvenient. However, this effectively guards against accidentally committing your Vault encryption password, which could have serious drawbacks.