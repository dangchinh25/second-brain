
- Returning structured output: https://js.langchain.com/v0.2/docs/how_to/structured_output/#the-.withstructuredoutput-method
	- Normally, we use Function Calling to return function params that can be used to call the actual function
	- Not all provider will use Function Calling, but it may have something equivalent => LangChain already abstracted all of this away with `withStructuredOutput`

- **Runnable**
	- From Claude: https://claude.ai/chat/040e8355-1b06-4903-8ebb-4044476145a5
	- A core interface in Langchain. It defines a contract for objects that can process input and produce output. Any class implementing this interface must provide methods like `invoke()` or `ainvoke()`
	- Fundamental building blocks in LangChain. They're used to create modular, composable pieces of functionality. Can be easily combined and chained together to create more complex workflows.
	- Chain Runnable: https://js.langchain.com/v0.2/docs/how_to/sequence/
	- Represent various components:
		- Language models
		- Prompts
		- Chains (sequences of operations)
		- Tools or external API calls
	- Most core components in LangChain is a *Runnable*
```typescript
// Simple Runnable for text generation
import { ChatOpenAI } from "langchain/chat_models/openai";

const model = new ChatOpenAI();
const result = await model.invoke("Write a haiku about programming");
console.log(result);

// Chaining operations
import { ChatOpenAI } from "langchain/chat_models/openai";

const model = new ChatOpenAI();
const result = await model.invoke("Write a haiku about programming");
console.log(result);
```

- **Chain**
	- A Chain is a specific type of *Runnable* that combines multiple steps or components into a single, cohesive operation. Chains implement the Runnable interface, allowing them to be used wherever a Runnable is expected
	- LangChain exposes a lot of predefined *Chain* for common task or pattern
	-  We can use `.pipe` to connect different runnable to create a custom chain
```typescript
import { ConversationChain } from "langchain/chains";
import { ChatOpenAI } from "langchain/chat_models/openai";
import { BufferMemory } from "langchain/memory";

const chain = new ConversationChain({
  llm: new ChatOpenAI(),
  memory: new BufferMemory()
});

const response1 = await chain.call({ input: "Hi, I'm John." });
console.log(response1.response);

const response2 = await chain.call({ input: "What's my name?" });
console.log(response2.response);
```
# LangGraph

- **StateGraph**
	- The core abstraction of LangGraph, used to create the graph workflows
	- The graph maintains a shared state between all the nodes. The state can be arbitrary and follow a custom user defined schema
	- We can define a reduce function for each state key to instruct how the result of each node will update the state of the graph
```typescript
interface IState {
	messages: BaseMessage[];
}

const graphState: StateGraphArgs<IState>["channels"] = {
	messages: {
		// In this case, the result state of each will be appended to the shared state
		value: (x: BaseMessage[], y: BaseMessage[]) => x.concat(y),
		default: () => [],
	},
};
```

- **Checkpoint** ^7c8e71
	- When we execute the graph, it will create a checkpoint after each internal execution step.
	- By default it will not create anything unless we bind it with a *Checkpointer*
	- Once you start [checkpointing](https://langchain-ai.github.io/langgraphjs/how-tos/time-travel/persistence.ipynb) your graphs, you can easily **get** or **update** the state of the agent at any point in time. This permits a few things:
		1. You can surface a state during an interrupt to a user to let them accept an action.
		2. You can **rewind** the graph to reproduce or avoid issues.
		3. You can **modify** the state to embed your agent into a larger system, or to let the user better control its actions.
	- By using `.updateState`, we can update the state of the graph with the given values, as if they came from node `as_node`
		- This can be understand as if we are simulating a node run ourselves instead of modifying a value of a specific state
	- We can write a custom *Checkpointer* by extending the abstract class *BaseCheckpointSaver*, where we have to implement 3 functions:
		- *getTuple*: Get the checkpoint tuple for the given configuration, used whenever we start invoking the graph, every internal graph traversal does not call this
		- *list*
		- *put*: Put the checkpoint for the given configuration. Every internal step will result in a new checkpoint to be inserted.
		- *serde*: This is not a function that we need to override, we can understand it as a parser that we can use to transform an object into a *Checkpoint* and vice versa. Not having this does not affect anything at all, we can just implement that logic else where and use when needed in the class and everything would be the same
		