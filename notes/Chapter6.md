# Chapter 6 Containerizing an Application with Docker

Not - I've done this with Azure already. 

Common kernel namespaces include the following:

Process ID (PID) Isolates the process IDs
Network (net) Isolates the network interface stack
UTS Isolates the hostname and domain name
Mount (mnt) Isolates the mount points
IPC Isolates the SysV-style interprocess communication
User Isolates the user and group IDs
Using these namespaces is not enough, however. You also need to control how much memory, CPU, and other physical resources a container uses. That’s where cgroups come in. Cgroups manage and measure the resources a container can use. They allow you to set resource limitations and prioritization for processes. The most common resources Docker sets with cgroups are memory, CPU, disk I/O, and network. Cgroups make it possible to stop a container from using up all the resources on a host.

You can use minikube to install Docker if you haven't already - I'm not aware of what the advantages would be at this time. minikube needs a VM manager to install Docker. 

[minikube](https://minikube.sigs.k8s.io/docs/start/)

```bash
minikube start --driver=virtualbox
# It's importat to start minikube's docker-env 
minikube docker-env
# export DOCKER_TLS_VERIFY="1"
# export DOCKER_HOST="tcp://192.168.59.105:2376"
# export DOCKER_CERT_PATH="/home/jasen/.minikube/certs"
# export MINIKUBE_ACTIVE_DOCKERD="minikube"
# To point your shell to minikube's docker-daemon, run:
# eval $(minikube -p minikube docker-env)
eval $(minikube -p minikube docker-env)
```

## Building the Container Image

Navigate to the telnet-server/ directory and open the Dockerfile,

Don't miss the dot!

```bash
docker build -t dftd/telnet-server:v1 . 
docker image ls dftd/telnet-server:v1 
docker run -p 2323:2323 -d --name telnet-server dftd/telnet-server:v1 
docker container ls -f name=telnet-server
```

The CONTAINER ID column matches the first 12 digits of the ID received from the docker run command issued previously. The IMAGE column contains the image ID given when you built the container image. The PORTS column shows that port 2323 is exposed on every interface (0.0.0.0) and is mapping that traffic to port 2323 inside the container. The directional arrow (->) denotes the traffic flow direction. Finally, the NAMES column shows the telnet-server name set earlier from the run command.

Stop the server with `docker container stop telnet-server`

[Other Docker Client Commands](https://learning.oreilly.com/library/view/devops-for-the/9781098130251/c06.xhtml#:-:text=Other%20Docker%20Client,working%20with%20containers)

```bash
jasen@bertha telnet-server(main*)$ minikube ip
192.168.59.101
jasen@bertha telnet-server(main*)$ telnet 192.168.59.101 2323
Trying 192.168.59.101...
Connected to 192.168.59.101.
Escape character is '^]'.

____________ ___________
|  _  \  ___|_   _|  _  \
| | | | |_    | | | | | |
| | | |  _|   | | | | | |
| |/ /| |     | | | |/ /
|___/ \_|     \_/ |___/

>d
Sun Mar 12 16:37:19 +0000 UTC 2023
>q
Good Bye!
Connection closed by foreign host.
```

## Holy moly batman what a trip

When the going gets tough turn to 90s grunge. Pink Floyd also works. Nothing like the steel african drums in Jane Says though.

AHHH all the keyboard shortcuts. Is your left pinky getting tired yet?