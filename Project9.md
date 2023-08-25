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

![jenkins starting page](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/3144c4a1-d1e5-4809-a51e-5dcf9e74b948)

Retrieve it from your server:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
Then you will be asked which plugins to install – choose suggested plugins.


Once plugins installation is done – create an admin user and you will get your Jenkins server address.

The installation is completed!


**Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks**

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


![built artifacts outcome](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/d3f74148-18a0-4619-b73f-47509da9ecf0)


## CONFIGURE JENKINS TO COPY FILES TO NFS SERVER VIA SSH

**Step 3 – Configure Jenkins to copy files to the NFS server via SSH**

Now we have our artifacts saved locally on the Jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory.

Jenkins is a highly extendable application and there are 1400+ plugins available. We will need a plugin that is called [“Publish Over SSH”](https://plugins.jenkins.io/publish-over-ssh/).

1. Install the “Publish Over SSH” plugin.

On the main dashboard select “Manage Jenkins” and choose the “Manage Plugins” menu item.

On the “Available” tab search for the “Publish Over SSH” plugin and install it 

![publish over ssh](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/0e226d49-039a-4540-a683-6d8cdc207deb)

2. Configure the job/project to copy artifacts over to the NFS server.

On the main dashboard select “Manage Jenkins” and choose the “Configure System” menu item.

Scroll down to Publish over the SSH plugin configuration section and configure it to be able to connect to your NFS server:

1. Provide a private key (the content of the .pem file that you use to connect to the NFS server via SSH/Putty)

2. Arbitrary name

3. Hostname – can be the private IP address of your NFS server

4. Username – ec2-user (since the NFS server is based on EC2 with RHEL 8)

5. Remote directory – /mnt/apps since our Web Servers use it as a mounting point to retrieve files from the NFS server

Test the configuration and make sure the connection returns Success. Remember, that TCP port 22 on the NFS server must be open to receive SSH connections.

![successful config publish](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/02da38cf-900e-4846-a024-3424413c0f72)

Save the configuration, open your Jenkins job/project configuration page and add another one “Post-build Action”


Configure it to send all files produced by the build into our previously define remote directory. In our case we want to copy all files and directories – so we use **.

If you want to apply some particular pattern to define which files to send – use this syntax.

![send build artifact](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/0eaef9f3-35f2-4357-aacf-49e8ed7bf722)

Save this configuration and go ahead and change something in README.MD file in your GitHub Tooling repository.

Webhook will trigger a new job and in the “Console Output” of the job you will find something like this:

![config publishoverssh](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/dcdf2568-30e9-4ff7-97a1-215bd4eddbb0)

To make sure that the files in /mnt/apps have been updated – connect via SSH/Putty to your NFS server and check README.MD file

cat /mnt/apps/README.md

![cat README](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/9e9e1aa2-fda8-4bd4-9775-ed13f93bdfba)

If you see the changes you had previously made in your GitHub – the job works as expected.

![BUILD 6 github change](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/69482cf4-5793-49a7-9673-fc5d9fd05cc2)

We have just implemented your first Continuous Integration solution using Jenkins CI. Watch out for advanced CI configurations in upcoming projects.



















