# Continous Integration Pipeline For Tooling Website

## INSTALL AND CONFIGURE JENKINS SERVER

**Step 1 – Install Jenkins server**

Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it “Jenkins”

Install JDK (since Jenkins is a Java-based application)
```bash
sudo apt update
sudo apt install default-jdk-headless
```
Install Jenkins
```bash
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt-get install jenkins
```

Make sure Jenkins is up and running
```bash
sudo systemctl status jenkins
```

By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group

![Jenkins instance inbound rules](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/af01ff9e-f9cd-462c-990c-4616cd45ebee)








































