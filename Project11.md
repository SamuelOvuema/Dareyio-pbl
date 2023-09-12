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

































