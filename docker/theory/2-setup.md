# Setup

This guide provides step-by-step instructions for installing Docker.  

> This guide will only cover the **Debian** operating system.

By the end of this guide, you will have Docker installed on your Linux machine.

- [Installation](#installation)
  - [Uninstall old versions](#uninstall-old-versions)
  - [Installation (using apt repository)](#installation-using-apt-repository)
  - [Verifying installation](#verifying-installation)
- [Config](#config)
  - [Manage Docker as a non-root user](#manage-docker-as-a-non-root-user)
    - [Troubleshooting](#troubleshooting)
  - [Configure Docker to start on boot](#configure-docker-to-start-on-boot)
  - [Default logging driver](#default-logging-driver)

## Installation

- [Official Documentation](https://docs.docker.com/engine/install/)

> **OS requirements:**
>
> - Debian Bookworm 12 (stable)
> - Debian Bullseye 11 (oldstable)

### Uninstall old versions

Before you can install Docker Engine, you should uninstall any conflicting packages.  
You should always run this before installing, even if you have a fresh OS installation.

```bash
sudo apt remove docker.io \
                docker-doc \
                docker-compose \
                podman-docker \
                containerd \
                runc
```

### Installation (using apt repository)

Before installing docker you need to add the Docker repository to your apt sources:  

```bash
sudo apt update
sudo apt install ca-certificates \
                 curl
sudo install --mode=0755 \
             --directory /etc/apt/keyrings
sudo curl --fail \
          --silent \
          --show-error \
          --location https://download.docker.com/linux/debian/gpg \
          --output /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian $(. /etc/os-release && echo "${VERSION_CODENAME}") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
```

Now you are ready to install docker:

```bash
sudo apt install docker-ce \
                 docker-ce-cli \
                 containerd.io \
                 docker-buildx-plugin \
                 docker-compose-plugin
```

### Verifying installation

To verify that the installation was successful you can run the [`hello-world`](https://hub.docker.com/_/hello-world) image:

```bash
sudo docker run hello-world
```

## Config

### Manage Docker as a non-root user

By default the Docker daemon can only be accessed by users with `root` permissions.  
The reason for this is, that the daemon always runs as the `root` user.

If you don't want to preface the `docker` command with `sudo` everytime you can greate a group called `docker` and add your user to it:

```bash
# creation of docker group
sudo groupadd docker
# adding your user to the docker group
sudo usermod --append \
             --groups docker \
             ${USER} # replace ${USER} with your username
```

After this you need to logout and log back in again to reload your group memberships.

#### Troubleshooting

You might get the following error:

```bash
WARNING: Error loading config file: /home/user/.docker/config.json -
stat /home/user/.docker/config.json: permission denied
```

This error indicates that the permissions for the `~/.docker/` directory are incorrect.  
To fix this problem, either remove the `~/.docker/` directory or change it's ownership and permissions:

```bash
# replace "${USER}" with your username
sudo chown --recursive "${USER}:${USER} "/home/${USER}/.docker"
sudo chmod --recursive g+rwx "/home/${USER}/.docker"
```

### Configure Docker to start on boot

Usually Docker automatically starts on boot. To ensure this it is however recommended to run the following commands:

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

### Default logging driver

> Logging will be covered in detail in [8. Logging](./8-logging.md)

The default logging driver is `json-file`. This writes data to JSON-formateed files on the host filesystem.  
Over time, these log files expand in size, leading to potential exhaustion of disk resources.

To avoid issues with overusing disk space for logs, consider doing the following:

Add the following to your `daemon.json` (located in `/etc/docker/`):

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m", // maximum size of the log before it's rolled (units: "k", "m" or "g")
    "max-file": "3"    // maximum number of log files before the oldest is removed
  }
}
```