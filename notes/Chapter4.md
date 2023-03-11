# Chapter 4 Controlling User Commands with Sudo

## Sudoers Secuity Polity - less is more

1. Install python3-flask, gunicorn3, and nginx
2. Copy Flask Sample Application
3. Copy Systemd Unit file for Greeting
4. Start and enable Greeting Application

I know Flask, Gunicorn, and Nginx so I'm not covering it here. 

## Anatomy of a sudeors file

1. Host_Alias A host or a group of hosts
2. Runas_Alias A list of users or groups a command can be run as
3. Cmnd_Alias Specifies a command or multiple commands
4. User_Alias A user or group of users

## Look at the examples from this chaper

And the template in ansible/templates/developers.j2