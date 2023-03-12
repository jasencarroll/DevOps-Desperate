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
sudo apt install minikube
minikube start --driver=virtualbox
eval $(minikube -p minikube docker-env)
```

Navigate to the telnet-server/ directory and open the Dockerfile,