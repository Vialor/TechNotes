**database**
	**collection**
		**object**/**document** has an `_id`, which is unique within the collection

```Bash
# mongosh connects to a mongodb server, the interaction is sh-like
mongosh --host=mongo-csgy-6513-spring.db --authenticationDatabase=bigdata -u yz10590 -p bigdata
client = MongoClient(
    "mongodb://yz10590:bigdata@mongo-csgy-6513-spring.db:27017/?authSource=bigdata"
)

# db
show dbs # show all dbs
db # current db
use <db_name> # switch to a db, create one if necessary
db.getCollectionNames()

# collection
## collection will be created if necessary for the action
db.<collection>.insert
db.<collection>.insertOne
db.<collection>.insertMany
db.<collection>.find().forEach(printjson)
```


```python
from bson import ObjectId

collection.find({"_id": ObjectId("67f853b9472e4243daf46e16")})
```