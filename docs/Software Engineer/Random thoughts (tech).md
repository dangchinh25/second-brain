## New hot tech
### tRPC
- tRPC is pretty nice but it doesn’t provide any mechanism for sharing types between calls, mostly relies on type inference of typescript
	- The adaptor to work with express is not a way to replace traditional REST api implementation, but more about a way to have both RPC calls and REST Api call
	- Haven’t really found a good reason to use it yet, aside from saving time for personal project (backend & frontend in the same repo), but in real work, probably not?

	> TRPC only works if client and server are on the same repo, so that they can share the `AppRouter` type definition, which enables the whole type safety thing, there's no magic here, just literally sharing type object between client and server. If BE and FE are on different repos, it doesn't work at all => Understandable why it's good for solo dev as it saves quite a lot of time 
	
	- This is only specific to tRPC, not gRPC, which is totally different

### Zod
- Zod is a library similar to Joe, but more lightweight, but Joi is much more battle tested and provide more flexible validation rule and support more built in functionality

### Drizzle
- Drizzle just kinda feel a better version of Sequelize that supports typescript and sql query generate, so kinda in middle of Sequelize and Prisma in terms of developer experience
- The generated types does not show as a type name but the actual content of the type itself, which is not ideal in some cases

## AI Hype
The market right now is hype on OpenAI-wrapper tech so there has been a rising in the number of these types of company.
- The idea for the company/tools does not have to be really complex, nor does the implementation.
	- E.G: 
		- ATS on steroid: instead of just ATS doing all the filtering and checking, we can leverage AI to parse and scan through all of the resumes and let it chooses the best (say 10) resumes and pass it to the hiring manager. They only need to look through 10 of them and choose the one that they like and then AI will handle sending out the email as well as scheduling with the candidate.
		- Support Ticket on steroid: All support ticket will be saved as well as the back and forth communication. New submitted ticket will be handled by AI as it can get access to the old data, find similar issue and answer them or route them to the Support Engineer if needed?.
		- A really good real world example is Retool AI
			- Retool already provides something called Retool workflow, which has a bunch of integration with multiple things and one trigger will enable the whole pipeline.
			- We can AI to any steps in these workflow, maybe we want to send an email to a bunch of people given the current step with the current data. However, we only need some specific format of the data, not a whole, we can use AI and write instruction for it on how to process the data in the way that we want. Everything is highly customizable and automated.
			- Retool Vector ? => To read more after understand embedding
				- The idea is that we try to make the AI understand more context whenever we try to do anything. For example, if you just say What is this and how to use this? AI will just return a very generic answer. But if we provide more context for it, it can use that context and give a much better answer. However, most user don’t know how to provide context in the right way, or maybe the context is a whole bunch of documentation, Retool Vector let the user declare a set of predefined context, and whenever they want to do anything, they can just ask a very generic question and choose the right context, Retool Vector will automatically inject the predefined context into the actual call to Open AI Api. 
				- The cool thing here is that we can append more things to the Vector list, so we can potentially build the whole pipeline around asking and adding to vector list and continue, creating a feedback loop.

- All of this is made available by the ability to understand context and extract/classify data by AI.
	- OpenAI embedding: Basically transform input data to a vector, which makes it easier to work with for AI ? => To read more

- Tools to build AI application easily:
	- Langchain
	- Marvin AI
	- ETC

- [x] How to let Open AI access the database safely?
	- There’s no way to do it directly
	- We have to manually build a middle layer between Open Ai and our database
		- For example we can feed Open AI with all of our database schemas and let it figure out the relationship.
		- Technically we don’t need to let Open AI to read our data but mostly just understand the relationship and generate the query
- [ ] How to use Dall E and what is it good for ?
- [ ] How to generate UI design using AI (DallE vs Midjourney etc?)
- [ ] 
---


- Idea:
	- With OpenAi embedding’s ability to figure out the semantic meaning of any given text, it’s really a game changer. One immediate application is that we can build a QnA platform around anything.
	- Side Project Idea: 
		- We can replicate the Retool Vectors feature, where we let the user input a list of text/url as context, and then when they want to ask something, or instruct to do something they can choose the context, we can create embedding from the chosen context and send it along with the question.
			- This can expand further where we can let user input anything (PDF for example)
			- User can have multiple context for different purpose.
			- When the user input into the context list, we can built an underlying crawler that go through the website, grab all the text data and do it recursively for all the child website.
			- We can also save the generated embedding into a  Vector database for future use instead of regenerating every time.
			- If the user does not choose to use any context, it will just be a regular plain api without any embedding.
		- AI-powered Support Ticket Helper
			- Save support ticket step to resolution and create embedding out of it
			- When the next support ticket comes in, we just need to find similar one and construct into steps for the Human in the Loop (HITL)
		- Database Relationship View
			- Insert Database url/Database Credentials
			- Auto generate DB Diagram for It
			- Feed Open AI with Database schema and let is reformat/extract data into the Entities Schema suitable to render on the UI
			- Maybe save the database url so that we can re-use in the future
			- Do we need to save the generated schema or are we going to recreate every time?
				- Maybe save and reuse
				- Have some integration to detect if there is anything change in the schema and recreate it at that time only => Save money on Open AI API call 
		- Integrate our own AI into Raycast, and later on it will also show up on the Context Ask GPT history => Recast Plugin