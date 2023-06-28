### Add PAM

```bash
- name: Install libpam-pwquality
  package:
    name: "libpam-pwquality"
    state: present

- name: Configure pam_pwquality
  lineinfile:
    path: "/etc/pam.d/common-password"
    regexp: "pam_pwquality.so"
    line: "password required pam_pwquality.so minlen=12 lcredit=-1 ucredit=-1 dcredit=-1 ocredit=-1 retry=3 enforce_for_root"
    state: present
```

### Password

```bash
- name: Create the user 'bender'
	no_log: True # set this to conceal sensitive data in Ansible logs
  user:
    name: bender
    shell: /bin/bash
    # hashed pwd
    password: $6$OStSJfA6FA$ffSDaJBWfSpmnwlC6QWJwLG3PgZWQhPYUSezmRvSiUFpwSWy/57KuUuLg/4yGBpKx2TWYsPDz9Yq7nL2I2XT3/
```

