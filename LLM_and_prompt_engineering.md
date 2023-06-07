## Two Types of LLM

https://www.deeplearning.ai/short-courses/building-systems-with-chatgpt/

- Base LLM
    - Predicts next word, based on text training data
- Instruction Tuned LLM
    - Tries to generate a sequence after/following an instruction
    - Is fine-tuned on the Base LLM, by doing the following:
        - Each Input is an instruction
        - Obtain human-ratings of the quality of different LLM outputs, on criteria such as whether it is helpful, honest and harmless
        - Tune the Base LLM to increase probability that it generates the more highly rated outputs (using Reinforcement Learning from Human Feedback)

## Prompting Principles

https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/

- Principle 1: Write clear and specific instructions
    - Tactic 1: Use delimiters to clearly indicate distinct parts of the input (Delimiters can be anything like: ```, """, < >, <tag> </tag>, :)
    - Tactic 2: Ask for a structured output (e.g. JSON, HTML)
    - Tactic 3: Ask the model to check whether conditions are satisfied (e.g. in the paragraph, if there are steps mentioned, return me a list of steps; otherwise, tell me that there are no clear steps)
    - Tactic 4: "Few-shot" prompting (e.g. give the model a few examples)
- Principle 2: Give the model time to “think”
    - Tactic 1: Specify the steps required to complete a task
    - Tactic 2: Instruct the model to work out its own solution before rushing to a conclusion

How to reduce made-up statements
- First ask the model to find relevant information/quotes
- Then ask the model to answer the question based on the relevant information/quotes

## Techniques and LangChain

https://www.deeplearning.ai/short-courses/langchain-for-llm-application-development/

- LLM is stateless; previous conversations are just saved in the memory as context. LangChain use LLM to summarize previous conversations to reduce the memory usage
- Regarding LangChain's Router Chain
    1. User provides a different prompt for each different problem, resulting in a "router".
    2. User also provides a prompt to ask the LLM to choose the best prompt among the aforementioned prompts
    3. When a question is asked, the LLM first chooses the best prompt, and then the question and the best prompt are used to invoke LLM again. All in one function call.
- LangChain's Indexes functionality answer questions based on documents (whose size is too large to directly pass to LLM) by
    1. Splitting the documents into chunks
    2. Creating an embedding vector for each document by calling API (many APIs to choose from)
    3. Storing documents and embedding vectors in a vectorstore
    4. Finding documents most related to the question by comparing the embedding vectors (many retrieval ways to choose from)
    5. Passing the question and the relevant documents to LLM to get the final answer
- Can ask LLM to determine to which category a user's question belongs, and then pass only documents of that category and the original user question to LLM again. The benefit is (1) More focused (2) Deal with documents being too long / too many (3) Reduce cost (because of paying per token).
- Human QA ground truth labels do not exactly match LLM's outputs, but can still be matching. LangChain's QAEvalChain functionality use LLM itself to evaluate if (and to what degree) LLM's outputs match human QA ground truth labels.
- Can pass LLM's answer and the context to the LLM again, and ask it to determine if the answer is sufficiently good, before displaying to the user. But this technique is used less commonly in practice due to latency and cost.
- LangChain's Agents functionality allows connecting LLM to calculators and search engines.
- Can ask LLM's answer to contain answers to many small steps, each separated by delimiters, or in a specific format, so the answer can be easily parsed, and only display the last paragraph to the user.
