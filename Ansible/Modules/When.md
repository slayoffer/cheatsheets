### When

```yaml
---
- name: 'Am I a Banana or not?'
  hosts: localhost
  connection: local
  vars:
    fruit: banana
  tasks:
    - command: 'echo "I am a Banana"'
      when: fruit == "banana"

    - command: 'echo "I am not a Banana"'
      when: fruit != "banana"

#
-
    name: 'Execute a script on all web server nodes'
    hosts: all_servers
    tasks:
        -
            service: 'name=mysql state=started'
            when: 'ansible_host=="server4.company.com"'
```

### When finds line

```bash
-
    name: 'Add name server entry if not already entered'
    hosts: localhost
    tasks:
        -
            shell: 'cat /etc/resolv.conf'
            register: command_output
        -
            shell: 'echo "nameserver 10.0.250.10" >> /etc/resolv.conf'
            when: 'command_output.stdout.find("10.0.250.10") == -1'
```

### Loop array

```bash
name: Install Softwares
	hosts: all
	vars: 
		packages:
			- name: nginx
			required: true
			- name: mysql
			required: true
			- name: apache
			required: false
	tasks:
		- name: Install "{{ item.name }}" on Debian
		  apt:
		  	name: "{{ item.name }}"
		  	state: present
		  when:
		  	item.required == true
		  loop: "{{ packages }}"
```

