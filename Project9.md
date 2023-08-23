# Continous Integration Pipeline For Tooling Website

## INSTALL AND CONFIGURE JENKINS SERVER

**Step 1 – Install Jenkins server**

1. Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it “Jenkins”

2. Install JDK (since Jenkins is a Java-based application)
```bash
sudo apt update
sudo apt install default-jdk-headless
```
3. Install Jenkins
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

4. By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group

![Jenkins instance inbound rules](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/af01ff9e-f9cd-462c-990c-4616cd45ebee)

5. Perform initial Jenkins setup.

From your browser access http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080

You will be prompted to provide a default admin password



Retrieve it from your server:

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Then you will be asked which plugins to install – choose suggested plugins.


Once plugins installation is done – create an admin user and you will get your Jenkins server address.

The installation is completed!


Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks

In this part, you will learn how to configure a simple Jenkins job/project (these terms can be used interchangeably). GitHub webhooks will trigger this job and execute a ‘build’ task to retrieve codes from GitHub and store them locally on the Jenkins server.



Go to Jenkins web console, click “New Item” and create a “Freestyle project”


To connect your GitHub repository, you will need to provide its URL, you can copy from the repository itself



In the configuration of your Jenkins freestyle project choose the Git repository and provide there the link to your Tooling GitHub repository and credentials (user/password) so Jenkins could access files in the repository.



Save the configuration and let us try to run the build. For now, we can only do it manually.

Click “Build Now” button, if you have configured everything correctly, the build will be successful and you will see it under #1


You can open the build and check in “Console Output” if it has run successfully.

If so – congratulations! You have just made your very first Jenkins build!

But this build does not produce anything and it runs only when we trigger it manually. Let us fix it.

Click “Configure” your job/project and add these two configurations
Configure triggering the job from the GitHub webhook:


Configure “Post-build Actions” to archive all the files – files resulting from a build are called “artefacts”.


Now, go ahead and make some changes in any file in your GitHub repository (e.g. README.MD file) and push the changes to the master branch.

You will see that a new build has been launched automatically (by webhook) and you can see its results – artifacts, saved on the Jenkins server.



You have now configured an automated Jenkins job that receives files from GitHub by webhook trigger (this method is considered as ‘push’ because the changes are being ‘pushed’ and files transfer is initiated by GitHub). There are also other methods: trigger one job (downstream) from another (upstream), poll GitHub periodically and others.

By default, the artifacts are stored on the Jenkins server locally

ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/






































