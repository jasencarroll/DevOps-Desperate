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

```bash
apt install minikube
minikube start --driver=virtualbox
eval $(minikube -p minikube docker-env)
```

## Building the Container Image

Navigate to the telnet-server/ directory and open the Dockerfile,

Ok for some reason I have to when interacting with the Docker through my SSH. I wonder if jasen's permissions are messed up. I can't think of a harm in interacting with docker as root. 

Don't miss the dot!

```bash
build -t dftd/telnet-server:v1 .
docker image ls dftd/telnet-server:v1
docker run -p 2323:2323 -d --name telnet-server dftd/telnet-server:v1
docker container ls -f name=telnet-server
```

The CONTAINER ID column matches the first 12 digits of the ID received from the docker run command issued previously. The IMAGE column contains the image ID given when you built the container image. The PORTS column shows that port 2323 is exposed on every interface (0.0.0.0) and is mapping that traffic to port 2323 inside the container. The directional arrow (->) denotes the traffic flow direction. Finally, the NAMES column shows the telnet-server name set earlier from the run command.

Stop the server with `docker container stop telnet-server`

[Other Docker Client Commands](https://learning.oreilly.com/library/view/devops-for-the/9781098130251/c06.xhtml#:-:text=Other%20Docker%20Client,working%20with%20containers)

We test with minikube so, looks pretty cool actually. Ok, they're running Docker inside of minikube which is a VM that uses qemu as the VM manager. They used the IP address that minikube exposed for docker but I should just be able to use my Docker IP once I get it running again since the book said to shut it down and then I was like this is going to get confusing I might as well get off this client. 

Ok it's probably something I did. 

It's nice to know minikube can run from qemu but I'm back to following this book and set minikube to run with virtualbox. Hmm, even minikube says qemu2 is better. Well let's give it a go then and delete virtualbox with `minikube delete` - maybe something changed since the book came out. And minikube can't find my Docker CLI "default". Wow. I don't need docker locally and shutting it down allowed minikube to progress. Now I should be able to follow the book again, from building the container. 

I had to use sudo for docker local because there was something wrong with the Linux passkey required to log in to your Docker account. 

```bash
docker build -t dftd/telnet-server:v1 .
docker image ls dftd/telnet-server:v1
docker run -p 2323:2323 -d --name telnet-server dftd/telnet-server:v1
docker container ls -f name=telnet-server
```

Nope still need the passkey that I don't remember. 

```bash
sudo docker build -t dftd/telnet-server:v1 .
sudo docker image ls dftd/telnet-server:v1
sudo docker run -p 2323:2323 -d --name telnet-server dftd/telnet-server:v1
sudo docker container ls -f name=telnet-server
```

Ok I think I had misunderstood. And naturally I skipped a sentence. I have to add jasen. The sudo issue was from not adding jasen to the docker group for sudoers like we learned in a previous chapter for bender. I still think this might be because I had a local install conflicting with the instructions from the book.

Ok. 3 hours later I've finally fucking uninstalled docker entirely and I think I'm ready to start this chapter over after a reboot and that awfully long encryption password again. [I'm never install docker locally again](https://askubuntu.com/questions/935569/how-to-completely-uninstall-docker).

```bash
jasen@bertha DevOps-Desperate(main)$ minikube --version
Command 'minikube' not found, did you mean:
  command 'minitube' from deb minitube (3.9.1-1)
Try: sudo apt install <deb name>
jasen@bertha DevOps-Desperate(main)$ docker version
Command 'docker' not found, but can be installed with:
sudo snap install docker         # version 20.10.17, or
sudo apt  install docker.io      # version 20.10.21-0ubuntu1~22.04.2
sudo apt  install podman-docker  # version 3.4.4+ds1-1ubuntu1
See 'snap info docker' for additional versions.
jasen@bertha DevOps-Desperate(main)$ 
```

Success!.

# Starting Over

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

I'm not using virtualbox. 

```bash
minikube start
```

Ok I have to install Docker again. 

```bash
sudo apt-get update
sudo apt-get install ./Downloads/docker-desktop-4.17.0-amd64.deb
eval $(minikube -p minikube docker-env)
```

OMG! No sudo and no errors and I never opened Docker in the background, all that and my telnet still isn't connecting. Going back to virtualbox.

```bash
docker build -t dftd/telnet-server:v2 .
docker image ls dftd/telnet-server:v2
docker run -p 2323:2323 -d --name telnet-server dftd/telnet-server:v2
docker container ls -f name=telnet-server
```

**DEEP SIGH OF RELIEF** EVERYONE - THEY WROTE THE BOOK THIS WAY INTENTIONALLY!

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