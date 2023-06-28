### Via Docker Compose

- Update all images (in the folder):

  ```
  docker compose pull
  ```

  - or update a single image: `docker compose pull plex`

- Let compose update all containers as necessary:

  ```
  docker compose up -d
  ```

  - or update a single container: `docker compose up -d plex`

- You can also remove the old dangling images: `docker image prune`

### Via Docker Run

- Update the image: `docker pull lscr.io/linuxserver/plex:latest`
- Stop the running container: `docker stop plex`
- Delete the container: `docker rm plex`
- Recreate a new container with the same docker run parameters as instructed above (if mapped correctly to a host folder, your `/config` folder and settings will be preserved)
- You can also remove the old dangling images: `docker image prune`

### Via Watchtower auto-updater (only use if you don't remember the original parameters)

- Pull the latest image at its tag and replace it with the same env variables in one run:

  ```bash
  docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower \
  --run-once plex
  ```

- You can also remove the old dangling images: `docker image prune`

**Note:** We do not endorse the use of Watchtower as a solution to automated updates of existing Docker containers. In fact we generally discourage automated updates. However, this is a useful tool for one-time manual updates of containers where you have forgotten the original parameters. In the long term, we highly recommend using [Docker Compose](https://docs.linuxserver.io/general/docker-compose).

### Image Update Notifications - Diun (Docker Image Update Notifier)

- We recommend [Diun](https://crazymax.dev/diun/) for update notifications. Other tools that automatically update containers unattended are not recommended or supported.