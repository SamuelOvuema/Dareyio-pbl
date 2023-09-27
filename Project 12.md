# Ansible Refactoring, Assignments & Imports

In this project you will continue working with ansible-config-mgt repository and make some improvements of your code. Now you need to refactor your Ansible code, create assignments, and learn how to use the imports functionality. Imports allow to effectively re-use previously created playbooks in a new playbook – it allows you to organize your tasks and reuse them when needed.

Side Self Study: For better understanding or Ansible artifacts re-use – [read this article](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse.html).

Code Refactoring
[Refactoring](https://en.wikipedia.org/wiki/Code_refactoring) is a general term in computer programming. It means making changes to the source code without changing expected behaviour of the software. The main idea of refactoring is to enhance code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic.

In your case, you will move things around a little bit in the code, but the overal state of the infrastructure remains the same.

Let us see how you can improve your Ansible code!

### Step 1 – Jenkins job enhancement
Before we begin, let us make some changes to our Jenkins job – now every new change in the codes creates a separate directory which is not very convenient when we want to run some commands from one place. Besides, it consumes space on Jenkins serves with each subsequent change. Let us enhance it by introducing a new Jenkins project/job – we will require Copy Artifact plugin.

1. Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build.
```bash
sudo mkdir /home/ubuntu/ansible-config-artifact
```
![ansibleconfigartifact directory](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/78122aa5-4bda-400f-b4de-dd80cc7f189f)

2. Change permissions to this directory, so Jenkins could save files there – chmod -R 0777 /home/ubuntu/ansible-config-artifact

3. Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins

![copy artifact plugins](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/9d097100-b866-4fc7-b875-ef282abc2cf0)

4. Create a new Freestyle project (you have done it in [Project 9)](https://professional-pbl.darey.io/en/latest/project9.html) and name it save_artifacts.

![saveartifacts freestyle project](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/56dc83bf-2706-4f0d-98fe-1aa0209f6a66)

5. This project will be triggered by completion of your existing ansible project. Configure it accordingly:

![General config](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/860b2843-cf8e-460f-b858-2781d88a67f3)

![sourcecode mgt](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/1019adad-f913-42f1-ba33-22f8819ced10)

Note: You can configure number of builds to keep in order to save space on the server, for example, you might want to keep only last 2 or 5 build results. You can also make this change to your ansible job.

6. The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory. To achieve this, create a Build step and choose Copy artifacts from other project, specify ansible as a source project and /home/ubuntu/ansible-config-artifact as a target directory.

![build steps](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/7d26ef8f-f932-4439-bc35-d81507cdbf73)

7. Test your set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside master branch).

![git hub main readme changes](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/f220838c-608f-4978-b8a7-87aabd601e1b)

If both Jenkins jobs have completed one after another – you shall see your files inside /home/ubuntu/ansible-config-artifact directory and it will be updated with every commit to your master branch.

![Cat readme ansibleconfigartifact](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/cc943707-be35-4099-9b19-024de768ea3e)

Now your Jenkins pipeline is more neat and clean.


## REFACTOR ANSIBLE CODE BY IMPORTING OTHER PLAYBOOKS INTO SITE.YML

### Step 2 – Refactor Ansible code by importing other playbooks into site.yml

Before starting to refactor the codes, ensure that you have pulled down the latest code from master (main) branch, and created a new branch, name it refactor.

DevOps philosophy implies constant iterative improvement for better efficiency – refactoring is one of the techniques that can be used, but you always have an answer to question "why?". Why do we need to change something if it works well?

In [Project 11](https://www.dareyio.com/docs/ansible-configuration-management-automate-project-7-to-10/) you wrote all tasks in a single playbook common.yml, now it is pretty simple set of instructions for only 2 types of OS, but imagine you have many more tasks and you need to apply this playbook to other servers with different requirements. In this case, you will have to read through the whole playbook to check if all tasks written there are applicable and is there anything that you need to add for certain server/OS families. Very fast it will become a tedious exercise and your playbook will become messy with many commented parts. Your DevOps colleagues will not appreciate such organization of your codes and it will be difficult for them to use your playbook.

Most Ansible users learn the one-file approach first. However, breaking tasks up into different files is an excellent way to organize complex sets of tasks and reuse them.

Let see code re-use in action by importing other playbooks.

1. Within playbooks folder, create a new file and name it site.yml – This file will now be considered as an entry point into the entire infrastructure configuration. Other playbooks will be included here as a reference. In other words, site.yml will become a parent to all other playbooks that will be developed. Including common.yml that you created previously. Dont worry, you will understand more what this means shortly.

2. Create a new folder in the root of the repository and name it static-assignments. The static-assignments folder is where all other children playbooks will be stored. This is merely for easy organization of your work. It is not an Ansible-specific concept, therefore you can choose how you want to organize your work. You will see why the folder name has a prefix of static very soon. For now, just follow along.

3. Move the common.yml file into the newly created static-assignments folder.

4. Inside site.yml file, import common.yml playbook.

![site](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/1eb3bca1-83b3-40d1-a4d5-ac8ddbf9ada9)


The code above uses built-in [import_playbook](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/import_playbook_module.html) Ansible module.

Your folder structure should look like this;
```bash
├── static-assignments
│   └── common.yml
├── inventory
    └── dev
    └── stage
    └── uat
    └── prod
└── playbooks
    └── site.yml
```

5. Run ansible-playbook command against the dev environment

Since you need to apply some tasks to your dev servers and wireshark is already installed – you can go ahead and create another playbook under static-assignments and name it common-del.yml. In this playbook, configure deletion of wireshark utility.


![common-del](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/23516c74-ae05-47f1-9baf-8afa448e5d3a)

```

update site.yml with - import_playbook: ../static-assignments/common-del.yml instead of common.yml and run it against dev servers:
```bash
cd /home/ubuntu/ansible-config-mgt/

ansible-playbook -i inventory/dev.yml playbooks/site.yaml
```

![ansible playbook site](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/aa240cfb-4520-407b-8e28-26fa6f2740cb)

Make sure that wireshark is deleted on all the servers by running wireshark --version

![wiershark version deleted](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/45b88d11-b424-483a-8545-20c17196bacb)

Now you have learned how to use import_playbooks module and you have a ready solution to install/delete packages on multiple servers with just one command.













