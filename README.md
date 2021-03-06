# Node-Examples
**What does each folder mean??**
_______
#### Simple Example [folder]
* simplerect.js
  Simply for illustration purposes. No modules, just code
* rect-1.js, solve-1.js
  Same code as in simplerect.js. Difference is that we've introduced modules
* rect-2.js, solve-2.js
  A more complex example that includes callbacks, closures, and error handling. Still the same code output as above.
* solve-3.js
  An even more complex example that utilizes yargs node module to allow command line input. Instead of hard coding the variables we now input them in command line
  
#### Node-Http [folder]
* server-1.js, server-2.js
  Simple HTML server that returns html files from a folder
  
#### Node-Express [folder]
* Utilize application routes and express router in Express framework to support a restful API

#### Con-fusion-app [folder]
* Created 3 separate Node Modules implementing an express router to support the rest API

#### Node-Express-Gen [folder]
* Utilized the express generator to build the same rest API as above, but much quicker and easier. Steps:
1. npm install express-generator -g
2. express node-express-gen
3. npm install
4. npm start

** How to install Mand run MongoDB with the Node-Express-Gen Folder above:**
* Go to http://www.mongodb.org, then download and install MongoDB as per the instructions given there.
* Create a folder named mongodb on your computer and create a subfolder under it named data.
* Move to the mongodb folder and then start the MongoDB server by typing the following at the prompt: ```mongod --dbpath=data```
* Open another command window and then type the following at the command prompt to start the mongo REPL shell: ```mongo```
* The Mongo REPL shell will start running and give you a prompt to issue commands to the MongoDB server. At the Mongo REPL prompt, type the following commands one by one and see the resulting behavior: ``` db
     use conFusion
     db
     db.help() ```
* You will now create a collection named dishes, and insert a new dish document in the collection: 
``` db.dishes.insert({ name: "Uthapizza", description: "Test" }); ```
* Then to print out the dishes in the collection, type:
``` db.dishes.find().pretty(); ```
* Next, we will learn the information encoded into the ObjectId by typing the following at the prompt: 
``` var id = new ObjectId();
     id.getTimestamp(); ```


#### Node-mongodb [folder]
* simpleserver.js
* Utilize the node MongoDB driver module and configure node application to communicate with the MongoDB server. 

** How to Use it**
* ```npm install```
* navigate to mongodb folder and run ```mongod --dbpath=data```
* navigate to node-mongodb folder and run ```node simpleserver```

#### Rest-Server [folder]
* Integrated the REST API server based on the Express framework that we implemented earlier, together with the Mongoose schema and models that we developed to create a full-fledged REST API server
* To build this we started by scaffolding out an Express application using ```express rest-server```
* We then added in our ```app.js``` file from the node-express-gen folder.
* We then added in our routes from node-express-gen: ```dishRouter.js promoRouter.js leaderRouter.js```
* We then copied in our models
* Then ran ```npm install``` and ```npm install mongoose mongoose-currency --save``` to add them to our dependency list.
* After wiring it all together with some code changes, we are good to go!

**To run**
* navigate to your database: ```mongod --dbpath=data```
* navigate to rest-server: ```npm-start```
* Open postman and run your post/get/put/delete requests to test

#### Basic-Auth [folder]
* An Express server to handle basic authentication
* Set up the Express application to send signed cookies.
* Set up the Express server to use Express sessions to track authenticated users

------

#### Rest-Server-Passport [folder]
**This folder is the production folder and puts all of our learning together**
* Use JSON web tokens for token-based user authentication
* Use Passport module together with passport-local and passport-local-mongoose for setting up local authentication within your server.
```
npm install
```

### How to use the database
**You need postman**
```
//First we need to create our users. Run two post requests:
post: localhost:3000/users/register
```
To create a regular user, send raw json in the body of the request with the text:
```
{"username":"user", "password":"pass", "firstname":"brandon", "lastname":"morelli"}
```
To create an admin user, do the same:
```
{"username":"Admin", "password":"pass", "firstname":"Abrandon", "lastname":"Amorelli"}
```
Now lets make our admin user an actual admin. Go to terminal and start mongo shell
```
mongo

//switch to our database
use conFusion

//set to admin status
db.users.update({username:"Admin"},{$set:{admin:true}});

//now find all users
db.users.find().pretty()
```

Head back to postman and login:
post: localhost:3000/users/login
body, raw, json: {"username":"Admin", "password":"pass"}

Our token will be returned. Copy and save the token

Now we can look at a list of registered users:
get:localhost:3000/users/

In the header set x-access-token to our token value from the previous step.
This will return our array of users. 

Still logged in as an admin, we can post dishes to the database:
post: localhost:3000/dishes/
```
{
"name": "dishName",
"image": "dishURL",
"category": "dishCat",
"label": "dishLabel",
"price": "1.99",
"description": "dish description paragraph lorem ipsum"
}
```

Now, we can login as an ordinary user and comment on a dish:
1. Login as normal user
2. Use token to do a get: localhost:3000/dishes/
3. Copy a dishId you want to comment on
4. post: localhost:3000/dishes/**dishID**/comments
```
{
    "rating": 5,
    "comment": "anything you want to say goes here"
}
```

5. Now if we do a get: localhost:3000/dishes/ our comment will be displayed AND it will show all user information about the user that created that comment

### How to run the HTTPS Server:
* First navigate to /bin and create your private key and certificate
```
openssl genrsa 1024 > private.key
openssl req -new -key private.key -out cert.csr
openssl x509 -req -in cert.csr -signkey private.key -out certificate.pem
```
* Then run ``` npm start ``` and navigate to ``` https://localhost:3443 ```

### How to use Facebook OAuth Support:
* Go to ``` https://developers.facebook.com/apps/ ``` and register your application. Be sure to add your callback url ``` https://localhost:3443/users/facebook/callback ``` under the settings tab.
* Update your ```config.js``` file with your new ```clientID```, and ```clientSecret```
* Now you should be able to login via ``` https://localhost:3443/users/facebook ```
