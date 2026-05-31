## Design and Implementation of LangChain Expression Language (LCEL) Expressions

### AIM:
To design and implement a LangChain Expression Language (LCEL) expression that utilizes at least two prompt parameters and three key components (prompt, model, and output parser), and to evaluate its functionality by analyzing relevant examples of its application in real-world scenarios.

### PROBLEM STATEMENT:
Design an LCEL pipeline using LangChain with at least two dynamic prompt parameters. Integrate prompt, model, and output parser components to form a complete expression. Evaluate its functionality through real-world query-response scenarios.

### DESIGN STEPS:

### STEP 1: Define the Prompt Template
The first step involves setting up a ChatPromptTemplate that includes at least two input parameters. This allows the chain to be dynamic and accept different variables (e.g., topic and language) during execution.

### STEP 2: Initialize the Model and Output Parser
You must select and initialize the core components of the chain:

Model: A Chat Model (like Claude or GPT) to process the prompt.

Output Parser: A component (like StrOutputParser) to convert the model's raw message output into a more usable format, such as a plain string.

### STEP 3: Compose the LCEL Chain
Using the pipe operator (|), you combine the components into a single expression. The syntax typically follows this pattern:

### PROGRAM:
#### Simple Chain
~~~

import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']# Pass both parameters as a dictionary
chain.invoke({"topic": "bears", "language": "Spanish"})
from langchain.prompts import ChatPromptTemplate
from langchain.chat_models import ChatOpenAI
from langchain.schema.output_parser import StrOutputParser
prompt = ChatPromptTemplate.from_template(
    "Give me a Para about {topic} in {language}"
)
model = ChatOpenAI()
output_parser = StrOutputParser()
chain = prompt | model | output_parser
# Pass both parameters as a dictionary
chain.invoke({"topic": "bears", "language": "Spanish"})

~~~
#### Complex Chain
~~~
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import DocArrayInMemorySearch
from langchain.prompts import ChatPromptTemplate
from langchain.schema.runnable import RunnableMap
from langchain.chat_models import ChatOpenAI
from langchain.schema.output_parser import StrOutputParser

model = ChatOpenAI()
output_parser = StrOutputParser()

vectorstore = DocArrayInMemorySearch.from_texts(
    [
        "The Eiffel Tower is famous for being a historic landmark in Paris, France, known for its iron structure and popularity among tourists.",
 
        "People visit beaches for relaxation, swimming, vacations, and enjoying natural scenery.",

        "Exercise improves physical fitness, health, energy levels, and mental well-being.",

        "Books are important because they provide knowledge, improve imagination, and support learning."
    ],
    embedding=OpenAIEmbeddings()
)

retriever = vectorstore.as_retriever()

print(retriever.get_relevant_documents("What is Python?"))
print(retriever.get_relevant_documents("What is Machine Learning?"))

template = """
Answer the question based only on the following context:

{context}

Question: {question}
"""

prompt = ChatPromptTemplate.from_template(template)

chain = RunnableMap({
    "context": lambda x: retriever.get_relevant_documents(x["question"]),
    "question": lambda x: x["question"]
}) | prompt | model | output_parser

result1 = chain.invoke({
    "question": "What is the Eiffel Tower famous for?"
})

print(result1)

result2 = chain.invoke({
    "question": "Why do people visit beaches"
})

print(result2)

inputs = RunnableMap({
    "context": lambda x: retriever.get_relevant_documents(x["question"]),
    "question": lambda x: x["question"]
})

print(inputs.invoke({
    "question": "What are the benefits of exercise?"
}))
~~~
### OUTPUT:
#### Simple Chain

<img width="1251" height="440" alt="image" src="https://github.com/user-attachments/assets/4f862030-2cba-4104-8970-720219ab4aed" />

#### Complex Chain
<img width="1571" height="298" alt="image" src="https://github.com/user-attachments/assets/ef68859b-cf84-431b-9f49-d6baff770ba6" />



### RESULT:
The implemented LCEL expression takes at least two prompt parameters, processes them using a model, and formats the output with a parser, demonstrating its effectiveness through real-world examples.
