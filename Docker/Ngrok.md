### Install ngrok via Apt

```bash
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null && echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list && sudo apt update && sudo apt install ngrok
```

### Install ngrok via Snap

```bash
snap install ngrok
```

### Add authtoken

```bash
ngrok config add-authtoken <token>
```

### Run

```bash
ngrok http 80
```

