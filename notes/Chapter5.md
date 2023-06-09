# Automating and Testing a Host-Based Firewall

I'm starting to understand how using Ansible would be nice to provision machines over writing bash scripts.  I've reset machines so much lately or lost my condigurations some other manner that it would be nice to figure out if I can use Ansible for local machines - not just the VMs with vagrant.

Jumping to testing.

## Testing

For some reason my login tockens stopped working. 

```bash
bender@dftd:~$ ip -4 -br addr
lo               UNKNOWN        127.0.0.1/8 
enp0s3           UP             10.0.2.15/24 
enp0s8           UP             192.168.56.3/24 
```

### Fast Scan

```bash
vagrant@dftd:~$ sudo nmap -F 192.168.56.3
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-12 00:06 UTC
Nmap scan report for dftd (192.168.56.3)
Host is up (0.000010s latency).
Not shown: 97 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
5000/tcp open  upnp

Nmap done: 1 IP address (1 host up) scanned in 0.41 seconds
```

### Running Services

```bash
vagrant@dftd:~$ sudo nmap -sV 192.168.56.3
Starting Nmap 7.80 ( https://nmap.org ) at 2023-03-12 00:08 UTC
Nmap scan report for dftd (192.168.56.3)
Host is up (0.000010s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
5000/tcp open  http    Gunicorn 20.0.4
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.93 seconds
```
