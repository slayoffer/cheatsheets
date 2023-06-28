### Create file and add content

```bash
---
- hosts: node01
  become: true
  tasks:
    - name: Creating blog.txt file
      file:
        path: /opt/news/blog.txt
        state: touch
        group: sam
    - name: create a file and add text
      copy:
        dest: /opt/file.txt
        content: "This file is created by Ansible!"

- hosts: node02
  become: true
  tasks:
    - name: Creating story.txt file
      file:
        path: /opt/news/story.txt
        state: touch
        owner: sam
```

