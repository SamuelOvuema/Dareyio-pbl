# Devops tooling website solution

## Step 1 – Prepare NFS Server

1. Spin up a new EC2 instance with RHEL Linux 8 Operating System.

2. Based on your LVM experience from Project 6, Configure LVM on the Server.
![sudo gdisk](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/6a0d4854-686c-41f2-86ff-06ba5b3293a6)

Instead of formating the disks as ext4, you will have to format them as xfs

Ensure there are 3 Logical Volumes. lv-opt lv-apps, and lv-logs

![sudo yum install lvm2](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/06770ee9-6f5b-48a4-ac04-fcb1ad074482)

Create mount points on /mnt directory for the logical volumes as follow:

![sudo create physical volumes](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/a9fd27e6-6db3-432e-bce9-866030ce1b40)

Mount lv-apps on /mnt/apps – To be used by webservers

Mount lv-logs on /mnt/logs – To be used by webserver logs

Mount lv-opt on /mnt/opt – To be used by Jenkins server in Project 8

![sudo create logical volumes](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/e7aa750d-4200-4bb1-815a-c184ac61eeb8)

![sudo create volume group](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/3baef536-7789-4987-860b-78470c647f47)

![sudo vgdisplay VGPVLV](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/abd6dc66-e139-45af-b379-b64bc0816c7b)

![create mnt dir   mount](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/09ce4a55-4418-4597-a17f-0262e01a5e2c)

Instead of formating the disks as ext4, you will have to format them as xfs

![sudo mkfs xfs](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/ae18e370-0c31-4333-9fdc-e46bd414b816)


3. Install NFS server, configure it to start on reboot and make sure it is u and running

```bash
sudo yum -y update
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```
![sudo systemctl status nfs server service](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/0511677d-53ca-46bc-942f-03ac3a6a2450)

5. Export the mounts for webservers’ subnet cidr to connect as clients. You will install all three Web Servers inside the same subnet for simplicity. Still, in the production set-up, you would probably want to separate each tier inside its own subnet for a higher level of security.

To check your subnet cidr – open your EC2 details in AWS web console and locate ‘Networking’ tab and open a Subnet link:

![subnet cidr IP](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/c959d864-3c11-4a7d-ad1a-d1bc0f7ba77d)

Make sure we set up permission that will allow our Web servers to read, write and execute files on NFS:

```bash
sudo chown -R nobody: /mnt/apps
sudo chown -R nobody: /mnt/logs
sudo chown -R nobody: /mnt/opt

sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt

sudo systemctl restart nfs-server.service
```
Configure access to NFS for clients within the same subnet (example of Subnet CIDR – 172.31.32.0/20 ):

sudo vi /etc/exports

![sudo vi ets export](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/de840ff9-db7b-413a-8160-609268b3a728)

Esc + :wq!

![sudo systemctl status nfs server service](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/a47de0f5-0819-44a0-bbc0-afcaac1146be)

sudo exportfs -arv

6. Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)
```bash
rpcinfo -p | grep nfs
```

![rpcinfo pgrep nfs](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/05f86367-edcc-4c51-b305-6e7d6ae247eb)

**Important note:** In order for NFS server to be accessible from your client, you must also open following ports: TCP 111, UDP 111, UDP 2049

![inbound rule for NFS](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/bc81418a-eef5-4de9-8e5d-e5bd0711a5f2)


STEP 2 — CONFIGURE THE DATABASE SERVER
By now you should know how to install and configure a MySQL DBMS to work with remote Web Server

Install MySQL server

![sudo apt install mysql](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/326a3059-1b19-4986-9b95-4718ee0a4c80)

Create a database and name it tooling

Create a database user and name it webaccess

Grant permission to webaccess user on tooling database to do anything only from the webservers subnet cidr

![sudo mysql](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/56ef15c3-ac78-41ac-b137-70294d2147bd)

Step 3 — Prepare the Web Servers
We need to make sure that our Web Servers can serve the same content from shared storage solutions, in our case – NFS Server and MySQL database.
You already know that one DB can be accessed for reads and writes by multiple clients. For storing shared files that our Web Servers will use – we will utilize NFS and mount previously created Logical Volume lv-apps to the folder where Apache stores files to be served to the users (/var/www).

This approach will make our Web Servers stateless, which means we will be able to add new ones or remove them whenever we need, and the integrity of the data (in the database and on NFS) will be preserved.

During the next steps we will do following:

Configure NFS client (this step must be done on all three servers)

Deploy a Tooling application to our Web Servers into a shared NFS folder

Configure the Web Servers to work with a single MySQL database


Launch a new EC2 instance with RHEL 8 Operating System

Install NFS client
```bash
sudo yum install nfs-utils nfs4-acl-tools -y
```
Mount /var/www/ and target the NFS server’s export for apps
```bash
sudo mkdir /var/www
```
```bash
sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www
```
Verify that NFS was mounted successfully by running df -h. Make sure that the changes will persist on Web Server after reboot:
```bash
sudo vi /etc/fstab
```
add following line

<NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0

![sudo vi etc fstab pr7](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/bc6eddf5-4649-4cdc-8bea-f8af4b69402b)
Install Remi’s repository, Apache and PHP


sudo yum install httpd -y

sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

sudo dnf module reset php

sudo dnf module enable php:remi-7.4

sudo dnf install php php-opcache php-gd php-curl php-mysqlnd

sudo systemctl start php-fpm

sudo systemctl enable php-fpm

setsebool -P httpd_execmem 1
Repeat steps 1-5 for another 2 Web Servers.

6. Verify that Apache files and directories are available on the Web Server in /var/www and also on the NFS server in /mnt/apps. If you see the same files – it means NFS is mounted correctly. You can try to create a new file touch test.txt from one server and check if the same file is accessible from other Web Servers.

7. Locate the log folder for Apache on the Web Server and mount it to NFS server’s export for logs. Repeat step №4 to make sure the mount point will persist after reboot.

![sudo vi etc fstab apps and logs](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/23f88b65-d251-49c5-8b77-f7ca59261c6e)


8. Fork the tooling source code from Darey.io Github Account to your Github account. (Learn how to fork a repo here)
 
9 . Deploy the tooling website’s code to the Webserver. 

![Darey fork tooling](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/a5b28d37-c867-48ee-8483-4d874c06bfda)

Ensure that the html folder from the repository is deployed to /var/www/html
Note 1: Do not forget to open TCP port 80 on the Web Server.


Note 2: If you encounter 403 Error – check permissions to your /var/www/html folder and also disable SELinux sudo setenforce 0

To make this change permanent – open following config file sudo vi /etc/sysconfig/selinux and set SELINUX=disabledthen restrt httpd.


















































