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
    - Choose your instance plan (any tier works)
    - Give your instance a hostname
    - Wait for startup, then follow the instructions provide to SSH into your server.
    
  
### Security

 * Update all currently installed packcages (this could take a while)
 ~~~
 sudo apt-get update
 sudo apt-get upgrade
 ~~~
 
 * Configure the Uncomplicated Firewall (UFW)
 ~~~
  sudo ufw status                     <-- to check firewall status
  sudo ufw allow 22/tpc
  sudo ufw allow 2222/tpc
  sudo ufw allow 2200/tcp
  sudo ufw allow 80/tcp
  sudo ufw allow 123/tcp
  sudo ufw enable
 ~~~
 
 * Change the ssh port from 22 to 2200 and change `permitRootLogin` from `without-password` to `no`

 ~~~
 sudo nano /etc/ssh/sshd_config
 ~~~
 now restart ssh with `sudo service ssh restart`
 
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
 
 ***Outside of our virtual machine*** create a SSH key pair for grader using the ssh-keygen tool.
 ~~~
 ssh-keygen -t rsa
 ~~~
 
 ***again on your virtual machine*** create the files to storage the key
 ~~~
 cd ~                          <-- to put our command line on root
 mkdir .ssh
 touch .ssh/authorized_keys
 nano .ssh/authorized_keys     <-- copy the public key generated on your local machine here, all the content.
 ~~~
 
 * Change permissions
 ~~~
 chmod 700 .ssh
 chmod 644 .ssh/authorized_keys
 ~~~
now restart ssh again with `sudo service ssh restart`

* You can now login as `ssh grader@127.0.0.1 -p 2222 -i ~/.ssh/linuxCourse`

### Install and configure

1_ Apache
~~~
sudo apt-get install apache2   <-- check your localhost:8080 to see the apache ubuntu default page
~~~

2_ mod_wsgi
~~~
sudo apt-get install libapache2-mod-wsgi
~~~

* Configure apache to handle request using WSGI module
~~~
sudo nano /etc/apache2/sites-enabled/000-default.conf
~~~

* For now, add the following line at the end of the `<VirtualHost *:80>` block, right before the closing `</VirtualHost>` line: `WSGIScriptAlias / /var/www/html/myapp.wsgi`

* restart Apache with `sudo apache2ctl restart` ( you might get a warning, ignore for now)

* Test if you have your Apache configuration right create a wsgi file using `sudo nano /var/www/html/myapp.wsgi` and write:
~~~
def application(environ, start_response):
    status = '200 OK'
    output = 'Hello World!'

    response_headers = [('Content-type', 'text/plain'), ('Content-Length', str(len(output)))]
    start_response(status, response_headers)

    return [output]
~~~
 ***WARNING*** maybe you will need to delete the content from the ubuntu apache2 default page to see the hello world!, edit it with the command `sudo nano /var/www/html/index.html`
