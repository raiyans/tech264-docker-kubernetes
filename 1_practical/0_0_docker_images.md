 Docker Commands Documentation

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
