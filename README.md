# Project Summary: #

This project is a Linux server that serves the web application from my Item-
Catalog-Project repository to the web with basic firewall specifications.
This README file will provide the following information to connect with the
server:

<br/>

1. IP address and SSH port
2. Complete URL to hosted web application
3. A summary of software installed and configuration changes made.
4. A list of any third-party resources used.

<br/>

# Configurations #  

<br/>

### IP Address and SSH port ###

<br/>

**IP:** 18.214.152.104
**SSH port:** 2200

<br/>

### Complete URL to Hosted Web App ###

<br/>

**URL:** http://18.214.152.104.xip.io/

<br/>

### Software Installed and Config Changes ###

<br/>

**Summary of Installed Software:**

* python3 (Language)
* python3-pip
* apache2 (HTTP server)
* flask (web framework for Python)
* postgresql (Database)
* packaging, oauth2client, redis, passlib, flask-httpauth
* sqlalchemy, flask-sqlalchemy, psycopg2-binary, bleach, requests
* libapache2-mod-wsgi-py3
* git

<br/>

**Config changes**

1. Updated all currently installed packages using the following statements:

```
sudo apt-get update
sudo apt-get upgrade
```

<br/>

2. Configure Lightsail firewall to allow ssh on port 2200 and within the Linux
server, we must change ssh port to be 2200 instead of 22.
<br/>

**Part 1-** For Lightsail, you need to change it within the Lightsail platform by going into
the instance networking tab. In this new tab, you can find the firewall section
and add a new connection that has the application named as custom, protocol
as TCP and port range as 2200.

**Part 2-** Now sign in to the Linux server and edit the file sshd_config to change
ssh port from 22 to 2200. Somewhere in the file, you should see a comment that
says "# What ports, IPs and protocols we listen for" and right under that line,
there should be the port number 22. Change this line to Port 2200 instead.

<br/>

Here is the path to sshd_config: "/etc/ssh/sshd_config"

<br/>

3. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections
for SSH (port 2200), HTTP (port 80), and NTP (port 123). Use the following
commands to configure the ufw.

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow www
sudo ufw allow 2200/tcp
sudo ufw allow 123/udp
sudo ufw enable
```

<br/>

4. Create a new user account named "grader," give user grader permission to
sudo, and create SSH key pair for grader using the ssh-keygen tool.


**Part 1-** Create user grader

```
sudo adduser grader --- All user information is optional to fill in
```

**Part 2-** Give user grader permission to sudo by using the following commands:

```
sudo cp /etc/sudoers.d/90-cloud-init-users /etc/sudoers.d/grader
sudo nano /etc/sudoers.d/grader
```

<br/>

When editing "etc/sudoers.d/grader", you should see the line
"ubuntu ALL=(ALL) NOPASSWD:ALL". Change ubuntu to grader instead and save.

<br/>

**Part 3-** Create SSH key pair for grader using the ssh-keygen tool and installing
a public key.

Go into your own linux system or bash and using the following command:

```
ssh-keygen
```

Now sign into server as ubuntu user and switch to grader user by using this
command: "sudo su grader"

As user grader, go to the home page of grader and install public key. Follow
the commands below.

```
mkdir .ssh
touch .ssh/authorized_keys
nano .ssh/authorized_keys
```

The file authorized_keys should be empty right now. Fill the file with contents
from the public key you generated using ssh-keygen. Remember to use the public key
which ends with the file extension ".pub"

Once you have copied the contents from the public key into authorized_keys,
save the file. Finally to finish it off, type these two final commands so
we can sign in as grader:

```
chmod 700 .ssh
chmod 644 .ssh/authorized_keys
```

5. Configured the local timezone to UTC </br>
6. Installed postgresql and created a new database user name catalog that has limited permissions
  to catalog application database. </br>
7. Added my item-catalog-project for deploy within apache folder </br>
8. Made additional necessary changes in file catalog.conf to handle connections. </br>
9. Made a python virtual environment for running python code. </br>

### Locate the SSH key created for the grader key ###

The grader key is located in the .ssh folder within the home folder of grader
