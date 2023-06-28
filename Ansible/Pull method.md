### To pull

```bash
ansible-pull -U https://github.com/myusername/ansible.git myplaybook.yml

# with SSH
sudo ansible-pull --url git@github.com:slayoffer/ansible-docker-tls.git \
        --key-file ~/.ssh/id_rsa --accept-host-key \
        local-docker-and-tls.yml.yml
        
# with branch
sudo ansible-pull --url git@github.com:slayoffer/ansible-docker-tls.git \
        --key-file ~/.ssh/id_rsa --accept-host-key \
        --checkout develop \ # for branch
        local.yml
```

### Pull only when changes in repo

```bash
ansible-pull -o -U https://github.com/jlacroix82/ansible_pull_tutorial.git

# If you don’t include the -o option, Ansible will run the entire playbook every hour. This will consume valuable CPU resources for no good reason at all. With the -o option, Ansible will only use as much CPU as required for simply checking the repository for changes. The playbook will only be run if you actually commit changes to the repository.
```

### Cron job

```bash
- name: add ansible-pull cron job
  cron:
    name: ansible auto-provision
    user: velociraptor
    minute: "*/10" # pull every 10 minutes
    job: ansible-pull -o -U https://github.com/jlacroix82/ansible_pull_tutorial.git # only pull if changes in repo
```

