                                                                                                                                                                                                                                                                    
                     # LEMP STACK IM

## Web stack implementation (lamp stack) in aws

We have to connect to our AWS account and launch and connect to our EC2 instance.

![Connect to AWS and run ssh on Gitbash](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/ed499301-1667-4427-a436-9fc0718fde7e)

**Step 1 – installing the nginx web server**
In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web server. We’ll use the apt package manager to install this package.
Since this is our first time using apt for this session, start off by updating your server’s package index. Following that, you can use apt install to get Nginx installed.
Run the following command:
```bash
sudo apt update.
```
![Installing mysql -server](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/ff126a48-fa85-450e-b9cd-f87440c2e2e1)
```bash
sudo apt install nginx
```
```bash
sudo systemctl status nginx.
```
If it is green and running, then you did everything correctly – you have just launched nginx Web Server in the Clouds!

![nginx active and running](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/654c61d0-a6dd-4e18-a01c-51f1b62cdf04)

To test how our Nginx server can respond to requests from the Internet. Open a web browser of your choice and copy and paste the public Ipv4 DNS
to the page below.

![Welcome to nginx page](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/8a836c5a-eb45-4442-8c6b-faaedc25e392)

**Step 2 — installing mysql**
Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.
Again, we will use 'apt' to acquire and install this software.
Run the following commands:.
```bash
sudo apt install mysql-server
```
```bash
sudo mysql
```

This will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command. You should see output like this:

![sudo mysql](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/26687079-9133-4154-8f6b-d2e43db2c246)

It’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. Before running the script you will set a password for the root user, using mysql_native_password as default authentication method. We’re defining this user’s password as PassWord.1.
```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```
Exit the Mysql shell with:
```bash
> mysql> exit
```
Start the interactive script by running:
```bash
sudo mysql_secure_installation
```
![mysql secure installation](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/f378869a-f180-41ea-8d9c-bc245acaab62)

To access your mysql you have to make use of your new generated password, else you will not be able to gain access into your 'mysql'. If you run the coommand "sudo mysql" it will deny you accesss and show this message  "Access denied for user 'root'@'localhost' (using password: NO)" This lead us to our next command.
```bash
sudo mysql -p 
```
It will request for your password, you should enter the new genarated password and you are in!

To exit the MySQL console, type: exit.
```bash
mysql> exit
```

![Creating password for mysql server](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/80a4b81a-2615-4c11-94e1-573d9e6fbe2c)

MySQL server is now installed and secured. Next, we will install PHP, the final component in the LEMP stack.

__Step 3 — installing php__

You have Nginx installed to serve your content and MySQL installed to store and manage your data. Now you can install PHP to process code and generate dynamic content for the web server.

You’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.

To install these 2 packages at once, run:.
```bash
sudo apt install php-fpm php-mysql
```
When prompted, type Y and press ENTER to confirm installation.

![installing php-fpm   php-mysql](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/3cef93a1-b6d3-4116-a1c7-bcc54913bdc3)

check the version by running: php -v.

![checking php version](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/024bd8b0-2e48-46f4-9f77-eb313d3b8586)

You now have your PHP components installed. Next, you will configure Nginx to use them.

**Step 4 — Configuring Nginx to Use PHP Processor**

When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use projectLEMP as an example domain name.

On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for the your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.

Create the root web directory for your_domain as follows:
```bash
sudo mkdir /var/www/projectLEMP
```
Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user:
```bash
sudo chown -R $USER:$USER /var/www/projectLEMP
```
Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:
```bash
sudo nano /etc/nginx/sites-available/projectLEMP
```
This will create a new blank file. Paste in the following bare-bones configuration:
```bash
#/etc/nginx/sites-available/projectLEMP
server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    } 
}
```
![sudo nana etcnginx](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/9053dea9-9f03-43ed-83c5-e530dc7b25f2)

When you’re done editing, save and close the file. If you’re using nano, you can do so by typing CTRL+X and then y and ENTER to confirm.

Activate your configuration by linking to the config file from Nginx’s sites-enabled directory:
```bash
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
This will tell Nginx to use the configuration next time it is reloaded. You can test your configuration for syntax errors by typing:.
```bash
sudo nginx -t
```
You shall see following message:
```bash
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:
```bash
sudo unlink /etc/nginx/sites-enabled/default
```
When you are ready, reload Nginx to apply the changes:
```bash
sudo systemctl reload nginx
```
Your new website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that your new server block works as expected:
```bash
sudo echo 'Hello LEMP from hostname' $(curl -s 
http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' 
$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) >
/var/www/projectLEMP/index.html
```
Now go to your browser and try to open your website URL using IP address:
```bash
http://<Public-IP-Address>:80.
```
If you see the text from ‘echo’ command you wrote to index.html file, then it means your Nginx site is working as expected.
In the output you will see your server’s public hostname (DNS name) and public IP address. You can also access your website in your browser 
by public DNS name, not only by IP – try it out, the result must be the same (port is optional).
```bash
http://<Public-DNS-Name>:80.
```    
You can leave this file in place as a temporary landing page for your application until you set up an index.php file to replace it. Once you do that, remember to remove or rename the index.html file from your document root, as it would take precedence over an index.php file by default.

Your LEMP stack is now fully configured. 

![configure Nginx to use php processor](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/fee3e9be-ec23-4cdd-be2a-684e9cd89f26)

In the next step, we’ll create a PHP script to test that Nginx is in fact able to handle .php files within your newly configured website.

__Step 5 – Testing PHP with Nginx__

Your LEMP stack should now be completely set up.

At this point, your LAMP stack is completely installed and fully operational.

You can test it to validate that Nginx can correctly hand .php files off to your PHP processor.

You can do this by creating a test PHP file in your document root. Open a new file called info.php within your document root in your text editor:
```bash   
sudo nano /var/www/projectLEMP/info.php.
```   
Type or paste the following lines into the new file. This is valid PHP code that will return information about your server:
```bash
<?php
phpinfo();.
```
You can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php:
```bash
http://`server_domain_or_IP`/info.php
```
You will see a web page containing detailed information about your server:

![php info page](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/15521212-fec1-4f09-a475-8cd76dc8314b)

After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file:  
```bash
sudo rm /var/www/your_domain/info.php
```
![removal of file](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/5e70acb8-0da8-42d0-b700-a15586576d45)

You can always regenerate this file if you need it later.

__Step 6 — Retrieving data from MySQL database with PHP__

In this step you will create a test database (DB) with simple “To do list” and configure access to it, so the Nginx website would be able to query data from the DB and display it.

At the time of this writing, the native MySQL PHP library mysqlnd doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. We’ll need to create a new user with the mysql_native_password authentication method in order to be able to connect to the MySQL database from PHP.

We will create a database named **example_database** and a user named **example_user**, but you can replace these names with different values.

First, connect to the MySQL console using the root account:
```bash
sudo mysql
```
To create a new database, run the following command from your MySQL console:
```bash
mysql> CREATE DATABASE `example_database`;
```
Now you can create a new user and grant him full privileges on the database you have just created.

The following command creates a new user named example_user, using mysql_native_password as default authentication method. We’re defining this user’s password as password, but you should replace this value with a secure password of your own choosing.
```bash
mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
```
Now we need to give this user permission over the example_database database:
```bash
mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';
```
This will give the example_user user full privileges over the example_database database, while preventing this user from creating or modifying other databases on your server.

Now exit the MySQL shell with:
```bash
mysql> exit
```
You can test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:
```bash
mysql -u example_user -p
```

![mysqluexample](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/fbc50cc2-1f96-46f3-905a-358eefd1a1bf)

Notice the -p flag in this command, which will prompt you for the password used when creating the example_user user. After logging in to the MySQL console, confirm that you have access to the example_database database:
```bash
mysql> SHOW DATABASES;
```
This will give you the following output:  
  

![Database](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/806d47c8-5549-45c6-8fe7-3a2b62a58e48)


Next, we’ll create a test table named todo_list. From the MySQL console, run the following statement:

```bash
CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content
VARCHAR(255),PRIMARY KEY(item_id));
```
Insert a few rows of content in the test table. You might want to repeat the next command a few times, using different VALUES:
```bash
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first
important item");
```
To confirm that the data was successfully saved to your table, run:
```bash
mysql>  SELECT * FROM example_database.todo_list;
```
You’ll see the following output:



After confirming that you have valid data in your test table, you can exit the MySQL console:
```bash
mysql> exit
```
Now you can create a PHP script that will connect to MySQL and query for your content. Create a new PHP file in your custom web root directory using your preferred editor. We’ll use vi for that:
```bash
nano /var/www/projectLEMP/todo_list.php
```    
The following PHP script connects to the MySQL database and queries for the content of the todo_list table, displays the results in a list. If there is a problem with the database connection, it will throw an exception.

Copy this content into your todo_list.php script:

![php todolist](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/40ebf155-d9c4-4522-ab08-0e44efe474fc)

Save and close the file when you are done editing.

You can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php:
```bash
http://<Public_domain_or_IP>/todo_list.php
```
![php todoscript](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/6c33c9de-3026-42af-a360-dee46f29f7f5)

You should see a page like this, showing the content you’ve inserted in your test table:

![php interaction with Mysql](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/eaec7e19-b54a-4d3f-a116-bccebc909b05)

That means your PHP environment is ready to connect and interact with your MySQL server.






















