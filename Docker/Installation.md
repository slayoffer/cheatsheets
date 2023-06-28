## Fast installation

One click installation script:

```bash
curl -fsSL https://get.docker.com -o get-docker.sh

sudo sh get-docker.sh
```

### General install (might be deprecated!)

Ориентируемся на [инструкцию](https://docs.docker.com/engine/install/ubuntu/) с официального сайта: 

```bash
sudo apt update

sudo apt install \
    ca-certificates \
    curl \
    gnupg
    
sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/docker.gpg

sudo chmod a+r /etc/apt/trusted.gpg.d/docker.gpg

# add Docker repo
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/trusted.gpg.d/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Добавим текущего пользователя в `docker group`:

```bash
  sudo groupadd docker
  sudo usermod -aG docker $USER 
```

Минутка торжества и "Hello world":

```bash
 docker run debian echo "Hello World" 
```

### Check Docker version

```bash
# check installation (Ubuntu)
sudo dpkg -l | grep -i docker

# check version
docker --version

# check edition and other details
docker version
```

### Keyring deprecation, Repo update error

```bash
sudo apt update

sudo apt install apt-transport-https

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo rm /etc/apt/sources.list.d/download_docker_com_linux_ubuntu.list

sudo apt update
```

