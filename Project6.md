# Web Solution With WordPress

### LAUNCH AN EC2 INSTANCE THAT WILL SERVE AS “WEB SERVER”.

Step 1 — Prepare a Web Server

1. Launch an EC2 instance as a “Web Server”. Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.

Learn How to Add EBS Volume to an EC2 instance [here](https://www.youtube.com/watch?v=HPXnXkBzIHw)

![create volumes](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/94088477-a481-43e8-90cb-198ee6f362c9)

![creating volume2](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/0ffd6156-e45a-45ab-bb0b-11def6457e5b)

![create volume3](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/64fef8d7-c5d7-457c-a00c-9c05a3e4822b)

2. Open up the Linux terminal to begin configuration

3. Use lsblk command to inspect what block devices are attached to the server. Notice names of your newly created devices. All devices in Linux reside in /dev/ directory. Inspect it with ls /dev/ and make sure you see all 3 newly created block devices there – their names will likely be xvdf, xvdh, xvdg.

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
























