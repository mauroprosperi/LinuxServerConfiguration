# LinuxServerConfiguration

You will take a baseline installation of a Linux server and prepare it to host your web applications. You will secure your server from a number of attack vectors, install and configure a database server, and deploy one of your existing web applications onto it.

# Why this Project?

A deep understanding of exactly what your web applications are doing, how they are hosted, and the interactions between multiple systems are what define you as a Full Stack Web Developer. In this project, you’ll be responsible for turning brand-new, bare bones, Linux server into the secure and efficient web application host your applications need.


## Instruction


### Initialize your server on [Amazon Lightsail][1]

[1]:https://aws.amazon.com/es/lightsail/

  
 - Step by step:
 
    - Log in
    - Create an instance
    - Choose an instance image: Ubuntu
    - Follow the instructions provide to SSH into your server.
    
  
### Security

 * Update all currently installed packcages (this could take a while)
 ~~~
 sudo apt-get update
 sudo apt-get upgrade
 ~~~
 
 * Configure the Uncomplicated Firewall (UFW)
 ~~~
  sudo ufw status                     <-- to check firewall status
  sudo ufw allow 2200/tcp
  sudo ufw allow 80/tcp
  sudo ufw allow 123/tcp
  sudo ufw enable
 ~~~
 
 * Change the ssh port from 22 to 2200 and change `permitRootLogin` from `without-password` to `no`

 ~~~
 sudo nano /etc/ssh/sshd_config
 ~~~
 
### User configuration
 
 * Create a new user name grader in order to the project be reviewed.
 ~~~
 sudo add user grader
 ~~~
 
 * Give grader the permission to sudo.
 ~~~
 sudo nano /etc/sudoers.d/grader
 ~~~
   and inside this file add the text, then save.
 ~~~
 grader ALL=(ALL) NOPASSWD:ALL
 ~~~
 
 * Configura timezone to UTC
 ~~~
 sudo dpkg-reconfigure tzdata
 ~~~
 
 create a SSH key pair for grader using the ssh-keygen tool.
 ~~~
 ssh-keygen -t rsa
 ~~~

now restart ssh again with `sudo service ssh restart`

* You can now login as `ssh -i .ssh/grader grader@34.210.194.203 -p 2200`

### Install and configure

* Ngnix

We use ngnix instead of Apache, configuration steps can be found here

[ngix tutorial][2]

[2]:https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04

* Postgresql
`sudo apt-get install postgresql`

* Remove remote connections

Create a new database with limited permission on your catalog application dabase.

` https://help.ubuntu.com/community/PostgreSQL`

or

STEP BY STEP COMMING SOON..

### Deploy the catalog project.

ip soon
