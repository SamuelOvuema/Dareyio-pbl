## ANSIBLE – AUTOMATE PROJECT 7 TO 10

### INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE

1. Update the Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

2. In your GitHub account create a new repository and name it ansible-config-mgt.


3. Install Ansible

```bash
sudo apt update
sudo apt install ansible
```
Check your Ansible version by running the Ansible --version

![ansible version](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/7d752310-9eff-4aa2-b185-2b6daf60ece4)

4. Configure the Jenkins build job to save your repository content every time you change it – this will solidify your Jenkins configuration skills acquired in Project 9.

Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.

![create ansible freestyle pro](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/44a95c61-5e7f-4f6e-94b2-4089cf2fc981)

Configure Webhook in GitHub and set Webhook to trigger ansible build.

![add webhook](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/b3827548-32f4-40fb-9718-7694321cbf92)

Configure a Post-build job to save all (**) files, as you did in Project 9.

![post build artifacts](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/464f5960-70ae-4523-b55b-13fccc43bfe1)

5. Test your setup by making some changes in README.MD file in the master branch and make sure that builds start automatically and Jenkins saves the files (build artifacts) in the following folder

![cat README md](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/cad41c3b-fb93-407b-9ddb-5bc8cb93dede)

ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/

Note: Trigger Jenkins project execution only for /main (master) branch.

Now your setup will look like this:

![set up](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/1fc818f3-bd62-4a95-a6d8-9a4ef86a0d39)

**Tip** Every time you stop/start your Jenkins-Ansible server – you have to reconfigure the GitHub webhook to a new IP address, In order to avoid it, it makes sense to allocate an [Elastic IP](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) to your Jenkins-Ansible server (you have done it before to your LB server in [Project 10](https://professional-pbl.darey.io/en/latest/project10.html)). Note that Elastic IP is free only when it is being allocated to an EC2 Instance, so do not forget to release Elastic IP once you terminate your EC2 Instance.

### Step 2 – Prepare your development environment using Visual Studio Code

1. The first part of ‘DevOps’ is ‘Dev’, which means you will be required to write some codes and you shall have proper tools that will make your coding and debugging comfortable – you need an [Integrated development environment (IDE)](https://en.wikipedia.org/wiki/Integrated_development_environment) or [Source-code Editor](https://en.wikipedia.org/wiki/Source-code_editor). There is a plethora of different IDEs and Source-code Editors for different languages with their own advantages and drawbacks, You can choose whichever you are comfortable with, but we recommend one free and universal editor that will fully satisfy your needs – [Visual Studio Code (VSC)](https://en.wikipedia.org/wiki/Visual_Studio_Code), you can get it [here](https://code.visualstudio.com/download).

2. After you have successfully installed VSC, configure it to connect to your newly created GitHub repository.

3. Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance
```bash
git clone <ansible-config-mgt repo link>
```

## BEGIN ANSIBLE DEVELOPMENT

1. In your ansible-config-mgt GitHub repository, create a new branch that will be used for the development of a new feature.
**Tip:** Give your branches descriptive and comprehensive names, For example, if you use [Jira](https://www.atlassian.com/software/jira) or [Trello](https://trello.com/) as a project management tool – include the ticket number (e.g. PRJ-11) in the name of your branch and add a topic and a brief description of what this branch is about – a bugfix, hotfix, feature, release (e.g. feature/prj-11-lvm)

2. Checkout the newly created feature branch to your local machine and start building your code and directory structure

![git checkout prj11](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/2e5118e8-53aa-43a4-8437-29961d7d9fb2)

3. Create a directory and name it playbooks – it will be used to store all your playbook files.
4. Create a directory and name it inventory – it will be used to keep your hosts organised.
5. Within the *playbooks* folder, create your first playbook, and name it common.yml
5. Within the *inventory* folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, UAT, and prod respectively.

![create dir and yml files](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/f78e5ba9-a456-4925-bfe9-06214c5e8af3)

**Step 4 – Set up an Ansible Inventory**

An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

Save below inventory structure in the inventory/dev file to start configuring your *development* servers. Ensure to replace the IP addresses according to your own setup.

**Note:** Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host – for this you can implement the concept of ssh-agent. Now you need to import your key into ssh-agent:

To learn how to setup SSH agent and connect VS Code to your Jenkins-Ansible instance, please see this video:

For Windows users – ssh-agent on windows
For Linux users – ssh-agent on Linux
```bash
eval `ssh-agent -s`
ssh-add <path-to-private-key>
```
Confirm the key has been added with the command below, you should see the name of your key
```bash
ssh-add -l
```
Now, ssh into your Jenkins-Ansible server using ssh-agent
```bash
ssh -A ubuntu@public-ip
```
Also notice, that your Load Balancer user is ubuntu and user for RHEL-based servers is ec2-user.

Update your inventory/dev.yml file with this snippet of code:

![inventory host](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/2006fd99-6620-4f47-a067-fed99c19ba15)

## CREATE A COMMON PLAYBOOK

**Step 5 – Create a Common Playbook**
It is time to start giving Ansible the instructions on what you needs to be performed on all servers listed in inventory/dev.

In common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

Update your playbooks/common.yml file with following code:























