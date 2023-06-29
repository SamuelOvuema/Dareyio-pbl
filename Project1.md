# LAMP STACK IMPLEMENTATION

## STEP 1 — INSTALLING APACHE AND UPDATING THE FIREWALL

What exactly is Apache?

Apache HTTP Server is the most widely used web server software. Developed and maintained by Apache Software Foundation, Apache is an open-source software available for free. It runs on 67% of all web servers in the world. It is fast, reliable, and secure. It can be highly customized to meet the needs of many different environments by using extensions and modules. Most WordPress hosting providers use Apache as their web server software. However, websites and other applications can run on other web server software as well. Such as Nginx, Microsoft’s IIS, etc.

The Apache web server is among the most popular web servers in the world. It’s well documented, has an active community of users, and has been in wide use for much of the history of the web, which makes it a great default choice for hosting a website.

Install Apache using Ubuntu’s package manager ‘apt’:

#update a list of packages in the package manager
```bash
sudo apt update
```

#run apache2 package installation
```bash
sudo apt install apache2
```

To verify that apache2 is running as a Service in our OS, use following command
```bash
sudo systemctl status apache2
```
If it is green and running, then you did everything correctly – you have just launched your first Web Server in the Clouds!

![sudo systemctl status apache2](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/41cc1cd7-0037-43ca-b739-b32cb0ef6182)

Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web browsers use to access web pages on the Internet

As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to open inbound connection through port 80:

![port 80](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/cec62369-8d32-418b-9670-2566dee3f67e)

Our server is running and we can access it locally and from the Internet (Source 0.0.0.0/0 means ‘from any IP address’).

First, let us try to check how we can access it locally in our Ubuntu shell, run:
```bash
 curl http://localhost:80
or
 curl http://127.0.0.1:80
```

![terminal ubuntu works](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/b892e470-4181-4b11-a275-b2ee7e99a78f)

These 2 commands above actually do pretty much the same – they use ‘curl’ command to request our Apache HTTP Server on port 80 (actually you can even try to not specify any port – it will work anyway). The difference is that: in the first case we try to access our server via DNS name and in the second one – by IP address (in this case IP address 127.0.0.1 corresponds to DNS name ‘localhost’ and the process of converting a DNS name to IP address is called “resolution”). We will touch DNS in further lectures and projects.

As an output you can see some strangely formatted test, do not worry, we just made sure that our Apache web service responds to ‘curl’ command with some payload.

Now it is time for us to test how our Apache HTTP server can respond to requests from the Internet.
Open a web browser of your choice and try to access following url

```bash
http://<Public-IP-Address>:80
```


Another way to retrieve your Public IP address, other than to check it in the AWS Web console, is to use the following command:
```bash
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
The URL in the browser shall also work if you do not specify a port number since all web browsers use port 80 by default.

If you see the following page, then your web server is now correctly installed and accessible through your firewall.

![Apache2 Default page](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/50d9218b-02fd-4e04-81ae-3a0f499548b3)

In fact, it is the same content you previously got by the ‘curl’ command but represented in nice HTML formatting by your web browser.

## STEP 2 — INSTALLING MYSQL

Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.

Again, use ‘apt’ to acquire and install this software:
```bash
sudo apt install mysql-server
```

When prompted, confirm installation by typing Y, and then ENTER.

When the installation is finished, log in to the MySQL console by typing:
```bash
sudo mysql
```

This will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command. You should see output like this:


































