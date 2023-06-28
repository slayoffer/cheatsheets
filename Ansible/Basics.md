### Inventory, Hosts

```bash
touch inventory

vim inventory

# add
worker-1 ansible_host=192.168.1.106 ansible_ssh_pass=osboxes.org ansible_connection=ssh ansible_port=22
# or for Windows
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Password123!

# check
ansible worker-1 -m ping -i inventory.txt

# if asks for sshpass
sudo yum install -y http://mirror.centos.org/centos/7/extras/x86_64/Packages/sshpass-1.06-2.el7.x86_64.rpm

# check hosts with ssh key
vim inventory

# add hosts
10.0.0.6
10.0.0.7
10.0.0.8

# check all
ansible all --key-file ~/.ssh/id_rsa -i inventory -m ping
# or
ansible all -m setup
```

### Run with user and ssh key

```bash
ansible tag_Role_webserver -i aws_ec2.yaml -m ping -u ec2-user --private-key ~/.ssh/willbutton_aws # -i for inventory file
```



### Local config

```bash
# create ansible.cfg in repo dir

# add
[defaults]
inventory = inventory
private_key_file = ~/.ssh/ansible_id_ed25519

# check
ansible all -m ping
```

### List hosts

```bash
ansible all --list-hosts
```

### Details about config host

```bash
ansible -i /inventory.yaml all -m setup
```

### List details about workers

```bash
ansible all -m gather_facts

# to limit hosts
ansible all -m gather_facts --limit 10.0.0.6
```

