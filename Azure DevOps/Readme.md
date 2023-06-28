### To build and run Azure DevOps runner via Docker Compose

```bash
# go into /azure-devops dir
cd /azure-devops

# get into compose dir
cd /compose

# open .env file, add PAT token to AZP_TOKEN var and change other ENVs if needed
vim .env

# start compose
docker-compose up -d

# after composing up can use
docker-compose start

# if docker-compose.yaml file was rename use custom file name compose
docker-compose -f custom-compose-file.yml start

# to stop Docker Compose
docker-compose stop

# to reset Docker Compose (delete)
docker-compose down
```

### To build and run Azure DevOps runner standard way (not preferred)

```bash
# go into /azure-devops dir
cd /azure-devops

# get into build dir
cd /dockeragent

# open run_ct.sh file, add PAT token to AZP_TOKEN var and change other ENVs if needed
vim run_ct.sh

# make run_ct.sh file executable
sudo chmod +x run_ct.sh

# build image
docker build -t dockeragent:latest .

# start CT from built image via script
./run_ct.sh
```

