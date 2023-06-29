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



Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web browsers use to access web pages on the Internet

As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to open inbound connection through port 80:






















































