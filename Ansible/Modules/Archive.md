### Archive

```yaml
---
- hosts: all
  become: true
  tasks:
  - name: Create an archive demo.tar.gz
    archive:
      path: /usr/src/ecommerce/
      dest: /opt/ecommerce/demo.tar.gz
```

