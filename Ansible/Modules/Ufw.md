### Enable logs

```yaml
- name: Turn Logging level to low
  ufw:
    logging: 'low' # off, low, medium, high, full
```

### Ports

```yaml
- name: Allow SSH over port 22
  ufw:
    rule: allow # deny - drop without explanations, reject - soft drop
    port: '22'
    proto: tcp
    
- name: Allow all access to port 5000
  ufw:
    rule: allow
    port: '5000'
    proto: tcp

- name: Rate limit excessive abuse on port 5000
  ufw:
    rule: limit # will drop connections if there are more than 6 during 30 secs
    port: '5000'
    proto: tcp
    
# to edit limit rule go to
/etc/ufw/user.rules
# edit line
-A ufw-user-input -p tcp --dport 5000 # change --hitcount and --seconds par
```

### Set default traffic policy

```yaml
- name: Drop all other traffic
  ufw:
    state: enabled
    policy: deny
    direction: incoming
```

