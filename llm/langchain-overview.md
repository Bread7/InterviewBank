# LLM Defense

## Introduction

Large Language Models (LLMs) are  on the rise today due to how accurate and precise they can generate and output based on user's input. Furthermore, it could help with auto completion of words or assistance in coding and many more capabilities. Security now becomes a concern due to exposure of new attack vectors and exploits within the LLM model system.

## Communication with LLMs

The most common way to interact with the model is to use the web interface of LLM (e.g. ChatGPT), followed by using the API endpoints of the models and lastly, using [LangChain](https://python.langchain.com/docs/get_started/introduction) library to combine with the API to have more control with query and response.

```python
# By Avikumar in The Modern Scientist
# insert an openai key below parameter
import os
from langchain.chat_models import ChatOpenAI
from langchain.chains.question_answering import load_qa_chain
os.environ["OPENAI_API_KEY"] = "YOUR-OPENAI-KEY"

# load the LLM model
model_name = "gpt-3.5-turbo"
llm = ChatOpenAI(model_name=model_name)


# Using q&a chain to get the answer for our query
chain = load_qa_chain(llm, chain_type="stuff",verbose=True)

# write your query and perform similarity search to generate an answer
query = "What are the emotional benefits of owning a pet?"
matching_docs = db.similarity_search(query)
answer =  chain.run(input_documents=matching_docs, question=query)
print(answer)

-----------------------------------[Results]---------------------------------
'Owning a pet can provide numerous emotional benefits. Pets offer
 companionship and can help reduce feelings of loneliness and isolation. 
They provide unconditional love and support, which can boost mood and overall
 well-being. Interacting with pets, such as petting or playing with them, 
has been shown to decrease levels of stress hormones and increase the
 release of oxytocin, a hormone associated with bonding and relaxation. 
Pets also offer a sense of purpose and responsibility, as taking care of 
them can give a sense of fulfillment and provide a distraction from daily 
stressors. Additionally, the bond between pets and their owners can provide
 a sense of stability and consistency during times of personal or societal 
stress.'
```

## Importance of LangChain

LLM is a new ecosystem just like the rapid rise of blockchains and smart contracts for ethereum (and based networks) have developed. LangChain has became a popular tooling for developer to test prompt chaining, logging, callbacks and memory connections to different data sources. One of the most important feature is the ability to integrate with different LLM providers.


### Agents in LangChain

Used by LLMs to recreate programming entity to execute given tasks which may be a specified sequence of actions to take. Especially useful to allow more development options and creativeness dependent on the support the LLM provider offers. They are used as reasoning engine to aid in automation of question answering, querying tabular data, summarization and evaluation. 

Agents can also be further enhanced and integrated with additional tools to perform web scraping, retrieval of information, data processing, applying additional machine learning algorithms, integration and customization of proprietary systems.


### Memory in LLM

Ways to retain context for further query prompts by the user is to pass previous context messages or use vector database (embed text to numerical values). LangChain offers memory module to ease context passing during development and supports different LLMs to ease chat history management.

ChatGPT rolled out memory on 13 Feb 2024, which helps in managing context passing of chats or specific prompts to combine as context for the users' next few queries. Benefit of using memory is removing the need to pass full chat history as context, but rather specific chat messages to retain enough context and ultimately, reduce [token](tokens.md) usage.

## Interview Questions

* What are the various methods to communicate with LLMs?
* What are the benefits of using LangChain over LLM provider APIs?
* Explain how to manage context passing when querying to LLMs?

## Author

- [Zheng Jie](https://github.com/Bread7) 🍞

## References

1. [Logankil - LangChain for development](https://logankilpatrick.medium.com/what-is-langchain-and-why-should-i-care-as-a-developer-b2d952c42b28)
2. [OpenAI - Memory in ChatGPT](https://openai.com/blog/memory-and-new-controls-for-chatgpt)
3. [LangChain - API docs](https://api.python.langchain.com/en/latest/langchain_api_reference.html)
4. [Comet - Implementing LangChain Agents](https://www.comet.com/site/blog/implementing-agents-in-langchain/)

## Future Todos

* [LangChain Plugins](https://www.danorlandoblog.com/reverse-engineering-chatgpt-plugins-with-langchain/)