# Dockerfile

The Docker image is based on Ubuntu 14.04. On top of it we install make, cmake, tmux,
NodeJs & Git, clone the repository from `https://github.com/trilogy-group/lucida` and then run the install scripts of lucida.
The following environment variable is set in the container:

- JAVA_TOOL_OPTIONS = -Dfile.encoding=UTF8

# Quick Start

  The Docker container can be built locally or in a remote docker host. In order to make both the installation and
  run faster, it is suggested to use a remote docker host. The local built and run will take some time to finish, it may
  take hours to complete depending on the network bandwidth and the configuration of the local computer.
  `tcp://build.swarm.devfactory.com:2375` is used for testing purposes in order to prepare this guideline.
  To use local docker, do not use `-H {DOCKER_HOST:PORT}` flag in the following `docker` and `docker-compose` commands.

- Create `development_docker` folder in `{project-root-folder}`
- Unzip `lucida-dockerization.zip` in `{project-root-folder}/development_docker/`
- Open a terminal session to the folder
- Run `docker-compose -H {DOCKER_HOST:PORT} build` to build the Docker image
- Run `docker-compose -H {DOCKER_HOST:PORT} up -d` to start the container in a detached mode
- Run `docker-compose -H {DOCKER_HOST:PORT} exec lucida-builder bash` to get a session in the container
- While inside the docker container you can run the build task `make local`, this will run scripts in `tools/` to install
  all the missing required dependencies. After that, it will compile back-end services in `lucida/`
- After compilation you can start all the services using `make start_all`. Then open you browser and visit `http://localhost:3000/`
  if the container is running locally, or visit `http://{CONTAINER_IP}:{CONTAINER_PORT}/` if it is running on a remote
  docker host. To get `{CONTAINER_IP}`, open a new terminal and run `docker -H {DOCKER_HOST:PORT} info`, use the value in the `name` attribute as ip.
  To get `{CONTAINER_PORT}`, run `docker -H {DOCKER_HOST:PORT} ps` and get the mapped port value to port 3000.
- To exit the container, type `exit`
- Run `docker-compose -H {DOCKER_HOST:PORT} down` to stop the service.


# Working with the Docker Container

First create a new folder under `{project-root-folder}` using the name `development-docker`
or a name you choose. This README.md file assumes `development-docker` is used as the folder name.
Copy the Docker deliverables (`Dockerfile`, `docker-compose.yml` & `tmp_install_mongodb.sh`) to
`{project-root-folder}/development-docker/`

## Building the Docker image

In `{project-root-folder}/development-docker/` folder, run:

```bash
docker-compose -H {DOCKER_HOST:PORT} build
```

This will create a `lucida-builder:latest` Docker image in the Docker host. For the first time,
it may take 1-2 hours to finish the build on a remote host, and even much more longer on local host.

## Running the Docker container

In `{project-root-folder}/development-docker/` folder, run:

```bash
docker-compose -H {DOCKER_HOST:PORT} up -d
```

Note: Parameter `-d` runs the container in detached mode.

This will create a running container in detached mode called `lucida-builder`.
You can check the status of the container by running `docker -H {DOCKER_HOST:PORT} ps`.

## Initiating a session in the Docker container

In `{project-root-folder}/development-docker/` folder, run:

```bash
docker-compose -H {DOCKER_HOST:PORT} exec lucida-builder bash
```

This will open the bash session in `/app/lucida` folder in the container.

## docker-compose.yml

The docker-compose.yml file contains a single service: `lucida-builder`.
We use this service to build the lucida sources and start the application using the git repository `https://github.com/trilogy-group/lucida`
which is cloned during the container build.

# Building the apps

After getting a shell in the container using `docker-compose -H {DOCKER_HOST:PORT} exec lucida-builder bash`,
run `make local` in the project folder in the container (`/app/lucida`).

# Running the apps

After getting a shell in the container using `docker-compose -H {DOCKER_HOST:PORT} exec lucida-builder bash`,
and after compiling using `make local` in the project folder in the container (`/app/lucida`),
run `make start_all` in the project folder in the container (`/app/lucida`).

# Accessing web app

When the applications are running using `make start_all` command, you can visit `http://localhost:3000` if they
are running locally.

If they are running on a remote docker host, you can again use your browser, but this time `http://{CONTAINER_IP}:{CONTAINER_PORT}/`
should be used as the url. To get `{CONTAINER_IP}`, open a new terminal and run `docker -H {DOCKER_HOST:PORT} info`,
use the value in the `name` attribute as ip. To get `{CONTAINER_PORT}`, run `docker -H {DOCKER_HOST:PORT} ps`
and get the mapped port value to port 3000.