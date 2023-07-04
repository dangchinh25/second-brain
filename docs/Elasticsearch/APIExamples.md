# APIs
### Create an index
- Use PUT API to create a new [[Concepts#Index| index]]
- In the following query, we define some basic settings like number of [[Concepts#Shards | shards]] and number of [[Concepts#Replicas| replicas]] for our index `traveler`. Along with this, we define that each of our document can have 4 fields, *name* and *nationality* of *keyword* type, *age* of *integer* type and *background* of *text* type
```
PUT traveler  
{  
	"settings":{  
		"number_of_shards":5,  
		"number_of_replicas":2  
	},  
	"mappings":{  
		"properties":{  
			"name":{  
				"type":"keyword"  
			},  
			"age":{  
				"type":"integer"  
			},  
			"background":{  
				"type":"text"  
			},  
			"nationality":{  
				"type":"keyword"  
			}  
		}  
	}  
}
```
### Insert a document
- Now that we have defiend the schema for our index, let's insert a sample document. In this, we specify the index name, a unique *_id* for our document and *name*, *age*, *nationality* and *background* field values
```
PUT traveler/_doc/1  
{  
	"name":"John Doe",  
	"age":"23",  
	"background":"Born and brought up in California. Engineer by profession. Loves to cook",  
	"nationality":"British"  
}
```
### Retrieve a document
- Use *GET API* on `travelver` index and specify the document's unique *_id*
```
GET traveler/_doc/1
```
### Delete a document
```
DELETE traveler/_doc/1
```

### Delete an index
```
DELETE traveler
```
### Get list of all indices present in cluster
```
GET /_cat/indices
```
### Get information to a specific index
```
GET traveler
```

# Search APIs
### Optional Attributes that can be specified in any search query
```
timeout_: A search timeout binds the search request to be executed within the specified time value and bail with the hits accumulated up to that point when expired. Search requests are cancelled after the timeout is reached.

from: It retrieves hits from a certain offset. Defaults to 0.

size: It returns the number of hits. Defaults to 10.

source: It specifies which fields of each matched document are to be displayed.
```
#### Find all query
- Gets all documents present in an index
```
GET traveler/_search  
{  
	"size":2,  
	"timeout":"30s",  
	"query":{  
		"match_all":{}  
	}  
}
```
#### Get total count
```
GET traveler/_count
```
#### Match Query
- Used to retrive all documents from an index or a set of indices which matcha. set of specific criteria
```
GET read_alias/_search  
{  
	"query":{  
		"match":{  
			"background":"brought up California Loves cook"  
		}  
	}  
}
```
- The value of *background* is analysed using the same analyser which was defined for *background* attribute at the time of *mapping*. Therefore, the value is processed into `["brough", "up", "California", "loves", "cook"]`. Each of the tokens are matched with the tokens created at the time of indexing the original documents. If any of the tokens match, then the corresponding documents is returned
### Term Query
- This is used for fetching documents that can contain an exact term in a provided field. It is applied on fields with *keyword*, *numeric*, *date*, *boolean* types. Use of term query with *text* field should be avoided as those fileds are analysed and then stored so it is difficult to find an exact match.
- The following query will return all documents whose *name* field has an exact value as "John Doe"
```
GET read_alias/_search  
{  
	"query":{  
		"term":{
			"name": {
				"value": "John Doe"
			} 
		}  
	}  
}
```

#### Prefix Query
```
GET read_alias/_search  
{  
	"query":{  
		"prefix":{
			"name": {
				"value": "Joh"
			} 
		}  
	}  
}
```
#### Regex Query
```
GET read_alias/_search  
{  
	"query":{  
		"regexp":{
			"name": {
				"value": "J.*e*"
			} 
		}  
	}  
}
```