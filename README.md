# DevOps For The Desperate

![dftd](book-cover.png "Book front cover")

This repo is about me working through the book [DevOps for the Desperate](https://nostarch.com/devops-desperate).

## Getting Started

If you have access to Linux or Intel based MacOS that would be the simplest solutions. Windows is... windows and arm64 MacOS might work if you can get [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [UTM](https://mac.getutm.app/) to emulate a different chip architecture. 

```bash
sudo apt install virtualbox vagrant ansible
vagrant plugin install vagrant-vbguest
```

## Exploring Vagrant

Vagrnat is known as Infastructure as Code (IaC). The Vagrantfile helps you define the operating system, networking, providers (plug-ins), etc. Basic commands to get running include `vagrant up` followed by `vagrant ssh`. `vagrant status` is self explainatory as well as `vagrant destory` though the running VM will be gone afterwards.

## Chapter 1 - Setting up a VM

### Exploring Ansible

Ansible is a Configuration Manager (CM) that can orchestrate the provisioning of infastructure like VMs by allowing you to describe what the desired state of infastructure should look like in a declarative manner as opposed to imperative, making it easier to work with for those not well versed in system admin. Ansible is written in Python and uses Yet Another Markup Language (YAML) for data serialization. Similar to Python, YAML uses indents for organization and is case sensitive. Ansible works over SSH.

#### Key Ansible Concepts

1. **Playbook:** A playbook is a collection of ordered tasks or roles that you can use to configure hosts.
2. **Control node:** A control node is any Unix machine that has Ansible installed on it. You will run your playbooks or commands from a control node, and you can have as many control nodes as you like.
3. **Inventory:** An inventory is a file that contains a list of hosts or groups of hosts that Ansible can communicate with.
4. **Module:** A module encapsulates the details of how to perform certain actions across operating systems, such as how to install a software package. Ansible comes preloaded with many modules.
5. **Task:** A task is a command or action (such as installing software or adding a user) that is executed on the managed host.
6. **Role:** A role is a group of tasks and variables that is organized in a standardized directory structure, defines a particular purpose for the server, and can be shared with other users for a common goal. A typical role could configure a host to be a database server. This role would include all the files and instructions necessary to install the database application, configure user permissions, and apply seed data.

#### Playbooks

This is the YAML (.yml) file and for the exaple of this book - it can be found under `ansible/site.yml`. This is the instructions to Ansible on how to configure the host. `sites.yml` can be broken up into multiple sections most notably the header and tasks sections. Refer to mentioned file for comments on how to read.

#### Basic Ansible Commands

The Ansible application comes with multiple commands, but you'll mostly use `ansible` and `ansible-playbook`. The first of which is used for running ad hoc or one-time commands that are executed from CLI. An example would be to instruct a group of web servers to restart Nginx:

```bash
ansible webservers-m service -a "name=nginx state=restarted" --become

# Note: the mapping for the webservers group would be located in the inventory file. service module would interact with the OS to perform the resart, the extra arguments are passed with -a, providing the service (nginx) and the fact that it should be restarted, which requires root priviledges. 
```

An example with `ansible-playbook` command, which runs the playbooks and used during the provisioning phase. 

```bash
ansible-playbook -l dockerhosts aws-cloudwatch.yml

# Note: the dockerhosts need to be listed in the inventory file for the command to succeed. If -l is not set, Ansible will run the playbook on all the hosts found in your inventory file. 
```

### Creating the Ubuntu VM

Go to the Vagrant directory and `vagrant up` . If you aren't familiar, this may take some time.

Once that is done you now have Ubuntu in Ubuntu. Let's see how many deep into the inception we are going today. Kewl beans. (I guess if you're on Apple silicone and running QTM for Ubuntu then a VM inside of Ubuntu that's 3 layers deep... so far.)

## Chapter 2: Ansible for password, group, and user management

