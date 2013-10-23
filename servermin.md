Server Administration
=====================

## Mishellaneous

The **VirtualBox** machine needs a **Bridged Adapter** network configuration to be able to be reached from outside!








## Common Tasks

### shutdown / reboot

    sudo shutdown -h now
    sudo reboot

### check server ip address

    ifconfig eth0 | grep inet | awk '{ print $2 }'
    
### remove apt package

    sudo aptitude remove vsftpd

### create / remove system users

    # create
    sudo useradd <username>
    
the process will prompt for user's password and other optionals informations which you can leave blank.
    
    # remove
    sudo deluser <username>
        
**NOTE:** remove user's account will not remove the user's home folder!

### enable / disable users

for security reasons you may want to disable some users when they ar not meant to perform actions on the system (es FTP users).

    # disable
    sudo passwd -l <username>
    
    # enable
    sudo passwd -u <username>
    
### create / remove groups

    # create
    sudo addgroup <groupname>
    
    # remove
    sudo delgroup <groupname>
    
### zip / unzip

    sudo apt-get install unzip
    unzip <archive.zip>







## SSH

This steps guide to enabling an **SSH Access** to manage the computer remotely.

    # install SSH server
    sudo apt-get install openssh-server
    
Now you need to edit the default settings to fit your needs:

    # edit SSH configuration
    sudo vi /etc/ssh/sshd_config
    
After editing the file you need to restart SSH server in order to apply changes:

    # restart SSH:
    sudo /etc/init.d/ssh restart
    # or:
    sudo reload ssh

### Connect to SSH Server
    
In order to connect to SSH server from outside you need to know **the remote user** you will use and the **remote SSH DNS** name or IP **and port**.


    # connect
    ssh username@hostname.com
    ssh username@192.168.0.1
    
    # connect with specific port
    ssh -u username -p 2222 hostname.com
    ssh user@host.com -p 2222
    
    # close connection
    logout
    








## Apache
    
    # download and install:
    sudo apt-get apache2

### restart
    
    sudo service apache2 restart
    
### enable mod_rewrite

    sudo a2enmod rewrite

### Create New VirtualHost

Create new config file `/etc/apache2/sites-available/domain.com` to store the virtual host configuration.

    <VirtualHost *:80>
        ServerAdmin webmaster@localhost
        ServerName domain.com
        ServerAlias www.domain.com

        DocumentRoot /var/www/domain.com/public_html/
        <Directory /var/www/domain.com/public_html/>
                Options FollowSymLinks MultiViews
                AllowOverride All
                Order allow,deny
                allow from all
        </Directory>
    </VirtualHost>

To add the new _VirtualHost_ to _Apache_ run:

    sudo a2ensite domain.com

then restart apache.



    
## MySQL

    # download and install:
    sudo apt-get install mysql-server libapache2-mod-auth-mysql php5-mysql
    
during the installation process you will be prompted to setting the `root password`.   
_I set up `qaz`as the system root!_

    # activate mysql
    sudo mysql_install_db
    
and then run the initialization script for secure the MySql server:
    
    # step by step guide to secure the server
    sudo /usr/bin/mysql_secure_installation

### Access the Shell

    mysql -u root -p
    quit;

### New database account

    # database
    create database dbname;
    
    # user
    create user 'username' identified by 'password'
    
    # grant access
    grant all privileges on db_name.* to username
    
    flush privileges;








## PHP

    # download and install php
    sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt

after _PHP_ installation you may check _Apache_ configuration to search for **index.php** files as folder root:

    sudo vi /etc/apache2/mods-enabled/dir.conf
    
    
### install GD library

    apt-get install php5-gd

then restart apache
    
    
    
    
    






## FTP Server
    
    # install "Very Secure FTP Daemon"
    sudo apt-get install vsftpd

### configuration file

    sudo vi /etc/vsftpd.conf
    
    









## References

### Initial Server Setup

[https://www.digitalocean.com/community/articles/initial-server-setup-with-ubuntu-12-04]()  

### Configuring LAMP

[https://www.digitalocean.com/community/articles/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu]()

#### Possible SSH Hosting

[https://www.digitalocean.com/pricing]()

#### MySql Administration

[https://www.digitalocean.com/community/articles/a-basic-mysql-tutorial]()

#### Add new virtualhost

[https://www.digitalocean.com/community/articles/how-to-set-up-apache-virtual-hosts-on-ubuntu-12-04-lts]()
