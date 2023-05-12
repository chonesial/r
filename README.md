# MongoDB Container 

**Table of Contents**

[Link to my paragraph](#my-paragraph)


- [Prerequisites](#prerequisites)
- [Docker](#dockcont)
- [Mongo CTL](#monctl)
- [Mondo Databse Creation](#credata) 
- [USE Syntax](#usesyn)
- [Collections in Mongo](#coll)
- [Objects in Collections](#objects)
- [Searching for Databases , Collection and objects](#search)
- [Deleting](#del)

<p id="my-paragraph">This is the paragraph I want to link to.</p>


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
//using mongo:5 as mongo:latest is unstable version of mongo 

Inside the container 

Start mongoldb Ctl by typing  “mongo” syntax

root@45ab820c7a09:/# mongo

Now Create a new Database by typing “use databasename”

> use databasename
switched to db databasename

You can view by using “show dbs” command 

Collections in Mongodb

Collection in mongoldb are the groups of objects with different sets of properties , different objects with different properties and unique values can be stored inside a collection group without enforcing any common schema as it follows in other RDBMS such as sql . 

In Mongodb you can store different object 
such as car with sets of properties like tyres, form_factor, Mileage, Fuel_norms.
And other object 
Such as movies with sets of properties like movie_name, cast_crew, release_date, Earning, Ranking .
Both of the object can be saved inside one collection group in a database 

To create a collection we can use 
db.createCollection(“my_collection_name”)

This will create a new collection named : my_collection_name inside the above mentioned database .

Inserting Objects inside Collection 
To insert objects and values inside said collection , we can use the command 

db.my_collection_name.insertOne({car_name: “Tata_nexon”, tyre_sixe: “16 inches”, form_factor: “SUV”, Mileage: “20 kmpl, Fuel_norms: “bs6”})

Above command will store object will defined property set inside mentioned database and declared collection “my_collection_name”

The syntax insertOne define a single object that is getting saved in collection . 
For multiple objects we can use insertMany .

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
This will allow inserting many objects will different properties inside a collection .

 create new databases using use command .

use database1 
			it will create a database with name database 1

use database 2 
			it will create a database with name database 1

use database 3 
			it will create a database with name database 1

Now we can list all the database using 
Show dbs 

Use command also used for switching to databases.

And similarly we can create collections after switching to preffered database .

db.createCollection(“my_collection_1”)
db.createCollection(“my_collection_2”)
db.createCollection(“my_collection_3”)
db.createCollection(“my_collection_4”)
db.createCollection(“my_collection_5”)

And similarly we use insertOne and insert Many commands to insert objects inside .

We can use show dbs to view all databases and switch to preferred database by “use” syntax. 

> show dbs
Output
admin         0.000GB
config        0.000GB
local         0.000GB
sunildatabse  0.000GB
> use sunildatabse
switched to db sunildatabse

Similarly we can view all the collections after switching by using “show collection” syntax.

> show collections
output
marvel-movies
moviename
mycollection


Similarly we can view all the contents inside collection by using “find”

> db.mycollection.find()
Output
{ "_id" : ObjectId("64535a6472aa3d174a91fe3a"), "name" : "John Doe", "age" : 30 }

> db.moviename.find()
Output
{ "_id" : ObjectId("64535c7072aa3d174a91fe3b"), "name" : "Avengers", "Realeased_date" : "2020", "Rank" : "1" }
{ "_id" : ObjectId("64535c8872aa3d174a91fe3c"), "name" : "RRR", "Realeased_date" : "2022", "Rank" : "1" }
{ "_id" : ObjectId("64535e7772aa3d174a91fe3d"), "name" : "Black_Panther", "Realeased_country" : "USA", "Realeased_DATE" : "2016", "cast" : [ "Scarlette Johson", "Spiderman", "antman" ], "Rank" : "1" }

We can also use find to view only relevant result 

> db.moviename.find({name: "RRR"})
Output
{ "_id" : ObjectId("64535c8872aa3d174a91fe3c"), "name" : "RRR", "Realeased_date" : "2022", "Rank" : "1" }




Managing the Assets 

Removing/Deleting (Objects, Collections and Database)

WE have several syntaxes for removing inserted values inside mongoDB, that are used to delete records , removing collections and dropping the Databases 

Delete the objects inside Collections

To delete an object or a document from a collection in MongoDB, you can use the deleteOne() or deleteMany() methods.

Deleteone method - for deleting single entry 

db.moviename.deleteone({name:”Avengers”})
This will delete the first object that matches the filter criteria 



Deletemany 
db.moviename.deletemany({tyre_size:”16 inches”})
This will delete the all objects that matches the filter criteria 




Deleting the collection
db.collectionName.drop()



Deleting the database 
   First use that database for switching and then delete by using 

db.dropDatabase()



