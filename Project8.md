 # Load Balancer Solution With Apache

## Configure Apache As A Load Balancer

1. Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb, so your EC2 list will look like this:

2. Open TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in Security Group.


3. Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:

#Install apache2
```bash
sudo apt update
sudo apt install apache2 -y
sudo apt-get install libxml2-dev
```

#Enable following modules:
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

Configure load balancing























