### Hosts

```bash
hosts:
	Host1, Host2, Host3
	Group1, Host1
	Host*
    *.company.com
    
# check ansible docs for more patterns
```

### Dynamic inventory file

```bash
# use python script instead of static file
ansible-playbook —i inventory.py playbook.yml

# check ansible docs for dynamic inventory scripts
```

### Custom modules

```bash
# check ansible dev guide for templates
```

