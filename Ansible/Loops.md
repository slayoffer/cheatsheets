### Standard loop

```bash
name: Create users
hosts: localhost
tasks:
  - user: name={{ item.name }} uid={{ item.uid }} state=present                       
   loop:
     - name: joe
       uid: 1010
     - name: vlad
       uid: 1011
     - name: slayo
       uid: 1012
     - name: smart
       uid: 1013
```

### With_* (old way)

```bash
-
    name: 'Install required packages'
    hosts: localhost
    vars:
        packages:
            - httpd
            - binutils
            - glibc
            - ksh
            - libaio
            - libXext
            - gcc
            - make
            - sysstat
            - unixODBC
            - mongodb
            - nodejs
            - grunt
    tasks:
        -
            yum:
                name: '{{ item }}'
                state: present
            with_items: '{{ packages }}'
            # or
            loop: {{ packages }}


# other example
name: Create users
hosts: localhost
tasks:
  - user: name={{ item.name }} uid={{ item.uid }} state=present                       
   with_items:
     - name: joe
       uid: 1010
     - name: vlad
       uid: 1011
     - name: slayo
       uid: 1012
     - name: smart
       uid: 1013
       
# for files
with_file:
	- "etc/hosts"

# for urls
with_url:
	- "https://blabla.com"
	
# others
with_items
with_file
with_ur1
with_mongodb
with_dict
with_etcd
with_env
with_filetree
With_ini
With_inventory_hostnames
With_k8s
With_manifold
With_nested
With_nios
With_openshift
With_password
With_pipe
With_rabbitmq
With_redis
with_sequence
With_skydive
With_subelements
With_template
With_together
With_varnames
```

