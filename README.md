## Linux Server Configuration ##
___
### Description ###
A Linux server that is setup to securely serve a web application.  


IP Address: 54.186.146.96
SSH Port: 2200
URL http://ec2-54-186-146-96.us-west-2.compute.amazonaws.com/

### Configuration Steps ###
1. Create an Ubuntu server instance on Amazon Lightsail

2. Configure ports on Lightsail side to accept following ports for usage
  - 2200 for ssh
  - 80 for http
  - 123 for ntp

3. SSH into server via *local* terminal
  - Download ssh key provided by AWS
  - Store into ~/.ssh local folder
    `ssh -v -i ~/.ssh/LightsailDefaultPrivateKey-us-west-2.pem ubuntu@54.186.146.96 -p 22`

4. Secure server with UFW (Uncomplicated Firewall).  The default setting is 'inactive'
   ```
   $ sudo ufw status
   $ sudo ufw allow 2200/tcp
   $ sudo ufw allow www
   $ sudo ufw allow ntp
   $ sudo ufw enable
   ```

5. Create a user and give sudo access
   - Create user

     `$ sudo adduser grader`

   - Add user for sudo access by creating /etc/sudoers.d/grader file with permissions setting

     `$ sudo nano /etc/sudoers.d/grader`

   - Append following information into the /etc/sudoers.d/grader file

     `grader	ALL=(ALL) NOPASSWD:ALL`

   - Create a ssh keypair using ssh-keygen on local terminal

      - Store *public* key on grader user account on Ubuntu server in the following file ~/.ssh/authorized_keys

        - Set permissions on directory

        `$ chmod 700 .ssh`

        - Set permissions on file

        `$ chmod 644 .ssh/authorized_keys`

     Store public/private key in *local* terminal from ssh-keygen in ~/.ssh directory

     ~/.ssh/[filename_public].pub

     ~/.ssh/[filename_private]

6.  Change SSH config settings on Ubuntu server

    - Access file /etc/ssh/sshd_config

    - Change PasswordAuthentication to no

    - Change port access from 22 to 2200

    - Change PermitRootLogin to no

    - Restart service

      `$ sudo service ssh restart`

7.  Logins are only accepted on port 2200 and 22 is now disabled.  

8. Configure Postgres to have a user catalog and limited permissions
    - Login as default postgres user
    ```
    $ sudo su postgres
    $ psql
    psql -> createuser catalog with password 'catalog';
    ```

9.  Configure wgsi-mod to use repository

    - clone repository into /var/www.
    - Create a .wgsi file to direct wgsi-mod to utilize app file in repository.  
    - Update /etc/apache2/sites-enabled/000-default.conf file to allow Apache communicate with wgsi-mod
        Add following lines
         - DocumentRoot /var/www/[respository_folder]
         - WSGIScriptAlias / /var/www/[repository_folder]/[project_name].wgsi

7. Update respository  to connect correctly within database.  

8. Update client_secrets.json with additional hostname from Ubuntu server instance and file location references

9.  Update Google Web App Signin console with additional signin.  

### Dependencies ###

* finger
* ssh-keygen
* apache2
* libapache2-mod-wsgi
* postgresql
* git

#### Web app dependencies installed ####
* python
* python-pip
* sqlalchemy
* flask
* oauth2client
* requests
* psycopg2


### Third Party resources ###
Deploy WGSI application on Ubuntu

https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps

Setting Secret Key

https://stackoverflow.com/questions/26080872/secret-key-not-set-in-flask-session

Configuring Linux Web Servers

https://www.udacity.com/course/configuring-linux-web-servers--ud299

Client_Secrets
https://discussions.udacity.com/t/unable-to-log-in-after-updating-client-secrets-json/249418/6
