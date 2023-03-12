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

We test with minikube so 

```bash
sudo apt install minikube
minikube start --driver=virtualbox
eval $(minikube -p minikube docker-env)
```

## Building the Container Image

Navigate to the telnet-server/ directory and open the Dockerfile,

Ok for some reason I have to sudo when interacting with the Docker through my SSH. I wonder if jasen's permissions are messed up. I can't think of a harm in interacting with docker as root. 

Don't miss the dot!

```bash
sudo build -t dftd/telnet-server:v1 .
sudo docker image ls dftd/telnet-server:v1
sudo docker run -p 2323:2323 -d --name telnet-server dftd/telnet-server:v1
sudo docker container ls -f name=telnet-server
```

The CONTAINER ID column matches the first 12 digits of the ID received from the docker run command issued previously. The IMAGE column contains the image ID given when you built the container image. The PORTS column shows that port 2323 is exposed on every interface (0.0.0.0) and is mapping that traffic to port 2323 inside the container. The directional arrow (->) denotes the traffic flow direction. Finally, the NAMES column shows the telnet-server name set earlier from the run command.

Stop the server with `sudo docker container stop telnet-server`

[Other Docker Client Commands](https://learning.oreilly.com/library/view/devops-for-the/9781098130251/c06.xhtml#:-:text=Other%20Docker%20Client,working%20with%20containers)