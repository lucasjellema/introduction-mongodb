# Introduction to MongoDB - Create and Query Data

Assumption: running in Gitpod workspace: [![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/lucasjellema/introduction-mongodb); this workspace contains MongoDB, mongosh (MongoDB Shell), VS Code with MongoDB Extension. 

## Work with MongoDB Shell (mongosh)

First we will work with the simple straightforward CLI for MongoDB. 

### First steps with shell

Open shell: 

```
mongosh
```

To display the database you are using, type `db`. The operation should return test, which is the default database.

To list the databases available to the user, use `show dbs`.

To create a new database, issue the `use <db>` command with the database that you would like to create. For example, the following commands create both the database *myNewDatabase* and the collection *myCollection* using the insertOne()
operation:

```
use myNewDatabase
db.myCollection.insertOne( { x: 1 } );
```
See docs: [Mongo DB Shell- Run Commands](https://www.mongodb.com/docs/mongodb-shell/run-commands/)

### Load Data
Let's load a data Set from CSV files into  MongoDB

Directory hr-queries contains the data sets in two CSV files: *export_dept.csv* and *export_emp.csv*. These are classic datasets for anyone with a history in the world of Oracle. Two helper files - *empFields.txt* and *deptFields.txt* – specify how the *mongoimport* utility should map the fields in each record to document properties in the MongoDB collection.

Using the following commands – running the monogimport tool in a new terminal window – you import the datasets into collections *emp* and *dept* in database called *hr*. The database and the collections will be dropped (if they already exist) and (re)created through these commands.

```
mongoimport --host 127.0.0.1:27017 --db hr --collection emp --drop --file export_emp.csv  --type csv --fieldFile empFields.txt
mongoimport --host 127.0.0.1:27017 --db hr --collection dept --drop --file export_dept.csv  --type csv --fieldFile deptFields.txt
```

### Querying Data

Start the MongoDB shell as you have done before. Switch to database hr:

```
use hr
```

Verify whether the collections were created:
```
show collections
```

Check the statistics for collection emp:
```
db.emp.stats()
```
Check the contents for both collections:
```
db.emp.find()
db.dept.find()
```

To get started, let’s find the names of all managers:

```
db.emp.find({"JOB":"MANAGER"},{ENAME:1})
```

And – find all salesmen, listed by salary descending; only show top 2

```
db.emp.find({"JOB":"SALESMAN"},{ENAME:1, SAL:1}).sort({'SAL':-1}).limit(2)
```

Introducing lookup in MongoDB: list all employees (empno and ename) along with the looked up department

```
db.emp.aggregate(
[  {$lookup:
      {
         from:"dept",
         localField:"DEPTNO",
         foreignField:"deptno",
         as:"dept"
     }
   }  
, {$project: {
        "EMPNO": 1,
        "ENAME": 1,
        "DEPT": { $arrayElemAt:["$dept", 0]},
      }
    }
]
)
```

### Some more advanced queries

The file *sample-hr-queries.txt* contains a number of queries, of increasing complexity, that you can try out to see what MongoDB can do in terms of retrieving information. 

You will come across:
* aggregation
* join & lookup
* nested collections
* geo spatial search (cities closest to…)
* text search
* materialized views
* stored procedures
* facet search


Note: You can compare the MongoDB search commands with their SQL counterparts in the presentation: https://www.slideshare.net/lucasjellema/comparing-30-mongodb-operations-with-oracle-sql-statements .




## Connect to the Local MongoDB instance

Click on the MongoDB extension icon in VS Code toolbar. Then click on the Connect button. 

![](images/connect-through-vscode-extension.png)  

Connect the MongoDB VS Code Extension to the local MongoDB database using this connection string: `mongodb://localhost:27017/` . Paste your connection string  `mongodb://localhost:27017/` into the Command Palette. Press the Enter or Return key.

You will now be connected. The connection is presented in the explorer and can be expanded to peek inside the database.

![](images/connection-navigator.png)  

Check the docs: [MongoDB for VS Code - Connect to Your MongoDB Deployment](https://www.mongodb.com/docs/mongodb-vscode/connect/)


## Navigate the data

Check the docs: [Navigate Your Data](https://www.mongodb.com/docs/mongodb-vscode/databases-collections/)

## Using MongoDB Compass GUI

Mongo Compass is a very nice and free GUI tool that allows you to work with MongoDB - for operational database management and for data analysis, querying and manipulation. Let's work with Compass for a moment - a welcome change from the shell. Compass runs as a native desktop application and can connect to local and remote MongoDB instances. Compass is available for Windows, MacOS and Linux. 

Download and install the appropriate version for your platform: [MongoDB Tools Download page](https://www.mongodb.com/try/download/compass)

In order to connect to the MongoDB instance running in your Gitpod workspace, you have connect your local VS Code over SSH to the remote workspace (see next section). Once that is done, the connection to the MongoDB instance can be made from Compass using the local connectstring `mongodb://localhost:27017/`.

### Run Local VS Code with Gitpod workspace

Try to open a workspace in your local VS Code desktop for the smoothest dev experience (using port forwarding for services and web applications). 
![image](https://user-images.githubusercontent.com/1296324/202265821-1faa9cc3-21fa-4cc2-8add-163ec5b83e9b.png)

You need the MS Remote SSH extension for VS Code:
![image](https://user-images.githubusercontent.com/1296324/202265701-ffe9492d-ff60-40e7-96c5-203d1ecedb70.png)

Also install the VS Code extension Gitpod Remote
![image](https://user-images.githubusercontent.com/1296324/202265866-5ac13ef4-3db9-4f2f-9c42-ca6ab900dcc6.png)

Open the command palette in VS Code in the browser. Select: *Gitpod - Open in VS Code*.
![image](https://user-images.githubusercontent.com/1296324/202265911-37a26892-3d50-4258-95bc-d8cfc18d537c.png)

You will be asked:
* Open Visual Studio Code?
* Copy the this temporary password
* Allow an extension to open this URL?
* Enter password for remote connection

as shown in this picture.
![image](https://user-images.githubusercontent.com/1296324/202266010-244eeff3-1a53-4d4d-a997-64eb159f8c0a.png)

Please comply and provide as requested.

You can now work locally against the CDE (Cloud Development Environment) - the cloud based Gitpod workspace environment. Try to type in the local editor and quickly check the browser based editor - and vice versa. Be really quick! They seem pretty well synchronized.

The MongoDB port (27017) that is opened in the workspace is forwarded. This means that when you access  `localhost:27017` on your laptop, the requests are forwarded correctly over the SSH connection to the Gitpod workspace environment, ending up on the MongoDB instance running in the workspace.

This figure shows the port-forwarding configuration in your local VS Code

![](images/port-forwarding-mongodbport.png)  


### First steps with Compass

Connect
List databases
List collections
Show documents