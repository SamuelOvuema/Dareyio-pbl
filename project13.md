# INTRODUCING DYNAMIC ASSIGNMENT INTO OUR STRUCTURE

### Introducing Dynamic Assignment Into Our Structure
In your https://github.com/<your-name>/ansible-config-mgt GitHub repository start a new branch and call it dynamic-assignments.

Create a new folder, name it dynamic-assignments. Then inside this folder, create a new file and name it env-vars.yml. We will instruct site.yml to include this playbook later. For now, let us keep building up the structure.

Your GitHub shall have the following structure by now.

**Note:** Depending on what method you used in the previous project you may have or not have roles folder in your GitHub repository – if you used ansible-galaxy, then roles directory was only created on your Jenkins-Ansible server locally. It is recommended to have all the codes managed and tracked in GitHub, so you might want to recreate this structure manually in this case – it is up to you.

![dynamic assignments dropdown](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/4abd1c77-a0ef-46e9-837a-3166419c1fce)

Since we will be using the same Ansible to configure multiple environments, and each of these environments will have certain unique attributes, such as server name, ip-address etc., we will need a way to set values to variables per specific environment.

For this reason, we will now create a folder to keep each environment’s variables file. Therefore, create a new folder env-vars, then for each environment, create new **YAML** files which we will use to set variables.

Your layout should now look like this.

![envvars folder](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/299a64af-6694-4e10-ad92-8997cde35dc4)

Now paste the instruction below into the env-vars.yml file.

![envvars file](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/5087a357-39d0-4687-b639-53f247695d6f)

Notice 3 things to notice here:

1. We used include_vars syntax instead of include, this is because Ansible developers decided to separate different features of the module. From Ansible version **2.8**, the include module is deprecated and variants of include_* must be used. These are:
- [include_role](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/include_role_module.html#include-role-module)
- [include_tasks](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/include_tasks_module.html#include-tasks-module)
- [include_vars](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/include_vars_module.html#include-vars-module)

In the same version, variants of import were also introduced, such as:

- [import_role](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/import_role_module.html#import-role-module)
- [import_tasks](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/import_tasks_module.html#import-tasks-module)

2. We made use of [special variables](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html) { playbook_dir } and { inventory_file }. { playbook_dir } will help Ansible to determine the location of the running playbook, and from there navigate to other paths on the filesystem. { inventory_file } on the other hand will dynamically resolve to the name of the inventory file being used, then append .yml so that it picks up the required file within the env-vars folder.

3. We are including the variables using a loop. with_first_found implies that looping through the list of files, the first one found is used. This is good so that we can always set default values in case an environment-specific env file does not exist.


## UPDATE SITE.YML WITH DYNAMIC ASSIGNMENTS

Update site.yml file to make use of the dynamic assignment. (At this point, we cannot test it yet. We are just setting the stage for what is yet to come. So hang on to your hats)

**site.yml** should now look like this.

![update siteyml](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/b88fac6a-0b16-4aae-97ce-fcc34e920679)

### Community Roles

Now it is time to create a role for MySQL database – it should install the MySQL package, create a database and configure users. But why should we re-invent the wheel? There are tons of roles that have already been developed by other open-source engineers out there. These roles are production-ready, and dynamic to accommodate most of Linux flavours. With Ansible Galaxy again, we can simply download a ready-to-use ansible role, and keep going.

### Download Mysql Ansible Role

You can browse available community roles [here](https://galaxy.ansible.com/home)

We will be using a [MySQL role developed by geerlingguy](https://galaxy.ansible.com/geerlingguy/mysql).

**Hint**: To preserve your GitHub in its actual state after you install a new role – make a commit and push to master your ‘ansible-config-mgt’ directory. Of course, you must have git installed and configured on the Jenkins-Ansible server and, for more convenient work with codes, you can configure Visual Studio Code to work with this directory. In this case, you will no longer need webhook and Jenkins jobs to update your codes on the Jenkins-Ansible server, so you can disable it – we will be using Jenkins later for a better purpose.

On the Jenkins-Ansible server make sure that git is installed with the git --version, then go to the ‘ansible-config-mgt’ directory and run
```bash
git init
git pull https://github.com/<your-name>/ansible-config-mgt.git
git remote add origin https://github.com/<your-name>/ansible-config-mgt.git
git branch roles-feature
git switch roles-feature
```
Inside the roles directory create your new MySQL role with ansible-galaxy install geerlingguy.mysql and rename the folder to mysql
```bash
mv geerlingguy.mysql/ MySQL
```
Read the README.md file, and edit the roles configuration to use the correct credentials for MySQL required for the tooling website.

Now it is time to upload the changes into your GitHub:
```bash
git add .
git commit -m "Commit new role files into GitHub"
git push --set-upstream origin roles-feature
```
Now, if you are satisfied with your codes, you can create a Pull Request and merge it to the main branch on GitHub.
![testing mysql](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/5af57934-8e74-453c-b38d-d6ef1902e55c)

## Load Balancer roles

We want to be able to choose which Load Balancer to use, Nginx or Apache, so we need to have two roles respectively:

1. Nginx
2. Apache

With your experience on Ansible so far you can:

- Decide if you want to develop your own roles, or find available ones from the community
- Update both static-assignment and site.yml files to refer to the roles

***Important Hints***:

- Since you cannot use both Nginx and Apache load balancer, you need to add a condition to enable either one – this is where you can make use of variables.
- Declare a variable in defaults/main.yml file inside the Nginx and Apache roles. Name each variable enable_nginx_lb and enable_apache_lb respectively.
- Set both values to false like this enable_nginx_lb: false and enable_apache_lb: false.
![nginxdefaultmainyml](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/916d1d38-c8fd-4da7-86b4-3fe5ca81429b)
- Declare another variable in both roles load_balancer_is_required and set its value to false as well
- Update both assignment and site.yml files respectively

loadbalancers.yml file

![loadbalanceryml](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/55691c85-4f79-4d5b-9a9b-cb7f370a6636)

site.yml file

![siteyml4nginxlb](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/9ae54686-6b2d-4674-8461-e91890ffb5f8)

Now you can make use of env-vars\uat.yml file to define which load-balancer to use in the UAT environment by setting the respective environmental variable to true.

You will activate load balancer, and enable nginx by setting these in the respective environment’s env-vars file.

![envvars devyml file](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/f471f0b2-5e15-49d0-9431-bb19a80a98b3)

The same must work with apache LB, so you can switch it by setting respective environmental variables to true and others to false.
run ansible

![run ansible 4 nginx1](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/4a2b809f-6dda-444f-a94f-6a6a83bb7028)
![run ansible 4 nginx2](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/98759cd1-fa58-4cb4-ad6a-7813ca0fa388)
![run ansible 4 nginx3](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/8eaefd41-6b4b-4fd7-b1ae-cb9629ff2f05)

To test this, you can update the inventory for each environment and run Ansible against each environment.

![sudo systemctl status nginx](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/7f7d005f-2d50-4e76-afeb-512241afb631)

Congratulations!
You have learned and practised how to use the Ansible configuration management tool to prepare the UAT environment for the Tooling web solution.
