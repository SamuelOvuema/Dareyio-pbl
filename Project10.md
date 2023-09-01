# Load Balancer Solution With Nginx and SSL/TLS

## CONFIGURE NGINX AS A LOAD BALANCER

You can either uninstall Apache from the existing Load Balancer server or create a fresh installation of Linux for Nginx.

1. Create an EC2 VM based on Ubuntu Server 20.04 LTS and name it Nginx LB (do not forget to open TCP port 80 for HTTP connections, also open TCP port 443 – this port is used for secured HTTPS connections)

![inbound rules for NginxLB](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/91099286-175d-44b0-8021-d7484cf37bfd)

2. Update /etc/hosts file for local DNS with Web Servers’ names (e.g. Web1 and Web2) and their local IP addresses

![update etchost web servers](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/dfd582b6-56a5-4fbf-898a-1f0f3866c248)

3. Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers

Update the instance and Install Nginx
```bash
sudo apt update
sudo apt install nginx
```
![Restart Nginx](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/2ccdd05f-f1a9-4599-8296-d767281c6a96)

Configure Nginx LB using Web Servers’ names defined in /etc/hosts

Hint: Read this [blog](https://linuxize.com/post/how-to-edit-your-hosts-file/) to read about /etc/host

Open the default nginx configuration file
```bash
sudo vi /etc/nginx/nginx.conf
```

#insert following configuration into http section

![sudo vi conf etcnginx](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/6f363bca-3a70-4185-95d6-87f13ead3ba4)

 
Restart Nginx and make sure the service is up and running

```bash
sudo systemctl restart nginx
sudo systemctl status nginx
```
![status nginx 2](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/736fead6-7294-4303-a86d-1681b143d44b)


Side Self Study: Read more about HTTP load balancing methods and features supported by Nginx [on this page](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)


## REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES

Let us make the necessary configurations to make connections to our Tooling Web Solution secured!

In order to get a valid SSL certificate – you need to register a new domain name, you can do it using any [Domain name registrar](https://en.wikipedia.org/wiki/Domain_name_registrar) – a company that manages the reservation of domain names. The most popular ones are [Godaddy.com](https://godaddy.com/), [Domain.com](https://www.domain.com/), [Bluehost.com](https://www.bluehost.com/).

1. Register a new domain name with any registrar of your choice in any domain zone (e.g. .com, .net, .org, .edu, .info, .xyz or any other)

![registered domain](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/170125d8-3d8a-4c3f-912c-87c30233a0ac)


2. Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP

You might have noticed, that every time you restart or stop/start your EC2 instance – you get a new public IP address. When you want to associate your domain name – it is better to have a static IP address that does not change after reboot. Elastic IP is the solution for this problem, learn how to allocate an Elastic IP and associate it with an EC2 server [on this page](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html).

3. Update [A record](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/) in your registrar to point to Nginx LB using Elastic IP address. However, in this documentation, I did not assign an Elastic IP, instead, I used the public IP to configure "A Record" in Route 53.

![A record in Route 53](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/0979680f-e8f6-499b-a085-7b1c0bead7ff)

Learn how to associate your domain name to your Elastic IP on [this page](https://medium.com/progress-on-ios-development/connecting-an-ec2-instance-with-a-godaddy-domain-e74ff190c233).

Side Self Study: Read about different [DNS record types](https://www.cloudflare.com/learning/dns/dns-records/) and learn what they are used for.

Check that your Web Servers can be reached from your browser using new domain name using HTTP protocol – http://<your-domain-name.com>

4. Configure Nginx to recognize your new domain name

Update your nginx.conf with server_name www.<your-domain-name.com> instead of server_name www.domain.com

![nginx config domain name](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/1de8f32a-8d0e-4d3c-ad0f-423ef27ba898)

5. Install [certbot](https://certbot.eff.org/) and request for an SSL/TLS certificate

Make sure [snapd](https://snapcraft.io/snapd) service is active and running
```bash
sudo systemctl status snapd
```
Install certbot
```bash
sudo snap install --classic certbot
```
Request your certificate (just follow the certbot instructions – you will need to choose which domain you want your certificate to be issued for, domain name will be looked up from nginx.conf file so make sure you have updated it on step 4).
```bash
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --nginx
```

![sudo certbot nginx ovuema](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/76dd6b73-cd03-4403-8a66-05692f62f359)

Test secured access to your Web Solution by trying to reach https://<your-domain-name.com>

You shall be able to access your website by using HTTPS protocol (that uses TCP port 443) and see a padlock pictogram in your browser’s search string.
Click on the padlock icon and you can see the details of the certificate issued for your website.     

![secured domain ip](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/5b12aac6-07f2-4df0-b9e1-442c4d544400)

6. Set up the periodical renewal of your SSL/TLS certificate

By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently.

You can test the renewal command in dry-run mode
```bash
sudo certbot renew --dry-run
```

Best practice is to have a scheduled job that to run renew command periodically. Let us configure a cronjob to run the command twice a day.

To do so, lets edit the crontab file with the following command:
```bash
crontab -e
```

Add the following line:
```bash
* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1
```

You can always change the interval of this cronjob if twice a day is too often by adjusting the schedule expression.

**Side Self Study:** Refresh your cron configuration knowledge by watching [this video](https://youtu.be/4g1i0ylvx3A).

You can also use this handy [online cron expression editor](https://crontab.guru/).

Congratulations!
You have just implemented an Nginx Load Balancing Web Solution with secured HTTPS connection with periodically updated SSL/TLS certificates.

















































