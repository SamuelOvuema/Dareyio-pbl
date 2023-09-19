# Ansible Refactoring, Assignments & Imports

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

4. Create a new Freestyle project (you have done it in Project 9) and name it save_artifacts.

![saveartifacts freestyle project](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/56dc83bf-2706-4f0d-98fe-1aa0209f6a66)

5. This project will be triggered by completion of your existing ansible project. Configure it accordingly:

![General config](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/860b2843-cf8e-460f-b858-2781d88a67f3)

![sourcecode mgt](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/1019adad-f913-42f1-ba33-22f8819ced10)

Note: You can configure number of builds to keep in order to save space on the server, for example, you might want to keep only last 2 or 5 build results. You can also make this change to your ansible job.

6. The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory. To achieve this, create a Build step and choose Copy artifacts from other project, specify ansible as a source project and /home/ubuntu/ansible-config-artifact as a target directory.

![build steps](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/7d26ef8f-f932-4439-bc35-d81507cdbf73)

7. Test your set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside master branch).

If both Jenkins jobs have completed one after another – you shall see your files inside /home/ubuntu/ansible-config-artifact directory and it will be updated with every commit to your master branch.

Now your Jenkins pipeline is more neat and clean.



