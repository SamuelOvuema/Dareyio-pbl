# Implement a Client-Server Architecture using MySQL Database Management System (DBMS).

To demonstrate a basic client-server using MySQL Relational Database Management System (RDBMS), follow the below instructions

1. Create and configure two Linux-based virtual servers (EC2 instances in AWS).
```
Server A name - `mysql server`
Server B name - `mysql client`
```

2. On mysql server Linux Server install MySQL Server software.

3. On mysql client Linux Server install MySQL Client software.

4. By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’.

![Db-server indound rules](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/845282d4-394c-422f-a09e-f9951651ff36)

5. You might need to configure MySQL server to allow connections from remote hosts.
```bash
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```
Replace ‘127.0.0.1’ to ‘0.0.0.0’ like this:

![replacing 1270001](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/2279b4fd-7be1-453e-8fc3-1cbf8d0feb91)

6. From mysql client Linux Server connect remotely to mysql server Database Engine without using SSH. You must use the mysql utility to perform this action.
7. Check that you have successfully connected to a remote MySQL server and can perform SQL queries:
```bash
Show databases;
```

If you see an output similar to the below image, then you have successfully completed this project – you have deployed a fully functional MySQL Client-Server setup.
Well, Done! You are getting there gradually. You can further play around with this setup and practice creating/dropping databases & tables and inserting/selecting records to and from them.



















