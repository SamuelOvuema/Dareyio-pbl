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

If everything goes well, you should see the Server running on port 5000 in your terminal.

![create index js Install dotenv](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/2e956a81-8fb6-4580-9db2-161e7fe125d0)

Now we need to open this port in EC2 Security Groups and create an inbound rule to open TCP port 5000, like this:

![inbound rules](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/40dd534d-2c30-412d-8461-93c18dc44d65)

Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000:

> http://<PublicIP-or-PublicDNS>:5000

![Welcome to Express](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/9c4d50f4-7700-4a2b-b331-97ea5da89b43)

Routes
There are three actions that our To-Do application needs to be able to do:

1. Create a new task
2. Display a list of all tasks
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
  
MongoDB Database
We need a database where we will store our data. For this, we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, you will need to sign up for a shared clusters free account, which is ideal for our use case. Sign up here. Follow the sign-up process, select AWS as the cloud provider, and choose a region near you.

Complete a get-started checklist as shown in the image below  
  
![mongodb reg](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/015cd98c-b57b-4af9-bdfd-32ccfe80f900) 
  
Allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing)

![mongodb first connectn](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/c782bf4a-20f4-47f8-b5d0-a34e96310aae)

In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.

Create a file in your Todo directory and name it .env.

> touch .env
> vi .env
Add the connection string to access the database in it, just as below:  

> DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
  
Ensure to update <username>, <password>, <network-address> and <database> according to your setup  

Here is how to get your connection string

![connectn mongodb](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/e43bad86-8aef-47e4-bdb2-dbabc54ac7fc)
  
![mongodb setup](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/62c28e89-ee4a-4284-8eb4-ed4b2aaaffb0)  
  
![mongodb nodes js conectn mthd](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/36588fc1-b914-40a0-8693-51101737d2d9)  
  
![mongodb conctn string](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/4fad65f0-639f-469b-bf6c-980c862f9297)  
  
Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database.

Simply delete existing content in the file, and update it with the entire code below.

To do that using vim, follow below steps

1. Open the file with vim index.js
2. Press esc
3. Type :
4. Type %d
5. Hit ‘Enter’
The entire content will be deleted, then,

6. Press i to enter the insert mode in vim
7. Now, paste the entire code below in the file.  
  
> const express = require('express');
> const bodyParser = require('body-parser');
> const mongoose = require('mongoose');
> const routes = require('./routes/api');
> const path = require('path');
> require('dotenv').config();
>
> const app = express();
> 
> const port = process.env.PORT || 5000;
> 
> //connect to the database
> mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
> .then(() => console.log(`Database connected successfully`))
> .catch(err => console.log(err));
< 
> //since mongoose promise is depreciated, we overide it with node's promise
> mongoose.Promise = global.Promise;
> 
> app.use((req, res, next) => {
> res.header("Access-Control-Allow-Origin", "\*");
> res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
> next();
> });
> 
> app.use(bodyParser.json());
> 
> app.use('/api', routes);
> 
> app.use((err, req, res, next) => {
> console.log(err);
> next();
> });
> 
> app.listen(port, () => {
> console.log(`Server running on port ${port}`)
> }); 

![index js](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/2b835297-ac9f-4813-9eed-2a5f31ce060a)


Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.

Start your server using the command:  
  
> node index.js  

You shall see a message ‘Database connected successfully’, if so – we have our backend configured. Now we are going to test it.


![node index js](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/e9643468-1629-4e5f-bed9-4c02bde4ea67)
  
#### Testing Backend Code without Frontend using RESTful API
So far we have written the backend part of our To-Do application and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTful API. Therefore, we will need to make use of some API development client to test our code.  
  
  
In this project, we will use [Postman](https://www.getpostman.com/) to test our API.
Click [Install Postman](https://www.getpostman.com/downloads/) to download and install postman on your machine.

Click [HERE](https://www.youtube.com/watch?v=FjgYtQK_zLE) to learn how perform [CRUD operations](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) on Postman

You should test all the API endpoints and make sure they are working. For the endpoints that require body, you should send JSON back with the necessary fields since it’s what we setup in our code.

Now open your Postman, create a POST request to the API http://<PublicIP-or-PublicDNS>:5000/api/todos. 
This request sends a new task to our To-Do list so the application could store it in the database.

Note: make sure your set header key Content-Type as application/json  
  
![postman POST](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/9f10c950-f4d9-4dad-9e0b-d982a42b9100)  
  
Create a GET request to your API on http://<PublicIP-or-PublicDNS>:5000/api/todos. This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request).

![postman GET](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/fcd9441b-221c-486a-b216-0179c7e96dd2)  
  
**Optional task:** Try to figure out how to send a DELETE request to delete a task from out To-Do list.

**Hint:** To delete a task – you need to send its ID as a part of DELETE request.

By now you have tested backend part of our To-Do application and have made sure that it supports all three operations we wanted:

* Display a list of tasks – HTTP GET request

* Add a new task to the list – HTTP POST request

* Delete an existing task from the list – HTTP DELETE request

We have successfully created our Backend, now let go create the Frontend. 
  
  
### Step 2 – Frontend creation
Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.  
  
In the same root directory as your backend code, which is the Todo directory, run:  
  
> npx create-react-app client 
  
This will create a new folder in your Todo directory called client, where you will add all the react code.  
  
### Running a React App
Before testing the react app, there are some dependencies that need to be installed.  
  
1.   Install concurrently. It is used to run more than one command simultaneously from the same terminal window.

> npm install concurrently --save-dev  
  
2.  Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.  
  
> npm install nodemon --save-dev  
  
3. In the Todo folder open the package.json file. Change the highlighted part of the below screenshot and replace it with the code below.  
  
> "scripts": {..
> "start": "node index.js",..
> "start-watch": "nodemon index.js",..
> "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""..
> }, .. 

![package json script](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/6dcf4382-6c22-449a-8034-3872f2236c59)
  
Below is the new Package.json file. 
  
![New package json file](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/0fae1170-af92-4e21-ba33-415d037f84a3) 
  
Configure Proxy in package.json 
  
1.  Change directory to ‘client’
  
> cd client 
  
2. Open the package.json file
  
> vi package.json  
  
3. Add the key-value pair in the package.json file "proxy": "http://localhost:5000".  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
