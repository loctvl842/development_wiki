# 🚀 Installation

```sh
sudo pacman -S docker docker-compose
```

Start/Enable docker

```sh
sudo systemctl start/enable docker.service
```

Add current User to the Docker Group

```sh
sudo usermod -aG docker $USER
```

# 🐳 Docker Commands, Help & Tips

### Build an Image from a Dockerfile

```sh
docker build -t image_name .
```

### Run a Container from an Image

```sh
docker run -p 8080:80 --name container_name image_name
```

If you want to run the container in the background, add the `-d` flag.

```sh
docker run -d -p 8080:80 --name container_name image_name
```

### Attach to a Running Container:

If a container is already running, attach to it.

```sh
docker run --rm -it <image> /bin/sh
```

```sh
docker exec -it container_name_or_id /bin/bash
```

## 🌐 Network

### Create a new network

```sh
docker network create --driver bridge network_name
```

### External networks

Use field `external: true` for external networks. (Use when we want to connect Docker containers to a network that is created and managed outside of Docker stack, such as a network managed by the host system or a network created by a different Docker Compose project.)

### Connect a container to a network

```sh
docker network connect network_name container_name
```

### Enabling Host Network Access for PostgreSQL Container in Docker Compose Configuration

Using `network_mode: "host"` in `docker-compose.yml` file to connect containers to the host network.

```sh
  postgres:
    image: postgres
    container_name: statistics-trading-postgres
    restart: no
    ports:
      - "5432:5432"
    network_mode: "host" # This will allow the container to connect to the host network (localhost)
```

**NOTE:**

- Using `network_mode: "host"` will ignore the `ports` section, and the container will be able to connect to the host network directly.

## 📁 Volume

### Inspecting a Docker Volume

```sh
docker volume inspect volume_name
```

### Removing Unused Docker Volumes

```sh
docker volume prune
```

### Finding Containers Using a Volume

```sh
docker ps --filter "volume=my_volume"
```

---

### List all container_id

```sh
docker ps -a -q
```

Stop all containers

```sh
docker stop $(docker ps -a -q)
```

### Clean up Docker resources (unused containers, networks, images, volumes)

- `-a`: This flag removes all unused resources, not just dangling ones. It effectively prunes all containers, networks, images, and volumes that are not currently in use by any running or stopped container. Without this flag, docker system prune would remove only dangling resources (those not associated with any container).

- `-f`: This flag is used to force the removal of resources without confirmation. When you use the -f flag, Docker will immediately proceed with the pruning operation without asking for user confirmation. Without this flag, Docker will prompt you to confirm the removal of resources.

```sh
docker system prune -af
```

## Docker compose facts

**1. Docker Compose: Environment Variable Override with `env_file` and `environment`**

In Docker Compose, when using both `env_file` and `environment` sections for a service:

- The `environment` section **overrides** environment variables defined in the `env_file` for the **same service**.

Example:

```yaml
env_file: .env
environment:
  - POSTGRES_HOST=database
```

The value for **POSTGRES_HOST** will be "database" as specified in the environment section, and any other environment variables defined in the **.env** file will remain unchanged unless overridden in the environment section.

## Install docker engine (Remote server)

- Follow this: [Install docker engine](https://docs.docker.com/engine/install/ubuntu/)

- Allow docker to run as root

```sh
sudo groupadd docker
sudo usermod -aG docker $USER
```
