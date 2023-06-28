The Swarm Visualizer is a sample app that is useful as a teaching aid. It gives us a web GUI to see our Swarm nodes and where services are running in the Swarm.  It talks to the Docker API securely through the "Linux socket" and auto-updates a web UI of what's happening. I'll be using it throughout the course, and it's also just fun to see things happening in real time graphically.

I recommend you use the `service create`  command below to run the visualizer, and there's also this same command and a stack file (if you prefer that) in the course GitHub repo under `/visualizer` 

You can just copy and paste this service create command into your Swarm:

```bash
docker service create --name=viz --publish=8080:8080/tcp --constraint=node.role==manager --mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock bretfisher/visualizer
```