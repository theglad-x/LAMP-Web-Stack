## Setting up a LAMP Stack on an Amazon EC2 Instance
 
 LAMP stack is a combination of Linux, Apache, Mysql and PHP for software development. It provides a robust, scalable, and flexible environment for developing web applications. Here's how each component works:
- **Linux**: serves as the platform or the operating system.
- **Apache**: handles requests and serves the web pages.
- **MySQL**: store and manage the data used by the application, such as user information.
- **PHP**: processes the data, interacting with the database to deliver dynamic web content.

## Setting up AWS Ec2 instance
 1. Create an AWS account
 2. Navigate to Ec2 Dashboard
 3. Launch an Ec2 instance 
 4. Give the instance a name
 5. Choose OS image
 6. Choose instance type
 7. Create keypair and download it 
 8. Create a security group, allow inbound traffic on ports 
  -   HTTP (port 80)
  -   HTTPS (port 443)
  -   SSH (port 22) from the host machine ip
 9. Configure other settings according to your requirement
  
##  Review and Launch
1. Review your configuration and proceed to launch the instance.

2. On the terminal Change permission for the key pair
  ``` bash
  sudo chmod 400 path/to/your/keypair.pem
```
3. Connect to the instance by running
``` bash
ssh -i path/to/your/keypair.pem ubuntu@public address
```
 ![Screenshot (338)](https://github.com/user-attachments/assets/d95b0f45-045c-4670-ac9b-89a68b57084d)
The instance is connected

## Update package index install LAMP Stack tools
1. update package
 ``` bash
Sudo apt update -y
```
2. Install Apache
```bash
sudo apt install apache2
```
3. Enable and Verify Apache
``` bash
sudo systemctl enable apache2
```
4. Check the status, to verify that Apache is running
```bash
sudo systemctl status apache2
```
5. Test Apache
  Open a web browser to access Apache with the public IP of the instance
``` bash
http://<Public-IP-Address>
```
![Screenshot (339)](https://github.com/user-attachments/assets/50dfa18a-738c-4af8-a6d3-c6bd8dfad2f9)

6. Install MySQL
``` bash
sudo apt install mysql-server
```
7. Test MySQL
- Secure the MySQL installation
```bash
sudo mysql_secure_installation
```
  Login in to mysql console
```bash
sudo mysql
```
  Configure login password
```bash
ALTER USER ‘root’@’localhost’ IDENTIFIED WITH mysql_native_password BY ‘<your password>’;
```

  Exit the mysql shell
```bash
exit
```

  To login to mysql shell after setting the password
```bash 
sudo mysql -p
```
Enter the password created

8. Install PHP
  Install PHP and necessary modules
```bash
sudo apt install php libapache2-mod-php php-mysql
```


## Creating a virtual host for the website using Apache

1. Create project directory
```bash
sudo mkdir /var/www/projectlamp
```
2. Change ownership of the directory to the current user
```bash
sudo chown -R $USER:$USER /var/www/projectlamp
```
3. Configuration file
- Create new configuration file in Apache's sites-available directory
```bash
sudo nano /etc/apache2/sites-available/projectlamb.conf
```
Add the following configuration
```bash
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName projectlamb
    ServerAlias www.projectlamb
    DocumentRoot /var/www/projectlamb
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
4. Enable the new created virtual host 
```bash
sudo a2ensite projectlamp
```
5. Disable the default Apache’s website 
```bash
sudo a2dissite 000-default
```
6. Ensure no syntax error
```bash
sudo apache2ctl configtest
```
7. Reload Apache
```bash
sudo systemctl reload apache2
```
8. Test the virtual host 
```bash
echo "<h1>Welcome, this is a LAMP web stack</h1>" > /var/www/projectlamb/index.html
```
Access the website in the browser
```bash
http://<Public-IP-Address>:80
```
![Screenshot (340)](https://github.com/user-attachments/assets/876141cc-1ccd-4a5d-8d49-0f62fa45226d)

## Enabling PHP on the Website
1. Modify Directory Index
 Open in a text editor
```bash
sudo nano /etc/apache2/mods-enabled/dir.conf
```
Change the order in which the files are listed, index.php to the first position
``` bash
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>` 
```
2. Reload Apache to apply changes
```bash
sudo systemctl reload apache2
```
3. PHP Test Script
Create a file index.php file in the web root folder
```bash
echo "<?php phpinfo(); ?>" > /var/www/projectlamp/index.php
```
## Test PHP Installation
Refresh the browser and verify that PHP is working. Apache is able to handle and process requests for PHP files
![Screenshot (341)](https://github.com/user-attachments/assets/888b8c3d-4679-43c1-b4a5-1e5c38a207b2)

## Conclusion
This project provided me with practical experience and a deeper understanding of web hosting and server management, which is crucial for handling more complex projects. I successfully set up a LAMP stack on an AWS EC2 instance, configuring Apache to serve a website, installing MySQL for database management, and integrating PHP for dynamic content handling. Additionally, I set up a virtual host for the web project and verified that Apache can process PHP files. This foundational experience with the LAMP stack equips me to build and deploy dynamic web applications, enabling efficient hosting and management of websites on the cloud using AWS infrastructure.
