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
