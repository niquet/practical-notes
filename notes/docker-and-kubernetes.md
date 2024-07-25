# Docker

- Docker is a platform or ecosystm around creating and running containers
- Includes the Docker Client (Docker CLI [tool used to issue commands]), Docker Server (Docker Daemon [tool that is responsible for creating images, running containers, etc.]), Docker Machine, Docker Images, Docker Hub, Docker Compose
- `Image`: single file with all the dependencies and configurations required to run a program
- `Container`: instance of an image; runs a program
- 

## Containers

- `Namespacing`: **isolating** resources per process or group of processes (processes, users, hard drive, network, hostnames, inter process communication [IPC])
- `Control Groups (cgroups)`: **limiting** amoung of resources per used proces (memory, CPU usage, HD I/O, network bandwidth)
- Namespacing and control groups are specific to the Linux operating system
- When installing Docker, a Linux Virtual Machine is installed with it that runs a Linux kernel which in turn manages the containers inside it
- `Container`: a running process or set of processes that have a portion of resources specifically assigned to it (namespacing + cgroups)
- **Container lifecycle** in Docker
  - Creating a container and starting it up (running it) are two separate processes
  - `docker run` = `docker create` + `docker start`
  - `docker create`:
  - `docker start`: 

## Docker Images

- `File System Snapshot`: "copy and paste" of a specific set of directories and files
- `Startup Command`: creates an instance of a specific process which is isolated to the specific set of resources of the container
- `Image`: contains/made up of a file system snapshot along with a specific a startup command
- 

## Docker Client (Docker CLI)

- **Creating and running a container from an image**
  - `docker run <image name>`: try to create and run a container specified by the name of the image
  - `docker run` uses the `Docker Server` to first look for the specified image in the `Image Cache` and alternatively reaches out to and grabs it from `Docker Hub` if not found in memory
  - "Hello, world" example:

  ```zsh
  docker run hello-world
  ```

- **Overriding default commands**
  - `docker run <image name> <command>`: try to create and run a container specified by the name of the image, ovveriding the default command (the startup command) with `<command>`
  - Whatever default command is specified in the image is not going to be executed
  - Example #1:
 
  ```zsh
  docker run busybox echo hi there
  ```

  - Example #2:
 
  ```zsh
  docker run busybox ls
  ```

  - Only commands that exist within the images can be executed (remember, an image consist of a specific set of directories and files)

- **Listing running containers**
  - `docker ps`: list all running containers on the currently used machine
  - `docker ps --all`: to show all containers that have ever been created on the current machine
- **Creating a container**
  - 
- **Running a container**
  -  

---

# Kubernetes

- 

## Credits

- S. Grider, "Docker and Kubernetes: The Complete Guide," Udemy, 22 Aug. 2018. [Online]. Available: [https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide](https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide). [Accessed: 19 Jul. 2024].
