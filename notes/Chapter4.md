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

I think I could actually uncomment the previous chapters for faster build times. 

```bash
ssh -i ~/.ssh/dftd -p 2222 bender@localhost
```

```bash
bender@dftd:~$ curl http://localhost:5000
<h1 style='color:green'>Greetings!</h1>
```

Wow most successful deploy in my life - hole in one! 

## Testing sudeos policy.

```bash
bender@dftd:~$ sudoedit /opt/engineering/greeting.py
bender@dftd:~$ sudo systemctl stop greeting
bender@dftd:~$ curl http://localhost:5000
curl: (7) Failed to connect to localhost port 5000: Connection refused
bender@dftd:~$ sudo systemctl start greeting
bender@dftd:~$ curl http://localhost:5000
<h1 style='color:green'>Greetings and Salutations!</h1>
```

## Audit Logs

