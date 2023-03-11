# Chapter 3 Using Ansible to Configure SSH

Ok, we're going to learn how to use 2factor in this one!

Since this chapter has me learning about SSH with an already live SSH connection, the local host would not be my client but the host my client is connected to. So from the last chapter we will

```bash
bender@dftd:~$ exit
logout
vagrant@dftd:~$ exit
logout
```

Followed by:

```bash
# the name of this one is dftd and lets make it so a human can read
ssh-keygen -t rsa -f ~/.ssh/dftd -C dftd
```

Whereas I almost had to `cat` my ssh key to my server's `authorized_keys` myself from my client until I found `ssh-copy-id` - we're going to pass this through using Ansible - specifically `/chapter3/authorized_keys.yml` and seemingly the file is set up to login in with user `bender` copy the host's ssh key and load it. Nice.

Install the libpam-google-authenticator package.
Copy over preconfigured GoogleAuthenticator config.
Disable password authentication for SSH.
Configure PAM to use GoogleAuthenticator for SSH logins.
Set ChallengeResponseAuthentication to Yes.
Set Authentication Methods for bender, vagrant, and ubuntu.
Insert an additional line here that reads: Restart SSH Server.

**WARNING** Do not use this google_authenticator file in the real world, as the secret key and tokens are not so secret. Instead, keep these values safe by storing them in Ansible Vault or another product like [HashiCorp’s Vault](https://www.vaultproject.io/). You can also add the no_log: True option to any task that might write sensitive information to the provision log.

Having read through the `.yml ` exercises and taken notes for how to configure my dev PC - it's time to provision the VM. 

```bash
vagrant provision
```

Should everything go well:

```bash
ssh -i ~/.ssh/dftd -p 2222 bender@localhost
```

In the ssh command, you set the identity file to your private key 1 for authentication and set the remote SSH port to 2222. The default SSH port is 22, but Vagrant listens on a different SSH port to avoid conflicts on your local host. You also set the login user to bender and the SSH host to localhost 2.

In mine I had to copy the dtfd manually to `authorized_keys` under `bender/.ssh/`. It also hasn't asked for two factor. Oh, Ansible is stopping after Chapter 2 for some reason. Ha! I need to git pull on my host.

Ok now the two factor should work. Ok, yeah, my bad.

```bash
jasen@bertha vagrant(main)$ ssh -i ~/.ssh/dftd -p 2222 bender@localhost
Enter passphrase for key '/home/jasen/.ssh/dftd': 
(bender@localhost) Verification code: 
Welcome to Ubuntu 20.04.5 LTS (GNU/Linux 5.4.0-144-generic x86_64)
bender@dftd:~$ 