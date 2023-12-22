- In some complex applications, Read and Write operations are not balanced.
	- On the read side, the application may perform many different queries, return different data object. Then, it may uses the data object to access a small part of data to perform some more queries.
- Read and write workloads are often asymmetrical, with very different performance and scale requirements.
	- There is often a mismatch between the read and write representations of the data, such as additional columns or properties that must be updated correctly even though they aren't required as part of an operation.
	- The traditional approach can have a negative effect on performance due to load on the data store and data access layer, and the complexity of queries required to retrieve information.
- The solution is **Command and Query Responsibility Segregation**
	- Separates reads and writes into different models, using *commands* to update data, and *queries* to read data
![](https://i.imgur.com/p1nRxQY.png)

# Usage with Event Sourcing
- For greater isolation and scaling ability, we can use this with [[Event Sourcing]] pattern.
![](https://i.imgur.com/ULQeate.png)
- Here, we take one step further and separate the *Read* data and logic to its own services and database.
	- The *Write* datastore can be in a format that is optimized to write and normalized with relational format
	- The *Read* datastore can be in a format that is easier to query (document-based) or much less normalized to simplify the query from the client.
	- The *Read* datastore can be view as a *read-replica* of the *Write* datastore, but with some modification optimized for query
- The *Write* and *Read* datastore much be kept in sync, which can be done by the *Write* model publish an event whenever it updates the database and the *Read* model can just process it (does not have to replicate the data in the exact format, can perform some additional logic if needed)

# Benefit
- **Independent scaling**: The *Read* and *Write* services can be scale independently
- **Optimized data schemas**: The *Read* side can use a schema that is optimized for queries, while the *Write* side uses a schema that is optimized for updates. ^8cc3f2
- **Separation of concern**: This pattern can result in models that are more maintainable and flexible. Most of the complex business logic goes into the *Write* model. The *Read* model can be relatively simple.
- **Simple queries**: This goes back to the point about [[#^8cc3f2 | Optimized data schemas]]

# Issues
- **Complexity**: The basic idea is simple, but actual implementation can be quite complex and add much more dev time and infrastructure costs.
- **Eventual consistency**: [[Consistency patterns#Eventual Consistency | Eventual Consistency]]