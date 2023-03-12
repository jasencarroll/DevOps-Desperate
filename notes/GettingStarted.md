## Getting Started

If you have access to Linux or Intel based MacOS that would be the simplest solutions. Windows is... windows and arm64 MacOS might work if you can get [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [UTM](https://mac.getutm.app/) to emulate a different chip architecture. 

Follow up. I wasn't patiecd -nt enough to survive the test of installing an emulated x86 Ubuntu version in UTM. I also couldn't get Vagrant to work with VirtualBox on arm64 - or qemu. Using UTM I installed an arm64 Ubuntu server that is headless. Updated the preferences to UTM to stay open if the VM is closed, and to show in dock. Went to the VM config and deleted the display after install. However, for some reason the VM requires me to boot from next image because it always wants me to install Ubuntu first again. Also the ssh key changes every time from the VM. But if you get all that working - it works...

I wasn't able to get Vagrant working through WSL on Windows either. Though I'm not done trying there. 

At least for the time being if I want to use a laptop and I'm on my local network there's also [Remote - SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh) to give you that local feeling. You could also see if you can do this from a [Dev Container](https://code.visualstudio.com/docs/devcontainers/containers) - this uses Docker which seems to run fine with Rosetta on MacOS. For example that could give you an Ubuntu based VSCode environment. Wow. I wonder if keybindings from the local OS would work... Well... in VSCode if you do that virtualbox won't install in Ubuntu on arm64 and pulling an amd64 image would keep crashing on run regardless of Rosetta.

Alright let's get back to work. At least there VSCode can help out immediately. 

### Ubuntu

```bash
sudo apt install virtualbox vagrant ansible
vagrant plugin install vagrant-vbguest
```

Vagrnat is known as Infastructure as Code (IaC). The Vagrantfile helps you define the operating system, networking, providers (plug-ins), etc. Basic commands to get running include `vagrant up` followed by `vagrant ssh`. `vagrant status` is self explainatory as well as `vagrant destory` though the running VM will be gone afterwards.

It's helpful to do the following at this point before continuing:

```bash
sudo apt update && sudo apt upgrade
```