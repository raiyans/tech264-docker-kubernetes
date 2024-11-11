 Docker Commands Documentation

- [Checking Running Containers](#checking-running-containers)
- [Starting and Stopping a Container](#starting-and-stopping-a-container)
- [Removing a Container](#removing-a-container)
- [Help Command for `docker rm`](#help-command-for-docker-rm)
- [Running an Nginx Container on a Custom Port](#running-an-nginx-container-on-a-custom-port)
- [Help Command for `docker exec`](#help-command-for-docker-exec)
- [Executing Commands in a Running Container](#executing-commands-in-a-running-container)
- [Commands Inside the Container](#commands-inside-the-container)
- [Navigating Inside the Container](#navigating-inside-the-container)
- [Verifying Container Status](#verifying-container-status)
- [Creating a New Image from Container Changes](#creating-a-new-image-from-container-changes)
- [Tagging an Image](#tagging-an-image)
- [Pushing the Image to Docker Hub](#pushing-the-image-to-docker-hub)
- [Pulling the Image from Docker Hub](#pulling-the-image-from-docker-hub)


## Checking Running Containers
```bash
docker container ls -a
```
- **Description**: Lists all containers, including those that are stopped.
- **Note**: Useful for seeing the status of all containers, including their `CONTAINER ID`, `IMAGE`, `COMMAND`, `STATUS`, and `NAMES`.

---

## Starting and Stopping a Container
```bash
docker start d0326c42d5ff
docker stop d0326c42d5ff
```
- **Description**:
  - `docker start`: Starts a stopped container.
  - `docker stop`: Stops a running container.
- **Note**: The `CONTAINER ID` (e.g., `d0326c42d5ff`) is used to reference specific containers. These commands are essential for managing container lifecycle.

---

## Removing a Container
```bash
docker rm d0326c42d5ff
```
- **Description**: Removes a container. Only works on stopped containers.
- **Note**: Use `docker rm -f <container_id>` to force remove a running container.

---

## Help Command for `docker rm`
```bash
docker rm --help
```
- **Description**: Shows options for removing containers.
- **Useful Options**:
  - `-f, --force`: Force removal (useful for removing running containers).
  - `-v, --volumes`: Removes any associated anonymous volumes with the container.
- **Note**: Helpful for learning additional options and aliases.

---

## Running an Nginx Container on a Custom Port
```bash
docker run -d -p 9080:80 nginx
```
- **Description**:
  - `-d`: Runs the container in detached mode (in the background).
  - `-p 9080:80`: Maps port 80 in the container to port 9080 on the host.
- **Note**: The container will be accessible at `http://localhost:9080`.

---

## Help Command for `docker exec`
```bash
docker exec --help
```
- **Description**: Shows options for executing commands within a running container.
- **Useful Options**:
  - `-i, --interactive`: Keeps `STDIN` open.
  - `-t, --tty`: Allocates a pseudo-TTY.
  - `-e, --env`: Sets environment variables.
- **Note**: This is essential for interactive tasks, debugging, and running commands in a container.

---

## Executing Commands in a Running Container
```bash
docker exec -it f18cf2e990a10dda81b6338925682f960d3bccc48903255d2b275ddd04d0b5f2 sh
```
- **Description**:
  - `-it`: Runs the command in interactive mode with a TTY.
  - `sh`: Opens a shell session within the container.
- **Note**: Useful for accessing the container’s file system and checking details like `hostname` and `uname`.

---

## Commands Inside the Container
```bash
# Inside the container shell
pwd       # Shows current directory
ls        # Lists files in the current directory
hostname  # Shows the hostname of the container
uname     # Shows system information
uname -a  # Shows detailed system information
apt update # (Optional) Starts an update if using a Debian-based container
```
- **Descriptions**:
  - `pwd`: Current directory.
  - `ls`: Lists files and directories.
  - `hostname`: Shows container’s hostname.
  - `uname` and `uname -a`: Display OS and kernel info.
- **Note**: These commands are helpful for inspecting the environment and verifying the container setup.


---

## Navigating Inside the Container
```bash
# List all files and directories
ls

# Change to the nginx directory
cd nginx

# Edit the index.html file
nano index.html

# Exit the container
exit
```
- **Description**:
  - `ls`: Lists all files and directories in the current location.
  - `cd nginx`: Navigates to the `nginx` directory.
  - `nano index.html`: Opens the `index.html` file for editing (requires nano installed in the container).
  - `exit`: Exits the shell and returns to the host machine.
- **Note**: Useful for inspecting and modifying files inside the container.

---

## Verifying Container Status
```bash
docker ps
```
- **Description**: Lists currently running containers, showing `CONTAINER ID`, `IMAGE`, `STATUS`, and `PORTS`.
- **Note**: The `PORTS` column confirms the host-to-container port mapping.

---

## Creating a New Image from Container Changes
```bash
docker commit f18cf2e990a1
```
- **Description**: Creates a new image from changes made to a container. Outputs the image ID of the newly created image.
- **Note**: By default, the new image will not have a repository or tag.

---

## Tagging an Image
```bash
docker tag 3cba54781453 tech264_raiyan_nginx_edit
docker tag tech264_raiyan_nginx_edit raiyans/tech264_raiyan_nginx_edit:latest
```
- **Description**:
  - `docker tag <image_id> <repository:tag>`: Tags an image with a repository name and optionally a tag (e.g., `latest`).
- **Note**: Allows you to prepare the image for pushing to Docker Hub under your own repository.

---

## Pushing the Image to Docker Hub
```bash
docker push raiyans/tech264_raiyan_nginx_edit:latest
```
- **Description**: Pushes the tagged image to Docker Hub.
- **Note**: The image is now publicly accessible under your Docker Hub repository.

---

## Pulling the Image from Docker Hub
```bash
docker pull raiyans/tech264_raiyan_nginx_edit
```
- **Description**: Pulls the image from Docker Hub to the local machine.
- **Note**: Ensures you have the latest version of the image from Docker Hub.
