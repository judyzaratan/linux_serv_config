## Linux Server Configuration ##
___
### Description ###
A Linux server that is setup to securely serve a web application.  


IP Address: 54.186.146.96
SSH Port: 2200
URL http://ec2-54-186-146-96.us-west-2.compute.amazonaws.com/

### Configuration Steps ###

#### Configure Ubuntu server ###
* Create an Ubuntu Linux server instance on Amazon Lightsail.

* SSH into the server via local terminal
  1. Download ssh private key supplied by AWS and save file in .ssh folder in home directory
  2. Enter following command to remotely log into instance
  ```
  ssh -v -i ~/.ssh/[PRIVATE KEY FILENAME HERE] ubuntu@54.186.146.96 -p 22
  ```
  *default ssh port 22 will be later disabled and will only be used for initial configuration purposes* 
* Perform update and upgrade default Ubuntu server packages.  
  ```
  $ sudo apt-get update
  ...[performs update]
  $ sudo apt-get upgrade
  ...[performs upgrade]
```

Software installed
python
finger
ssh-keygen
apache2
libapache2-mod-wsgi
postgresql


Third Party resources
Deploy WGSI application on Ubuntu
https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
Setting Secret Key
https://stackoverflow.com/questions/26080872/secret-key-not-set-in-flask-session
