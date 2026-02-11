# Hands-on Docker-02: Docker Container Basic Operations

The purpose of this hands-on training is to give students the knowledge of basic operations on Docker containers.

## Learning Outcomes

At the end of this hands-on training, students will be able to;

- List the help about the Docker commands.

- Run a Docker container on an EC2 instance.

- List the running and stopped Docker containers.

- Explain the properties of Docker containers.

- start, stop, and remove Docker containers.

## Outline

- Part 1 - Launch a Docker Machine Instance and Connect with SSH

- Part 2 - Basic Container Commands of Docker

## Part 1 - Launch a Docker Machine Instance and Connect with SSH

- Launch a Docker machine on Amazon Linux 2023 AMI with a security group allowing SSH connections using the [Cloudformation Template for Docker Machine Installation](../docker-01-installing-on-ec2-linux2/docker-installation-template.yml).

- Connect to your instance with SSH.

```bash
ssh -i .ssh/call-training.pem ec2-user@ec2-3-133-106-98.us-east-2.compute.amazonaws.com
```

## Part 2 - Basic Container Commands of Docker

- Check if the docker service is up and running.

```bash
sudo systemctl status docker
```

- Run either `docker` or `docker help` to see the help docs about docker commands.

```bash
Common Commands:
  run         Create and run a new container from an image
  exec        Execute a command in a running container
  ps          List containers
  build       Build an image from a Dockerfile
  pull        Download an image from a registry
  push        Upload an image to a registry
  images      List images
  login       Log in to a registry
  logout      Log out from a registry
  search      Search Docker Hub for images
  version     Show the Docker version information
  info        Display system-wide information

Management Commands:
  builder     Manage builds
  buildx*     Docker Buildx (Docker Inc., 0.12.1)
  container   Manage containers
  context     Manage contexts
  image       Manage images
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  plugin      Manage plugins
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Swarm Commands:
  swarm       Manage Swarm

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  import      Import the contents from a tarball to create a filesystem image
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  wait        Block until one or more containers stop, then print their exit codes

Global Options:
      --config string      Location of client config files (default "/home/ec2-user/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket to connect to
  -l, --log-level string   Set the logging level ("debug", "info", "warn", "error", "fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/home/ec2-user/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/home/ec2-user/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/home/ec2-user/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit
```

```bash
docker help | less
```

- Run `docker COMMAND --help` to see more information about a specific command.

```bash
docker run --help | less
```

- Download and run `ubuntu` os with an interactive shell open.

```bash
docker run -i -t ubuntu bash
```

- Display the os name on the container for the current user.

```bash
cat /etc/os-release
```

- Display the shell name on the container for the current user.

```bash
echo $0
```

- Update and upgrade os packages on `ubuntu` container.

```bash
apt-get update && apt-get upgrade -y
```

- Show that `ubuntu` container is like any other Ubuntu system but limited.

  - Go to the home folder and create a file named `myfile.txt`

    ```bash
    cd ~ && touch myfile.txt && ls
    ```

  - Try to edit `myfile.txt` file with `vim` editor and show that there is no `vim` installed.

    ```bash
    vim myfile.txt
    ```

  - Install `vim` editor.

    ```bash
    apt-get install vim
    ```

  - Edit `myfile.txt` file with `vim` editor and type `Hello from the Ubuntu Container` to show that `vim` command can be run now.

    ```bash
    vim myfile.txt
    ```

- Exit the `ubuntu` container and return to the ec2-user bash shell.

```bash
exit
```

- Show the list of all containers available on the Docker machine and explain container properties.

```bash
docker ps -a
```

or

```bash
docker container ls -a
```

- Run the second `ubuntu` os with an interactive shell open and name the container as `mycont` and show that this `ubuntu` container is different from the previous one.

```bash
docker run -i -t --name mycont ubuntu bash
```

- Exit the `ubuntu` container and return to the ec2-user bash shell.

```bash
exit
```

- Show the list of all containers again and explain the second `ubuntu` containers' properties and how the names of containers are given.

```bash
docker container ls -a
```

- Restart the first container by its `ID`.

```bash
docker start 4e6
```

- Show only running containers and explain the status.

```bash
docker container ls
```

- Stop the first container by its `ID` and show it is stopped.

```bash
docker stop 4e6 && docker container ls -a
```

- Restart the `mycont` container by its name and list only running containers.

```bash
docker start mycont && docker container ls
```

- Connect to the interactive shell of the running `mycont` container and `exit` afterwards.

```bash
docker exec -it mycont bash
```

- Show that `mycont` container has stopped by listing all containers.

```bash
docker container ls -a
```

- Restart the first container by its `ID` again and exec -it to show that the file we have created is still there under the home folder, and exit afterwards.

```bash
docker start 4e6 && docker exec -it 4e6 bash
```

- Show that we can get more information about `mycont` container by using `docker inspect` command and explain the properties.

```bash
docker inspect mycont | less
```

- Delete the first container using its `ID`.

```bash
docker rm 4e6
```

- Delete the second container using its name.

```bash
docker rm mycont
```

- Show that both containers are not listed anymore.

```bash
docker container ls -a
```
