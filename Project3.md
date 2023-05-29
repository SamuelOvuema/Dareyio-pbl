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


## Install ExpressJS
Remember that Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of your application based on HTTP methods and URLs.

To use express, install it using npm:

> npm install express

Now create a file index.js with the command below

> touch index.js

Run ls to confirm that your index.js file is successfully created

![install express](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/90838384-b8a1-4210-9ed5-4adf207bb9e5)

Install the dotenv module

> npm install dotenv


Open the index.js file with the command below

> vim index.js

simply paste the code below into the file.

> const express = require('express');
require('dotenv').config();
const app = express();
const port = process.env.PORT || 5000;
app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});
app.use((req, res, next) => {
res.send('Welcome to Express');
});
app.listen(port, () => {
console.log(`Server running on port ${port}`)
});

![todo index js](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/d44ed8ff-50a8-45fc-9487-cedf27aad862)

Notice that we have specified to use port 5000 in the code. This will be required later when we go on the browser.

Use :w to save in vim and use :qa to exit vim

Now it is time to start our server to see if it works. Open your terminal in the same directory as your index.js file and type:

node index.js

If every thing goes well, you should see Server running on port 5000 in your terminal.


















