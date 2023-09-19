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
