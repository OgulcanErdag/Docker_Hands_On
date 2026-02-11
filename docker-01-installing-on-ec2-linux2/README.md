# Hands-on Docker-01: Installing Docker on Amazon Linux 2023 AWS EC2 Instance

The purpose of this hands-on training is to teach the students how to install Docker on an Amazon Linux 2023 EC2 instance.

## Learning Outcomes

At the end of this hands-on training, students will be able to;

- Install Docker on an Amazon Linux 2023023 EC2 instance

- Configure a Cloudformation template for creating a Docker machine

## Outline

- Part 1 - Launch Amazon Linux 2023 EC2 Instance and Connect with SSH

- Part 2 - Install Docker on Amazon Linux 2023 EC2 Instance

## Part 1 - Launch Amazon Linux 2023 EC2 Instance and Connect with SSH

- Launch an EC2 instance using the Amazon Linux 2023 AMI with a security group allowing SSH connections.

- Connect to your instance with SSH.

```bash
ssh -i .ssh/my-key.pem ec2-user@ec2-3-133-106-98.us-east-2.compute.amazonaws.com
```

## Part 2 - Install Docker on Amazon Linux 2023 EC2 Instance

- Update the installed packages and package cache on your instance.

```bash
sudo dnf update -y
```

- Install the most recent Docker Community Edition package.

```bash
sudo dnf install docker -y
```

- Start docker service.

- Init System: Init (short for initialization) is the first process started during booting of the computer system. It is a daemon process that continues running until the system is shut down. It also controls services at the background. For starting docker service, init system should be informed.

```bash
sudo systemctl start docker
```

- Enable docker service so that docker service can restart automatically after reboots.

```bash
sudo systemctl enable docker
```

- Check if the docker service is up and running.

```bash
sudo systemctl status docker
```

- Add the `ec2-user` to the `docker` group to run docker commands without using `sudo`.

```bash
sudo usermod -a -G docker ec2-user
```

- Normally, the user needs to re-login into the bash shell for the group `docker` to be effective, but `newgrp` command can be used to activate `docker` group for `ec2-user`, without re-login into the bash shell.

```bash
newgrp docker
```

- Check the docker version without `sudo`.

```bash
$ docker version
Client:
 Version:           25.0.3
 API version:       1.44
 Go version:        go1.20.12
 Git commit:        4debf41
 Built:             Mon Feb 12 00:00:00 2024
 OS/Arch:           linux/amd64
 Context:           default

Server:
 Engine:
  Version:          25.0.3
  API version:      1.44 (minimum version 1.24)
  Go version:       go1.20.12
  Git commit:       f417435
  Built:            Mon Feb 12 00:00:00 2024
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.7.11
  GitCommit:        64b8a811b07ba6288238eefc14d898ee0b5b99ba
 runc:
  Version:          1.1.11
  GitCommit:        4bccb38cc9cf198d52bebf2b3a90cd14e7af8c06
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

- Check the docker info without `sudo`.

```bash
$ docker info
Client:
 Version:    25.0.3
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.0.0+unknown
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 3
 Server Version: 25.0.3
 Storage Driver: overlay2
  Backing Filesystem: xfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 64b8a811b07ba6288238eefc14d898ee0b5b99ba
 runc version: 4bccb38cc9cf198d52bebf2b3a90cd14e7af8c06
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.1.79-99.167.amzn2023.x86_64
 Operating System: Amazon Linux 2023.4.20240319
 OSType: linux
 Architecture: x86_64
 CPUs: 1
 Total Memory: 949.6MiB
 Name: ip-172-31-89-214.ec2.internal
 ID: 6d116b23-6d49-4a92-852b-08483d90be43
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
```
