# Docker Hub

## Commands

### Login
To authenticate with Docker Hub, use the `docker login` command. This command prompts you to enter your Docker Hub username and password.

```sh
docker login
```

### Logout

To log out of Docker Hub, use the docker logout command.

```sh
docker logout
```

### Push

To push a local image to Docker Hub, use the `docker push` command. You need to tag the local image with your Docker Hub username and repository name before pushing.

```sh
docker push <username>/<repository>:<tag>
```

### Pull

To pull an image from Docker Hub, use the `docker pull` command. Specify the image name with the tag you want to pull.

```sh
docker pull <username>/<repository>:<tag>
```

### Tag

To tag a local image with your Docker Hub username and repository name, use the `docker tag` command.
Need to build the image first.

```sh
docker tag <local_image_id> <username>/<repository>:<tag>
```

### Search

To search for images on Docker Hub, use the `docker search` command. Provide the search query as an argument.

```sh
docker search <query>
```

### Delete

To delete an image from Docker Hub, you need to log in to Docker Hub first. Then, you can use the `docker rmi` command to delete the image.

```sh
docker rmi <username>/<repository>:<tag>
```
