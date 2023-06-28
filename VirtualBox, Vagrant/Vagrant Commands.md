#### To initialize a vagrant machine

```
vagrant init

// better point to vagrant box at once
vagrant init geerlingguy/centos7
```

#### To build & start up vagrant machine

```bash
vagrant up
# ! If shows ISO download error, then turn on VPN !
```

#### To log into vagrant machine

```
vagrant ssh
```

#### To stop vagrant machine

```
vagrant halt
```

#### To delete vagrant machine

```
vagrant destroy
```

#### To reboot vagrant machine

```bash
vagrant reload

# to apply provisioning
vagrant reload --provision
```

Vagrant statuses

```bash
# simple status
vagrant status

# global
vagrant global-status # can add --prune arg
```

