```
===basic===
mongod --dbpath "E:\MongoDB\data" --logpath "E:\MongoDB\logs\mongo.log" --install -serviceName "MongoDB"
net start MongoDB

===auth===
use admin
db.createUser({user:"admin",pwd:"admin",roles:[{"role":"userAdmin","db":"admin"},{"role":"root","db":"admin"},{"role":"userAdminAnyDatabase","db":"admin"}]})
db.auth("admin","admin")
sc delete MongoDB
mongod --dbpath "E:\MongoDB\data" --logpath "E:\MongoDB\logs\mongo.log" --auth --install -serviceName "MongoDB"
net start MongoDB

mongo -u admin -p 123456 localhost:27017/admin

mongodb://<user>:<pwd>@<host>:27017/<collection>

===create user===
use <db>
db.createUser({user:"jamie956", pwd:"123456", roles:["readWrite","dbAdmin"]})
db.auth("jamie956","123456")

===db===
show dbs
db
use <db>
db.dropDatabase()
	
===collection===
show collections
db.createCollection("<collection>")
db.createCollection("<collection>", { capped : true, autoIndexId : true, size : 6142800, max : 10000 } )
db.<collection>.drop()

===document===
db.<collection>.find()
db.<collection>.find().pretty()
db.<collection>.find({key1:"val1"}).pretty()
db.<collection>.find( { $or:[{key1:"val1"},{key2:"val2"}] } ).pretty()
db.<collection>.find().count()
db.<collection>.find( {num:{$lt:<num>}} ).pretty()
db.<collection>.find().sort({<key>:<1 or -1>}).pretty()
db.<collection>.find().limit(1)
db.<collection>.insert( { [<key>:"<val>", …] } )
db.<collection>.remove({key1:"val1"})

===field===
add => db.<collection>.update( {key1:"val1"}, {$set:{key3:"val3"}} )
update => db.<collection>.update( {key1:"val1"}, {$set:{key2:"update val2"}} )
db.<collection>.update( {key1:"val1"}, {$inc:{num:10}} )
db.<collection>.update( {key1:"val1"}, {$rename:{"key3":"rename key3"}} )
remove =>  db.<collection>.update( {key1:"val1"},{$unset:{key3:1}} )

```