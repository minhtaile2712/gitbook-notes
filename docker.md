# Docker

### Operating

List

```
docker ps
```

Stop and remove a container

```
docker rm -f <container-id>
```

Start

```
docker run <image-name>
```

`-d` run in background

`-p 3000:3000` expose port

`--rm` will remove when exit

`--it` will in interactive mode

`-v <storage>:<guest-path>` mount a volume

### Storage

Two options:

* **Volumes (named volumes)**
* **Bind mounts**

#### Volumes

Create a volume

```
docker volume create <volume-name>
```

## Usage

removing dangling images

```bash
docker rmi $(docker images --filter "dangling=true" -q --no-trunc)
```

```bash
docker system prune
```

This will remove:

* all stopped containers
* all networks not used by at least one container
* all dangling images
* all dangling build cache

a list of all the containers only by their numeric ID

```bash
docker container ls -aq
```

remove stopped containers

```bash
docker container rm $(docker container ls -aq)
```

auto remove container when it exits

```bash
docker run --rm
```

