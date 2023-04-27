# ProjectLAMP
LAMP STACK IMPLEMENTATION
This is a project which implement the use of Web Stack (LAMP STACK) In AWS

What is a Technology stack?

A technology stack is a set of frameworks and tools used to develop a software product. This set of frameworks and tools are very specifically chosen to work together in creating a well-functioning software. They are acronymns for individual technologies used together for a specific technology product. some examples are…

LAMP (Linux, Apache, MySQL, PHP or Python, or Perl) LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl) MERN (MongoDB, ExpressJS, ReactJS, NodeJS) MEAN (MongoDB, ExpressJS, AngularJS, NodeJS

Create a EC2 Instance

Select region (the cl and launch a new EC2 instance of t2.micro family with Ubuntu Server 20.04 LTS (HVM)

Create a Key Pair as the EC2 is created

Move into the folder where the pair key is downloaded and run the following command to connect to the instance
        
        sudo chmod 0400 <private-key-name>.pem
    
        ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>
        
        Install APACHE and Update the Firewall

First is to update a list of packages in package manager, the following command is used

               sudo apt update
Then we run apache2 package installation using the following command

               sudo apt install apache2
To verify that apache2 is running as a Service in our OS, use following command

              sudo systemctl status apache2
we can try to check how we can access it locally in our Ubuntu shell, using the following command

               curl http://localhost:80
  
                             or
              
                curl http://127.0.0.1:80
The image below will be display with the ip address

<img width="801" alt="ApacheUbuntuDefault" src="https://user-images.githubusercontent.com/61475969/204667289-cc408c65-7a63-49f7-a365-3a52e625d2d9.png">

Installing MYSQL

Use the the command line to install the server

               sudo apt install mysql-server
We can the server using the following command line

               sudo mysql_secure_installation
You can sudo in the the server using the command

                      sudo mysql
This will be display which shows that it is successful
<img width="559" alt="terminalsc" src="https://user-images.githubusercontent.com/61475969/204668676-ff5b6f6d-8956-4b6c-a32b-35054320b5c8.png">

We can exit the server using the following command

                             exit
Installing PHP

We use the following command to install 3 packages at once

                      sudo apt install php libapache2-mod-php php-mysql
After the installation, we can run the check the php version with the following command

                                           php -v
Creating Virtual Host Using APACHE

Create the directory for projectlamp using ‘mkdir’ using the command

                             sudo mkdir /var/www/projectlamp
Assign ownership of the directory with current system user using the command

                            sudo chown -R $USER:$USER /var/www/projectlamp
Create and open a new configuration file in Apache’s usinng the command line

                            sudo vi /etc/apache2/sites-available/projectlamp.conf
paste the command below in the vim file and save

                                  <VirtualHost *:80>
                                  ServerName projectlamp
                                  ServerAlias www.projectlamp 
                                  ServerAdmin webmaster@localhost
                                  DocumentRoot /var/www/projectlamp
                                  ErrorLog ${APACHE_LOG_DIR}/error.log
                                  CustomLog ${APACHE_LOG_DIR}/access.log combined
                                  </VirtualHost>
You can list us the command line below tp list what is in the file

                             sudo ls /etc/apache2/sites-available
Enable new virtual host using the command

                                    sudo a2ensite projectlamp
You can disable Apache default website using the command

                                    sudo a2dissite 000-default
Check for syntax error in the configuration file with the command

                                    sudo apache2ctl configtest
Reload Apache for the changes to take effect

                                    sudo systemctl reload apache2
Create an index.html file in that location to test that the virtual host works as expected using the command

 sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s                           http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
The website URL can be access using thr IP address

                                    http://<Public-IP-Address>:80
Enable PHP on the website

Edit the conf file with the command

                             sudo vim /etc/apache2/mods-enabled/dir.conf
Paste in the command

                       <IfModule mod_dir.c>
                       #Change this:
                       #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
                       #To this:
                       DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
                       </IfModule>
Reload Apache

                             sudo systemctl reload apache2
Create index.php file

                             vim /var/www/projectlamp/index.php
Add the following text, which is valid PHP code, inside the file:

                                         <?php
                                        phpinfo();
Save and close the file, refresh the page and you will see a page similar to this:
<img width="986" alt="Screenshot 2022-12-01 at 18 37 14" src="https://user-images.githubusercontent.com/61475969/205133377-5740deab-1958-49c1-a944-09512e0e4f1c.png">

To remove the file created, you can run following command

                             sudo rm /var/www/projectlamp/index.php

