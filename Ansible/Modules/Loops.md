### Loop

```bash
- name: Copy Flask Sample Application
  copy:
    src: "../ansible/chapter4/{{ item }}"
    dest: "/opt/engineering/{{ item }}"
    group: developers
    mode: '0750'
  loop:
    - greeting.py
    - wsgi.py
```

