To avoid being prompted for your SSH password repeatedly while using Git, you can configure Git to use SSH key-based authentication and enable SSH agent forwarding. Here's how you can do it:

```bash
eval "$(ssh-agent -s)" && ssh-add ~/.ssh/rubius_ed25519

# to enable
git config --global --edit

# add
[core]
 sshCommand = "ssh -o ForwardAgent=yes"
```