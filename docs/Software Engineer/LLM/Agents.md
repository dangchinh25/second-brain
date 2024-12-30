- Great video: https://www.youtube.com/watch?v=uVkS05qPhik
- Blog post by OpenAI researcher: https://lilianweng.github.io/posts/2023-06-23-agent/
 

- ReAct articles: https://research.google/blog/react-synergizing-reasoning-and-acting-in-language-models/
- React videos: https://www.youtube.com/watch?v=Eug2clsLtFs
- ReAct Paper:
![[ReactPaper.pdf]]

## Overview
- In a LLM-powered autonomous agent system, LLM functions as the agent's brain, complemented by several key components:
	- **Planning**
		- *Subgoal and decomposition*: The agent breaks down large tasks into smaller, manageable subgoals, enabling efficient handling of complex tasks.
		- *Reflection and refinement*: The agent can do self-criticism and self-reflection over past actions, learn from mistakes and refine them for future steps, thereby improving the quality of final results.
	- **Memory**
		- *Short-term memory*
		- *Long-term memory*: This provides the agent with the capability to retain and recall (infinite) information over extended periods, often by leveraging an external vector store and fast retrieval.
	- **Tool use**
		- The agent learns to call external APIs for extra information that is missing from the model weights (often hard to change after pre-training), including current information, code execution capability, access to proprietary information sources and more
![](https://i.imgur.com/5AxJpYr.png)

### Planning
- A complicated tasks usually involves many steps, an agent needs to know what they are and plan ahead
- A few popular *prompting technique* is often used to help the LLM generate the desired execution plan and also improve its quality
	- *Chain of Thoughts* ([example](https://api.python.langchain.com/en/latest/agents/langchain.agents.react.agent.create_react_agent.html))
	- *Tree of Thoughts*
- Another crucial part beside planning is *Self-reflection/Observation*
	- A vital part to allows agent to improve iteratively by refining past action decisions and help making new decisions.
	- *ReAct* is one of the most popular technique
- Really good read to understand what goes behind the scene of Langchain agent: https://medium.com/@terrycho/how-langchain-agent-works-internally-trace-by-using-langsmith-df23766e7fb4
- Basically, the prompt will guide the behaviour of the Agent (*ReAct* style)
	- We start with just the prompt and the question
	- The agent will 
		1. *Thought*: think of what to do next
		2. *Action*: Perform the action using the *Action input*
		3. *Observation*: Perform observation based on the output of the action
		4. At each step, store the input &output (into a temporary *agent_scratchpad*, which holds data for 1 session of agent execution), after *Observation* append everything to the original prompt and repeat from 1 until it feels confident enough finish

### Memory
![](https://i.imgur.com/rGhcdz0.png)
- **Sensory Memory:** This component of memory captures immediate sensory inputs, like what we see, hear, or feel. In the context of prompt engineering and AI models, a prompt serves as a transient input, similar to a momentary touch or sensation. It's the initial stimulus that triggers the model's processing.
    
- **Short-Term Memory:** Short-term memory holds information temporarily, typically related to the ongoing task or conversation. In prompt engineering, this equates to retaining the recent chat history. This memory enables the agent to maintain context and coherence throughout the interaction, ensuring that responses align with the current dialogue.
    
- **Long-Term Memory:** Long-term memory stores both factual knowledge and procedural instructions. In AI models, this is represented by the data used for training and fine-tuning. Additionally, long-term memory supports the operation of RAG frameworks, allowing agents to access and integrate learned information into their responses. It's like the comprehensive knowledge repository that agents draw upon to generate informed and relevant outputs.
	- Can also be used a *tools* where LLM decides to use tool to retrieve data or not