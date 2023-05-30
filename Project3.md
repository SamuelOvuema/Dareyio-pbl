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

![create index js Install dotenv](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/2e956a81-8fb6-4580-9db2-161e7fe125d0)

Now we need to open this port in EC2 Security Groups and create an inbound rule to open TCP port 5000, like this:

![inbound rules](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/40dd534d-2c30-412d-8461-93c18dc44d65)

Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000:

> http://<PublicIP-or-PublicDNS>:5000

![Welcome to Express](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/9c4d50f4-7700-4a2b-b331-97ea5da89b43)

Routes
There are three actions that our To-Do application needs to be able to do:

1. Create a new task
2. Display list of all tasks
3. Delete a completed task
Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.

For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes

> mkdir routes

Tip: You can open multiple shells in Putty or Linux/Mac to connect to the same EC2

Change directory to routes folder.

> cd routes

Now, create a file api.js with the command below

> touch api.js

Open the file with the command below

> vim api.js

![api js](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/8ed24794-3e76-41e4-8cef-a7d26c515b34)

Copy below code in the file. (Do not be overwhelmed with the code)

> const express = require ('express');
const router = express.Router();
router.get('/todos', (req, res, next) => {
});
router.post('/todos', (req, res, next) => {
});
router.delete('/todos/:id', (req, res, next) => {
})

module.exports = router;  

![create routes](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/7b6fe72b-0774-4ab4-b653-be3d670f81c2)

Moving forward let create Models directory.

## MODELS

Now comes the interesting part, since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model.

A model is at the heart of JavaScript based applications, and it is what makes it interactive.

We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each Mongodb document. **(Seems like a lot of information, but not to worry, everything will become clear to you over time. I promise!!!)**

In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as *virtual properties*

To create a Schema and a model, install *mongoose* which is a Node.js package that makes working with mongodb easier.

Change directory back Todo folder with cd .. and install Mongoose

> npm install mongoose
Create a new folder models :

> mkdir models
Change directory into the newly created ‘models’ folder with

> cd models

![install mongoose](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/7136745f-40f2-4999-9dbe-393e3f53fcf8)

Inside the models folder, create a file and name it todo.js

> touch todo.js

Tip: All three commands above can be defined in one line to be executed consequently with help of && operator, like this:  
  
> mkdir models && cd models && touch todo.js  

Open the file created with vim todo.js then paste the code below in the file:  

![vim todo js](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/00fcb41f-1fbb-4889-8090-5efe8113c4c1)

> const mongoose = require('mongoose');
const Schema = mongoose.Schema;
//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})
//create model for todo
const Todo = mongoose.model('todo', TodoSchema);
module.exports = Todo;  

![mongoose todo](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/05187399-93fe-4c05-925a-0224e1bdc8b8)

Now we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.

In Routes directory, open api.js with vim api.js, delete the code inside with :%d command and paste there code below into it then save and exit
  
> const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');
router.get('/todos', (req, res, next) => {
//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});
router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});
router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})
module.exports = router;  

![api in routes dir](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/3963d987-e41b-4ea0-a860-dc5b52324947)  

The next piece of our application will be the MongoDB Database  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
