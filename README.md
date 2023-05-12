# MongoDB Container 

**Table of Contents**

[Link to my paragraph](#my-paragraph)


- [Prerequisites](#prerequisites)
- [Docker](#dockcont)
- [Mongo CTL](#monctl)
- [MongoDb Basic Commands](#usesyn)
- [Mondo Databse Creation](#credata) 
- [Collections in Mongo](#coll)
- [Objects in Collections](#objects)
- [Management of Assets](#search)


## Prerequisites 
<p id="Prerequisites">

Docker for running the container.
Docker-compose file 
Docker file   
or 
you can use official Image  
 </p>

## Creating Container using docker compose
<p id="dockcont">

#### docker-compose.yml
```
version: '3'
services:
  mongo:
    image: mongo:5
    ports:
      - "127.0.0.1:27017:27017"
```
	
#### using mongo:5 official image beacause mongo:latest is unstable version of mongo.

## MongoDb Command Line tool 

After Checking inside the MongoDb , We need to switch to MongoDb Command Line Tool in order to perform MongoDb related operations. 

####

<p id="monctl">

#### Start mongoldb Ctl by typing  “mongo” syntax

```
root@45ab820c7a09:/# mongo
```

Now Create a new Database by typing “use databasename”

## MongoDb Basic Commands to Begin With 
<p id="usesyn">These are some basic commands that we will use as we move forward operating the databse</p>

- show dbs - shows a list of all the databases in your MongoDB instance.
- use <database_name> - switches to the specified database. If the database doesn't exist, it will be created when you first save data   to it.

- db.dropDatabase() - deletes the current database.

- show collections - shows a list of all the collections in the current database.

- db.createCollection(<collection_name>) - creates a new collection in the current database with the specified name.

- db.<collection_name>.insertOne(<document>) - inserts a new document into the specified collection.

- db.<collection_name>.find() - retrieves all documents in the specified collection.

- db.<collection_name>.find(<query>) - retrieves documents that match the specified query in the specified collection.

- db.<collection_name>.updateOne(<query>, <update>) - updates the first document that matches the specified query in the specified        collection with the specified update.

- db.<collection_name>.updateMany(<query>, <update>) - updates all documents that match the specified query in the specified collection   with the specified update.

- db.<collection_name>.deleteOne(<query>) - deletes the first document that matches the specified query in the specified collection.

- db.<collection_name>.deleteMany(<query>) - deletes all documents that match the specified query in the specified collection.


## Creation and Managing the Database 
<p id="credata">This is the paragraph I want to link to.</p>

### Create a Database, Named "databsename"
 	
```
> use databasename
switched to db databasename
```


### The Use command will Automatically create and switch to the databse. 

 create new databases using use command .


```
use database1 
```			

it will create a database with name database 1 and switch automatically switch the databse for operations 


```
use database 2 
```			
	
it will create a database with name database 2 and switch automatically switch the databse from 1 for operations on 2


	
```
use database 3 
```
	
it will create a database with name database 2 and switch automatically switch the databse from 2 for operations on 3



Now we can list all the database using 

#### Show dbs 

Use command also used for switching to databases.
In Order to View all the Databses Present in MongoDb User.

You can view by using “show dbs” command 

```
show dbs 
```
	
#### The Output will show something like this 

```
Output
admin         0.000GB
config        0.000GB
local         0.000GB
sunildatabse  0.000GB
```
	
## Collections in Mongodb

<p id="coll">After the Creation of Database and login , we move forward for creating a collection in which we will store our objects as entries.</p>

Collection in mongoldb are the groups of objects with different sets of properties , different objects with different properties and unique values can be stored inside a collection group without enforcing any common schema as it follows in other RDBMS such as sql . 


To create a collection we can use 

	
```
db.createCollection(“my_collection_name”)
```

This will create a new collection named : my_collection_name inside the above mentioned database .
	
And similarly we can create collections after switching to preffered database .

	
	
```
db.createCollection(“my_collection_1”)
db.createCollection(“my_collection_2”)
db.createCollection(“my_collection_3”)
db.createCollection(“my_collection_4”)
db.createCollection(“my_collection_5”)
```	
Similarly we can view all the collections after switching to preffered database by using “show collection” syntax.

### start from begining 	
```
> show dbs
```

output 

```
admin         0.000GB
config        0.000GB
local         0.000GB
sunildatabse  0.000GB
```
	
	
#### Now write use dbs for switching to preffered database	

```
> use sunildatabse
```	
#### you will switch to sunildatabase and now write below command
	
```
show collections	
```	
#### the output will be as follow 

```
marvel-movies
moviename
mycollection
```	
	
## Inserting Objects inside Collection 

<p id="objects">This is the paragraph I want to link to.</p>
	
In Mongodb you can store different object, 
such as car with sets of properties like tyres, form_factor, Mileage, Fuel_norms.
And other object 
	
```
db.my_collection_name.insertOne({car_name: “Tata_nexon”, tyre_sixe: “16 inches”, form_factor: “SUV”, Mileage: “20 kmpl, Fuel_norms: “bs6”})
```
	
or movies with sets of properties like movie_name, cast_crew, release_date, Earning, Ranking .
Both of the object can be saved inside one collection group in a database 	

	
	
### To insert objects and values inside said collection , we can use insert command 
	
```

db.my_collection_name.insertOne({car_name: “Tata_nexon”, tyre_sixe: “16 inches”, form_factor: “SUV”, Mileage: “20 kmpl, Fuel_norms: “bs6”})

```
	
	
Above command will store object with defined property set inside mentioned database under declared collection “my_collection_name”

	
The syntax insertOne define a single object that is getting saved in collection . 
For multiple objects we can use insertMany .

	
```
db.my_collection_name.insertMany([
{
 movie_name: “RRR”,
cast_crew: [“director”, “Actor”, “Singer”, “Producer”],
release_date: “2022”,
Earning: “900 crores”,
Ranking: “rank1”
}
{
car_name: “Tata_nexon”, 
tyre_sixe: “16 inches”, 
form_factor: “SUV”, 
Mileage: “20 kmpl, 
Fuel_norms: “bs6”
}
]
)
```
	
This will allow inserting many objects with different properties inside a collection .
 
#### we can view all the contents inside collection by using “find”

```
> db.mycollection.find()
Output
{ "_id" : ObjectId("64535a6472aa3d174a91fe3a"), "name" : "John Doe", "age" : 30 }
```

	
```
db.moviename.find()
```
#### This generate following output 

```	
Output
{ "_id" : ObjectId("64535c7072aa3d174a91fe3b"), "name" : "Avengers", "Realeased_date" : "2020", "Rank" : "1" }
{ "_id" : ObjectId("64535c8872aa3d174a91fe3c"), "name" : "RRR", "Realeased_date" : "2022", "Rank" : "1" }
{ "_id" : ObjectId("64535e7772aa3d174a91fe3d"), "name" : "Black_Panther", "Realeased_country" : "USA", "Realeased_DATE" : "2016", "cast" : [ "Scarlette Johson", "Spiderman", "antman" ], "Rank" : "1" }```
```

	
We can also use find to view only relevant result 

```
> db.moviename.find({name: "RRR"})
Output
{ "_id" : ObjectId("64535c8872aa3d174a91fe3c"), "name" : "RRR", "Realeased_date" : "2022", "Rank" : "1" }
```

## Managing the Assets 
<p id="search">This is the paragraph I want to link to.</p>

Removing/Deleting (Objects, Collections and Database)

WE have several syntaxes for removing inserted values inside mongoDB, that are used to delete records , removing collections and dropping the Databases 

Delete the objects inside Collections

To delete an object or a document from a collection in MongoDB, you can use the deleteOne() or deleteMany() methods.

Deleteone method - for deleting single entry 

	
```db.moviename.deleteone({name:”Avengers”})
```
	

	
This will delete the first object that matches the filter criteria 

### Delete all objects related to tag

```
Deletemany 
db.moviename.deletemany({tyre_size:”16 inches”})
```

This will delete the all objects that matches the filter criteria 




### Deleting the collection

```
db.collectionName.drop()
```
This will delete the collection


### Deleting the database 
   
#### First use that database for switching and then delete by using 

```
db.dropDatabase()
```
This will Delete the Database you are working on 


