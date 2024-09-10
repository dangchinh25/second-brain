- **GPT = Generative Pre-trained Transformer**


- **Embedding Layer**
	1. Text input is *tokenized* into tokens
	2. Perform a look up operations for each token onto a *Embedding matrix*
		1. The LLM model has a fixed set of vocabulary
		2. Each word in that vocabulary is initialized with a vector of random values to represent the semantic meaning of that word
		3. These random values will be updated to a correct value as a result of the training process and then it will be ready to use (the result of the look up operation is to return 1 of these vectors that is corresponding with 1 of the input token)
	3. After the look up process, each token now is associated with its *embedding vector*, all of these creates the *embedding* for the given text input

- The result vector of the embedding layer, since it just got plugged out of the *embedding matrix*, it only encodes the meaning of the that word/token only, without taking in any context information => Continue with the process in the **Transformer**