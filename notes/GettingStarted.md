## Getting Started

If you have access to Linux or Intel based MacOS that would be the simplest solutions. Windows is... windows and arm64 MacOS might work if you can get [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [UTM](https://mac.getutm.app/) to emulate a different chip architecture. 

```bash
sudo apt install virtualbox vagrant ansible
vagrant plugin install vagrant-vbguest
```

Vagrnat is known as Infastructure as Code (IaC). The Vagrantfile helps you define the operating system, networking, providers (plug-ins), etc. Basic commands to get running include `vagrant up` followed by `vagrant ssh`. `vagrant status` is self explainatory as well as `vagrant destory` though the running VM will be gone afterwards.

