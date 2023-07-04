# Major Concepts
### Cluster
- A cluster is a collection of one or more nodes that holds the entire data. It provides federated indexing and search capabilities across all nodes and is identified by an unique name

### Node
- A node is a single server which is a part of cluster, stores data and participates in the cluster's indexing and search capabilities

### Index
- Similar to a table in a relational database which stores documents having a particular schema in JSON format
### Documents
- Basiclly records in an index just like a row in a relational database.
- Each document has a JSON format, a unique _id associated to it and pertains to a specific mapping/schema in the index
### Fields
- Basically attributes of a document in an index similar to columns in a table of a relational database
### Mapping
- Used to spcify the scheme for an index. It defines the fields within an index, the datatype of each filed,a nd how the filed should be handled by Elasticsearch
### Shards
- Elasticsearch provides the ability to subdivide the index into multiple pieces called shards.
- Each shared is in itself a fully-functional, fully managaged and independent "index" that can be hosted on any node within the cluster.
- Useful for the case when an index putted in a single node would take more diskspace than available, the index then is subdivided between diferernt nodes
- As a user of Elasticsearch, we only need to specify the number of primary and replica shards for an index and should never deal with shards individually but only at the index level
- ES distributes shards amongs all nodes in the cluster and can move shards automatically from one node to another in the case of a node failure
### Replicas
- It is a copy of an existing primary shard
- Main reasons behind keeping replicas
	- A replica shard can be promoted to a primary shard if the primary fails
	- Get and search request can be handled by primary or replica shars, so having replicas can improve performance
### Alias
- Used for specifying an altername for an existing index or set of indices
- Specifically usefull when we want to fetch documents form multiple indices => Instead of run search queries on multiple indices, we can mention all the indices in comma separated form and give a common alias to all the indices and just use that
