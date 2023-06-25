## MEAN STACK IMPLEMENTATION

Now, that you have already learned how to deploy LAMP, LEMP and MERN Web stacks – it is time to familiarise yourself with the MEAN stack and deploy it to the Ubuntu server.

MEAN Stack is a combination of the following components:

1. MongoDB (Document database) – Stores and allows retrieval of data.
2. Express (Back-end application framework) – Makes requests to Database for Reads and Writes.
3. Angular (Front-end application framework) – Handles Client and Server Requests
4. Node.js (JavaScript runtime environment) – Accepts requests and displays results to end user

Step 0 – Preparing prerequisites
In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.

If you do not have an AWS account – go back to Project 1 Step 0 to sign in to AWS free tier account and create a new EC2 Instance of the t2.nano family with an Ubuntu Server 20.04 LTS (HVM) image. Remember, you can have multiple EC2 instances, but make sure you STOP the ones you are not working with at the moment to save available free hours.

![select Ubuntu](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/7b387155-5411-4ec5-8e76-b25d3e498896)

![t2nano instance type](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/875e7240-6bdb-4dcf-8b7f-4ab3a8f9fb91)

![launch instance t2nano](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/09fc0b1a-8b4c-4583-bc64-ba9bbe04702c)

Task
In this assignment, you are going to implement a simple Book Register web form using MEAN stack.

Step 1: Install NodeJs
Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.

Update ubuntu
```bash
sudo apt update
```

![sudo apt upgrade p4](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/bfc3bae2-c34a-47d6-b9ce-fed999c26d30)

Upgrade ubuntu
```bash
sudo apt upgrade
```
![sudo apt upgrade2](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/1b2eb832-4ccc-40e4-bba0-0f0385ad96f1)

Add certificates
```bash
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates

curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

Install NodeJS
```bash
sudo apt install -y nodejs
```
Step 2: Install MongoDB

MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.
```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```
```bash
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```
Install MongoDB
```bash
sudo apt install -y mongodb
```
Start The server
```bash
sudo service mongodb start
```
Verify that the service is up and running
```bash
sudo systemctl status mongodb
```
Install npm – Node package manager.

sudo apt install -y npm
Install body-parser package

We need ‘body-parser’ package to help us process JSON files passed in requests to the server.
```bash
sudo npm install body-parser
```
Create a folder named ‘Books’
```bash
mkdir Books && cd Books
```
In the Books directory, Initialize npm project
```bash
npm init
```
Add a file to it named server.js
```bash
vi server.js
```

Copy and paste the web server code below into the server.js file.

```bash
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
```
![vi server js](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/487e389d-d548-4835-8237-dbaa4ac1dd20)

### Step 3: Install Express and set up routes to the server

Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use Express to pass book information to and from our MongoDB database.

We also will use the Mongoose package which provides a straightforward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.
```bash
sudo npm install express mongoose
```
In the ‘Books’ folder, create a folder named apps
```bash
mkdir apps && cd apps
```
Create a file named routes.js
```bash
vi routes.js
```
Copy and paste the code below into routes.js

```bash
const Book = require('./models/book');

module.exports = function(app){
  app.get('/book', function(req, res){
    Book.find({}).then(result => {
      res.json(result);
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while retrieving books');
    });
  });

  app.post('/book', function(req, res){
    const book = new Book({
      name: req.body.name,
      isbn: req.body.isbn,
      author: req.body.author,
      pages: req.body.pages
    });
    book.save().then(result => {
      res.json({
        message: "Successfully added book",
        book: result
      });
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while saving the book');
    });
  });

  app.delete("/book/:isbn", function(req, res){
    Book.findOneAndRemove(req.query).then(result => {
      res.json({
        message: "Successfully deleted the book",
        book: result
      });
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while deleting the book');
    });
  });

  const path = require('path');
  app.get('*', function(req, res){
    res.sendFile(path.join(__dirname, 'public', 'index.html'));
  });
};
```
![routes js](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/89961bd4-c01b-470f-9897-9dcd6cf9996a)

In the ‘apps’ folder, create a folder named models
```bash
mkdir models && cd models
```
Create a file named book.js
```bash
vi book.js
```
Copy and paste the code below into ‘book.js’
```bash
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);
```
![book js](https://github.com/SamuelOvuema/Dareyio-pbl/assets/132525203/ac1d50ed-2945-4800-816e-52a8c4129455)

```bash
Step 4 – Access the routes with AngularJS
```
AngularJS provides a web framework for creating dynamic views in your web applications. In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book register.

Change the directory back to ‘Books’
```bash
cd ../..
```
Create a folder named public
```bash
mkdir public && cd public
```
Add a file named script.js
```bash
vi script.js
```
Copy and paste the Code below (controller configuration defined) into the script.js file.

var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});















































