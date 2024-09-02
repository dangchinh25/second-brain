
https://www.notion.so/blog/data-model-behind-notion
- How Notion handle changes
	1. Client generate new block & static data
	2. Build operations 
		- Gist: https://gist.github.com/dangchinh25/6e4613e2a56c91139fcb15efab084e05
		- Schema: 
	```json
	{
		pointer: ,
		path: ,
		command: "",
		args: {} // JSON to describe the new state
	}
	```
	3. Group all operations into 1 transaction
	4. Push transaction to a TransactionQueue
	5. TransactionQueue will be stored in IndexedDB/SQLLite
	6. Transaction is processed in Notion's server and remove from TransactionQueue


# Notebook project
## Overview
- B2B SaaS project
- The general idea is create a tool that can connect to customer's data and generate a report based on specific customer's requirement (customer design the flow to generate the report) 
- A Jupiter Notebook-like interface where user can define blocks, with each blocks should serve a specific purpose
- Can also think of it is a workflow builder where each block is a step in the workflow
- Should support multiple type of block
	- Code block
	- AI block
	- Resource query block
	- etc
- Should support different kind of trigger to generate the report
	- Schedule
	- Webhook
	- Manual
- Should integrate with various datasources so client can connect to their own data 
- Should build a really good template system (good for demo and getting client purposes)
	- Templates for app
	- Templates for block?
- References:
	- Retool (Duh?)
	- Tines.com
	- Relay.app
	- Hex.tech
	- Zapier

- [UPDATED 08/27/2024] 
	- We could also add native integration to tools like Slack/Discord that supports custom command or sth similar

## Specific
- Instead of building a native integration of every tools/datasources, we should instead on focus on building a really good REST + OpenAPI integration and template system
- The reason with this is that it is pain to build a maintain different integration to dozens of integration and most integration supports REST API anyways
- Example: Tines.com
- We can just let user use the REST integration and add template for popular integrations
- An interesting idea would be to somehow open source the template for popular resources, so when user want to add new integration, they can do it themselves and make a PR? or import it directly into the app as a 'private templates'?
