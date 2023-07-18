# Web Solution With WordPress

### LAUNCH AN EC2 INSTANCE THAT WILL SERVE AS “WEB SERVER”.

**Step 1 — Prepare a Web Server**

1. Launch an EC2 instance as a “Web Server”. Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

Learn How to Add EBS Volume to an EC2 instance [here](https://www.youtube.com/watch?v=HPXnXkBzIHw)

![create volumes](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/94088477-a481-43e8-90cb-198ee6f362c9)

![creating volume2](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/0ffd6156-e45a-45ab-bb0b-11def6457e5b)

![create volume3](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/64fef8d7-c5d7-457c-a00c-9c05a3e4822b)

2. Open up the Linux terminal to begin configuration

3. Use lsblk command to inspect what block devices are attached to the server. Notice the names of your newly created devices. All devices in Linux reside in /dev/ directory. Inspect it with ls /dev/ and make sure you see all 3 newly created block devices there – their names will likely be xvdf, xvdh, xvdg.

![lsblk newly created blocks](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/76ff2854-e291-4124-942e-da1497070bf4)

4. Use df -h command to see all mounts and free space on your server

5. Use gdisk utility to create a single partition on each of the 3 disks
```bash
sudo gdisk /dev/xvdf
```

![sudo gdiskdevxvdf](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/fb40ca03-d8ff-4e99-a597-8addeb75d96e)

6. Use lsblk utility to view the newly configured partition on each of the 3 disks.

![lsblk partitioned 3 disks](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/392e838a-2d4c-4d28-ba56-f99c9ed8e705)

7. Install lvm2 package using sudo yum install lvm2. Run sudo lvmdiskscan command to check for available partitions.

Note: Previously, in Ubuntu we used apt command to install packages, in RedHat/CentOS a different package manager is used, so we shall use yum command instead.

8. Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM
```bash
sudo pvcreate /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
```
![sudo pvcreat xvdf1xvdg1xvdh1](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/56a96c6c-a4ca-4e28-94b9-05ffd1db9ec9)

9. Verify that your Physical volume has been created successfully by running sudo pvs

![sudo pvs](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/96d0ce6e-740d-467e-92fb-db0e65ca0f73)

10. Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg
```bash
sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
```

![sudo vgcreate webdata-vg](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/fe79090e-9294-4d7a-a996-64c5ba9adf8a)

11. Verify that your VG has been created successfully by running sudo vgs

![sudo vgs](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/bf7ed919-ac75-48cc-bb43-707bbd084daf)

12. Use lvcreate utility to create 2 logical volumes. apps-lv (Use half of the PV size), and logs-lv Use the remaining space of the PV size. NOTE: apps-lv will be used to store data for the Website while, logs-lv will be used to store data for logs.
```bash
sudo lvcreate -n apps-lv -L 14G webdata-vg
```
```bash
sudo lvcreate -n logs-lv -L 14G webdata-vg
```

![sudo lvcreate apps and logs](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/4ce446c2-f94b-41c8-adfb-01437968bf6b)

13. Verify that your Logical Volume has been created successfully by running sudo lvs

![sudo lvs](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/4309544b-da85-4ebb-abe0-a8c43af6a317)

14. Verify the entire setup
```bash
sudo vgdisplay -v #view complete setup - VG, PV, and LV
```
```bash
sudo lsblk
```
![sudo lsblk lv](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/acebded0-f7a2-4490-a442-7bb1883263de)

15. Use mkfs.ext4 to format the logical volumes with ext4 filesystem
```bash
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
```
```bash
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```
16. Create /var/www/html directory to store website files
```bash
sudo mkdir -p /var/www/html
```
17. Create /home/recovery/logs to store the backup of log data

```bash
sudo mkdir -p /home/recovery/logs
```
18. Mount /var/www/html on apps-lv logical volume
```bash
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```

19. Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)
```bash
sudo rsync -av /var/log/. /home/recovery/logs/
```

20. Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted. That is why step 15 above is very important)
```bash
sudo mount /dev/webdata-vg/logs-lv /var/log
```
21. Restore log files back into /var/log directory
```bash
sudo rsync -av /home/recovery/logs/log/. /var/log
```

22. Update /etc/fstab file so that the mount configuration will persist after restart of the server.

The UUID of the device will be used to update the /etc/fstab file;

```bash
sudo blkid
```

![sudo blkid](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/63b2c084-d14b-4839-b6ff-eee184e2349d)

```bash
sudo vi /etc/fstab
```

23. Update /etc/fstab in this format using your own UUID and remember to remove the leading and ending quotes.

![sudo vi etcfstab](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/de0436bd-e674-43d6-9eb7-09c146123ac3)

24. Test the configuration and reload the daemon
```bash
sudo mount -a
```
```bash
sudo systemctl daemon-reload
```

25. Verify your setup by running df -h, the output must look like this:

![verify setup df h](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/17f4a3a1-5413-4b6d-912b-933c49f95eb9)


## PREPARE THE DATABASE SERVER


**Step 2 — Prepare the Database Server**

Launch a second RedHat EC2 instance that will have a role – 'DB-Server'
Repeat the same steps as for the Web Server, but instead of apps-lv create db-lv and mount it to /db directory instead of /var/www/html/.

![dbase-vg lsblk](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/dd38c39b-a737-4500-9846-a296c02a3628)

![dbase-vg dfh](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/98cbc38c-6d0b-4e36-9b1b-be25c49604c5)


**Step 3 — Install WordPress on your Web Server EC2**

1. Update the repository
```bash
sudo yum -y update
```
2. Install wget, Apache and it’s dependencies
```bash
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```
3. Start Apache
```bash
sudo systemctl enable httpd
sudo systemctl start httpd
```
![sudo systemctl status httpd apache](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/2f5618bd-fc5f-44c8-a8f8-5f834fe71267)


4. To install PHP and it’s depemdencies
```bash
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
sudo yum module list php
sudo yum module reset php
sudo yum module enable php:remi-7.4
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
setsebool -P httpd_execmem 1
```

5. Restart Apache
```bash
sudo systemctl restart httpd
```

6. Download wordpress and copy wordpress to var/www/html
```bash
  mkdir wordpress
  cd   wordpress
  sudo wget http://wordpress.org/latest.tar.gz
  sudo tar xzvf latest.tar.gz
  sudo rm -rf latest.tar.gz
  cp wordpress/wp-config-sample.php wordpress/wp-config.php
  cp -R wordpress /var/www/html
```

7. Configure SELinux Policies
```bash
  sudo chown -R apache:apache /var/www/html/wordpress
  sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
  sudo setsebool -P httpd_can_network_connect=1
```

**Step 4 — Install MySQL on your DB Server EC2**
```bash
sudo yum update
sudo yum install mysql-server
```
Verify that the service is up and running by using sudo systemctl status mysqld, if it is not running, restart the service and enable it so it will be running even after reboot:
```bash
sudo systemctl restart mysqld
sudo systemctl enable mysqld
```

![sudo systemctl status mysql](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/7cf6a9be-4085-4c38-9b57-0b7c4a1122a6)

**Step 5 — Configure DB to work with WordPress**
```bash
sudo mysql
```
![setting up Db server](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/da9e91c3-2bc9-4926-9f8f-7e9bf2b0f0a2)

![wordpress show database](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/05b9cf3c-db15-4d17-9be7-b1371c1dfdcc)

**Step 6 — Configure WordPress to connect to remote database.**

Hint: Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, you shall allow access to the DB server ONLY from your Web Server’s IP address, so in the Inbound Rule configuration specify the source as /32

![port3306 db server](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/156ff84b-ac63-4421-826b-a8de4cc7112a)

1. Install MySQL client and test that you can connect from your Web Server to your DB server by using mysql-client
```bash
sudo yum install mysql
sudo mysql -u admin -p -h <DB-Server-Private-IP-address>
```

2. Verify if you can successfully execute SHOW DATABASES; command and see a list of existing databases.
3. Change permissions and configuration so Apache could use WordPress:
4. Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2 (enable from everywhere 0.0.0.0/0 or from your workstation’s IP)



6. Try to access from your browser the link to your WordPress


































