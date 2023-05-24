#LEMP STACK IMPLEMENTATION

##Web stack implementation (lamp stack) in aws

We have to connect to our AWS account and launch and connect to our EC2 instance.
![Connect to AWS and run ssh on Gitbash](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/ed499301-1667-4427-a436-9fc0718fde7e)

**Step 1 – installing the nginx web server**
In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web server. We’ll use the apt package manager to install this package.
Since this is our first time using apt for this session, start off by updating your server’s package index. Following that, you can use apt install to get Nginx installed:
Run the following command:
1. sudo apt update
2. sudo apt install nginx
3. sudo systemctl status nginx
If it is green and running, then you did everything correctly – you have just launched nginx Web Server in the Clouds!
![nginx active and running](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/654c61d0-a6dd-4e18-a01c-51f1b62cdf04)
To test how our Nginx server can respond to requests from the Internet. Open a web browser of your choice and copy and paste the public Ipv4 DNS
to the page below.
![Welcome to nginx page](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/8a836c5a-eb45-4442-8c6b-faaedc25e392)

**Step 2 — installing mysql**
Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.
Again, we will use 'apt' to acquire and install this software.
Run the following commands
1. sudo apt install mysql-server
2. sudo mysql
t’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. Before running the script you will set a password for the root user, using mysql_native_password as default authentication method. We’re defining this user’s password as PassWord.1.
3. ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
Exit the Mysql shell with the command 4.
4. mysql> exit
To access your mysql you have to make use of your new generated password, else you will not be able to gain access into your 'mysql'. If you run the coommand "sudo mysql" it will deny you accesss and sho this message  "Access denied for user 'root'@'localhost' (using password: NO)" This lead us to our next command.
5. sudo mysql -p
It will request for your password, you should enter the new genarated password and whola! you are in!

