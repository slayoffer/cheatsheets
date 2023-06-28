### Copy local pub key to remote

```bash
- name: Set authorized key file from local user
  authorized_key:
    user: bender 
    state: present
    key: "{{ lookup('file', lookup('env','HOME') + '/.ssh/dftd.pub') }}" # for Linux the path will be as /home/slayo/.ssh/dftd.pub and lookup function will cat this key
```

### 2FA

```bash
# create google auth config file and add its contents to Ansible vault
google-authenticator -f -t -d -r 3 -R 30 -w 17 -e 10 # -e 10 for 10 emergency tokens
# this will create ~/.google_authenticator file with secret and tokens

# to get new login tokens
apt install oathtool
oathtool --totp --base32 "your-base32-secret" # secret is at the top of ~/.google_authenticator

- name: Install the libpam-google-authenticator package
  apt:
    name: "libpam-google-authenticator"
    update_cache: yes
    state: present

- name: Copy over Preconfigured GoogleAuthenticator config
  copy:
    src: ../ansible/chapter3/google_authenticator
    dest: /home/bender/.google_authenticator
    owner: bender
    group: bender
    mode: '0600'

- name: Disable password authentication for SSH
  lineinfile:
    dest: "/etc/pam.d/sshd"
    regex: "@include common-auth"
    line: "#@include common-auth"

- name: Configure PAM to use GoogleAuthenticator for SSH logins
  lineinfile:
    dest: "/etc/pam.d/sshd"
    line: "auth required pam_google_authenticator.so nullok" # nullok sets 2FA as optional login method, it needs to be removed as soon as everybody sets 2FA (prod servers)

- name: Set ChallengeResponseAuthentication to Yes
  lineinfile:
    dest: "/etc/ssh/sshd_config"
    regexp: "^ChallengeResponseAuthentication (yes|no)"
    line: "ChallengeResponseAuthentication yes"
    state: present

- name: Set Authentication Methods for bender, vagrant, and ubuntu
  blockinfile:
    path: "/etc/ssh/sshd_config"
    block: |
      Match User "ubuntu,vagrant"
          AuthenticationMethods publickey
      Match User "bender,!vagrant,!ubuntu"
          AuthenticationMethods publickey,keyboard-interactive
    state: present
  notify: "Restart SSH Server"

```

