### Check

```bash
# check all
ansible all --key-file ~/.ssh/id_rsa -i inventory -m ping
# or
ansible all -m setup
```