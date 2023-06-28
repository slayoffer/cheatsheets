### Runner CT start

```bash
# https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops#linux

docker run --restart=always -d -e "AZP_URL=https://tfs.rubius.com" -e "AZP_TOKEN=3jptiphlmewhh2r63rokezvbexpxuzafjrppsoouawvrxlmvw5lq" -e "AZP_AGENT_NAME=dockeragent-1" -e "AZP_POOL=Linux-agents" dockeragent:latest

# or
export AZP_TOKEN=3jptiphlmewhh2r63rokezvbexpxuzafjrppsoouawvrxlmvw5lq
export AZP_URL=https://tfs.rubius.com
export AZP_AGENT_NAME=dockeragent-1
export AZP_POOL=Linux-agents 

docker run \
  --restart=always \
  -e AZP_URL=$AZP_URL \
  -e AZP_TOKEN=$AZP_TOKEN \
  -e AZP_AGENT_NAME=$AZP_AGENT_NAME \
  -e AZP_POOL=$AZP_POOL \
  -d dockeragent:latest
```

