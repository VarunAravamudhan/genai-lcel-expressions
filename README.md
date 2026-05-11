## Design and Implementation of LangChain Expression Language (LCEL) Expressions

### AIM:
To design and implement a LangChain Expression Language (LCEL) expression that utilizes at least two prompt parameters and three key components (prompt, model, and output parser), and to evaluate its functionality by analyzing relevant examples of its application in real-world scenarios.

### PROBLEM STATEMENT:

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
### OUTPUT:

<img width="1251" height="440" alt="image" src="https://github.com/user-attachments/assets/4f862030-2cba-4104-8970-720219ab4aed" />


### RESULT:
Thus the Program has been verified and executed successfully.
