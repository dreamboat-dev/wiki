# Docker CLI

The Docker CLI is a powerful tool used to interact with the Docker engine.  
With a wide range of commands, the Docker CLI provides control over all aspects of Docker.

- [Official Documentation](https://docs.docker.com/reference/cli/docker/)

- [Images](#images)
- [Volumes](#volumes)
- [Containers](#containers)
- [Networks](#networks)
- [System](#system)

## Images

| Command                              | Description                                         |
| ------------------------------------ | --------------------------------------------------- |
| `docker image inspect ${IMAGE_NAME}` | Display detailed information on the specified image |
| `docker image prune`                 | Remove unused images                                |
| `docker image rm ${IMAGE_NAME}`      | Remove the specified image                          |
| `docker image ls`                    | List all locally available images                   |
| `docker image pull ${IMAGE_NAME}`    | Download the specified image                        |

## Volumes

| Command                                | Description                                          |
| -------------------------------------- | ---------------------------------------------------- |
| `docker volume create ${VOLUME_NAME}`  | Create the specified volume                          |
| `docker volume inspect ${VOLUME_NAME}` | Display detailed information on the specified volume |
| `docker volume prune`                  | Remove unused volumes                                |
| `docker volume rm ${VOLUME_NAME}`      | Remove the specified volume                          |
| `docker volume ls`                     | List all available volumes                           |

## Containers

| Command                                      | Description                                                                        |
| -------------------------------------------- | ---------------------------------------------------------------------------------- |
| `docker container attach ${CONTAINER_NAME}`  | Attach local `STDIN`, `STDOUT` and `STDERR` to the specified container             |
| `docker container inspect ${CONTAINER_NAME}` | Display detailed information on the specified container                            |
| `docker container restart ${CONTAINER_NAME}` | Restart the specified container                                                    |
| `docker container start ${CONTAINER_NAME}`   | Start the specified container                                                      |
| `docker container stop ${CONTAINER_NAME}`    | Stop the specified container                                                       |
| `docker ps`                                  | List all running containers                                                        |
| `docker logs ${CONTAINER_NAME}`              | Display the logs of the specified container                                        |
| `docker run ${IMAGE_NAME}`                   | Create and run a new container from an image                                       |
| `docker compose up --detach`                 | Create and run one or more new containers specified in a `docker-compose.yml` file |
| `docker exec ${CONTAINER_NAME} ${COMMAND}`   | Run the specified command in the named container                                   |

## Networks

| Command                                  | Description                                           |
| ---------------------------------------- | ----------------------------------------------------- |
| `docker network create ${NETWORK_NAME}`  | Create the specified network                          |
| `docker network inspect ${NETWORK_NAME}` | Display detailed information on the specified network |
| `docker network ls`                      | List all available networks                           |
| `docker network prune`                   | Remove all unused networks                            |
| `docker network rm ${NETWORK_NAME}`      | Remove the specified network                          |

## System

| Command               | Description                     |
| --------------------- | ------------------------------- |
| `docker system info`  | Display system-wide information |
| `docker system df`    | Show docker disk usage          |
| `docker system prune` | Remove all unused data          |
