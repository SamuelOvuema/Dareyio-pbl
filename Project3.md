# MERN STACK IMPLEMENTATION

## STEP 1 – BACKEND CONFIGURATION

Update ubuntu

> sudo apt update

![apt update pjt3](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/e725ce75-ab2c-4129-81b6-17c1c6799c77)

Upgrade ubuntu

> sudo apt upgrade

![pjt3 ubuntu upgrade](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/176f34f8-c44a-4bde-a40d-26104ce10c0e)

Lets get the location of Node.js software from Ubuntu repositories.

> curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -

![node js 4rm ubuntu](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/79e92344-cc35-4ed3-9a42-fabb648dfdb1)

Install Node.js on the server
Install Node.js with the command below

> sudo apt-get install -y nodejs

![install nodejs](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/cce10203-b7ad-4d2e-b8cf-17cb22af5144)

Note: The command above installs both nodejs and npm. NPM is a package manager for Node like apt for Ubuntu, it is used to install Node modules & packages and to manage dependency conflicts.

Verify the node installation with the command below

> node -v

Verify the npm installation with the command below

> npm -v 

![verify node v](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/b5f95ca1-ed13-4c69-9722-fac202d450a6)

Application Code Setup
Create a new directory for your To-Do project:

> mkdir Todo

Run the command below to verify that the Todo directory is created with ls command

> ls

![create todo folder](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/3a6cd372-1114-4e57-aa67-395a0283287c)

Now change your current directory to the newly created one:

> cd Todo

Next, you will use the command npm init to initialise your project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.

> npm init

![create file package json](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/2c9cf6be-bea9-4890-8738-2dcd13d395fb)

Run the command ls to confirm that you have package.json file created.

![confirmed package json](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/19b63756-9798-4506-b243-980363ace0b6)

Next, we will Install ExpressJs and create the Routes directory.








