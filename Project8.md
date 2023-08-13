 # Load Balancer Solution With Apache

## Configure Apache As A Load Balancer

1. Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb, so your EC2 list will look like this:

![LB EC2 instance](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/2bceae4f-ea75-4d07-9ad2-1db22ab426b0)

2. Open TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in the Security Group.

![LB port 80 inbound rule](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/fceb7c43-64b8-4d73-99e9-8a04ee023a4b)

3. Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:

#Install apache2
```bash
sudo apt update
sudo apt install apache2 -y
sudo apt-get install libxml2-dev
```
![Apache2 status](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/b30999c0-5e88-4196-92e4-53d07c395b39)


#Enable the following modules:

```bash
sudo a2enmod rewrite
sudo a2enmod proxy
sudo a2enmod proxy_balancer
sudo a2enmod proxy_http
sudo a2enmod headers
sudo a2enmod lbmethod_bytraffic
```

#Restart apache2 service

```bash
sudo systemctl restart apache2
Make sure apache2 is up and running
```
```bash
sudo systemctl status apache2
```

![sudo systemctl status apache2](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/b1a22e18-85b6-45cd-9743-c201c304c0f0)

Configure load balancing
```bash
sudo vi /etc/apache2/sites-available/000-default.conf
```
#Add this configuration into this section <VirtualHost *:80>  </VirtualHost>

![configured load balancer](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/b9baf8e3-c7e4-4d42-acc9-846c571c8b56)


#Restart Apache server

```bash
sudo systemctl restart apache2
```

[bytraffic](https://httpd.apache.org/docs/2.4/mod/mod_lbmethod_bytraffic.html) balancing method will distribute incoming load between your Web Servers according to current traffic load. We can control in which proportion the traffic must be distributed by the load factor parameter.

You can also study and try other methods, like: [bybusyness](https://httpd.apache.org/docs/2.4/mod/mod_lbmethod_bybusyness.html), [byrequests](https://httpd.apache.org/docs/2.4/mod/mod_lbmethod_byrequests.html), [heartbeat](https://httpd.apache.org/docs/2.4/mod/mod_lbmethod_heartbeat.html)

4. Verify that our configuration works – try to access your LB’s public IP address or Public DNS name from your browser:

![lb tooling page](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/2012fd60-d168-4ca0-8a7a-70c32761ad49)

http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php



Note: If in Project-7 you mounted /var/log/httpd/ from your Web Servers to the NFS server – unmount them and make sure that each Web Server has its own log directory.

Open two ssh/Putty consoles for both Web Servers and run the following command:
```bash
sudo tail -f /var/log/httpd/access_log
```

Try to refresh your browser page http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php several times and make sure that both servers receive HTTP GET requests from your LB – new records must appear in each server’s log file. The number of requests to each server will be approximately the same since we set load factor to the same value for both servers – it means that traffic will be distributed evenly between them.

If you have configured everything correctly – your users will not even notice that their requests are served by more than one server.

![sudo tail varloghttpd](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/fa62822f-3576-4f28-9010-77b7de777741)

**Side Self Study:**

Read more about different configuration aspects of [Apache mod_proxy_balancer module](https://httpd.apache.org/docs/2.4/mod/mod_proxy_balancer.html). Understand what sticky session means and when it is used.

## Optional Step – Configure Local DNS Names Resolution

Sometimes it is tedious to remember and switch between IP addresses, especially if you have a lot of servers under your management.
What we can do, is configure local domain name resolution. The easiest way is to use /etc/hosts file, although this approach is not very scalable, it is very easy to configure and shows the concept well. So let us configure IP address to domain name mapping for our LB.

#Open this file on your LB server
```bash
sudo vi /etc/hosts
```
#Add 2 records into this file with the Local IP address and arbitrary name for both of your Web Servers

```bash
<WebServer1-Private-IP-Address> Web1
<WebServer2-Private-IP-Address> Web2
```

![sudo vi etchost](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/6adb8666-8b9b-4c15-8280-289d44ed5a66)

Now you can update your LB config file with those names instead of IP addresses.

BalancerMember http://Web1:80 loadfactor=5 timeout=1
BalancerMember http://Web2:80 loadfactor=5 timeout=1

![reconfig LB](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/4ec032d7-f621-4eac-be6a-7ee5b338f3c2)

You can try to curl your Web Servers from LB locally curl http://Web1 or curl http://Web2 – it shall work.

Remember, this is only internal configuration and it is also local to your LB server, these names will neither be ‘resolvable’ from other servers internally nor from the Internet.

Targeted Architecture
Now your set-up looks like this:





















