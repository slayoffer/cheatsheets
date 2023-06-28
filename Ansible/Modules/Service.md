### Started

```yaml
- name: Start Nginx Service
  ansible.builtin.service:
    name: nginx 
    state: started
```

### Restarted

```bash
- name: Restart SSH Server
  service:
    name: sshd
    state: restarted
```

