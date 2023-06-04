## Prompting Principles

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

## LangChain

- LangChain helps parse/transform model’s output from string to JSON
- LLM is stateless; previous conversations are just saved in the memory as context. LangChain helps summarize previous conversations to reduce the memory usage
- LangChain's Router Chain
    - User provides a different prompt for each different problem, resulting in a "router".
    - User also provides a prompt to ask the LLM to choose the best prompt among the aforementioned prompts
    - When a question is asked, the LLM first chooses the best prompt, and then the question and the best prompt are used to invoke LLM again. All in one function call.
