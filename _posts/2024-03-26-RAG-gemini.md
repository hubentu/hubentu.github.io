---
layout: post
title: RAG with langchain and free google gemini
date: 2024-03-26
description: Unleashing the Power of RAG with Langchain and Google's Free Gemini
tags: LLM, RAG
categories: NLP
related_posts: false
---

## Introduction
Combined with Langchain and Google's free Gemini API, this blog post
explores how to harness these technologies together to create a
powerful document retrieval and summarization system. *Autogen* is
another solution but doesn't work with free google GenrativeAI yet.


Briefly understand the core technologies involved:

* Langchain: An open-source library designed to facilitate the
  building of chains of language models for various applications,
  including question answering and document retrieval.

* Gemini: Google Generative AI and Google Generative AI
  Embeddings. Part of Google's suite of AI tools. It is free for 60
  requests per minute.

* Chroma: An essential component that allows for efficient storage and
  retrieval of vector embeddings, making it possible to perform
  similarity searches across large document sets.

```python
from langchain_google_genai import GoogleGenerativeAI, GoogleGenerativeAIEmbeddings
from langchain_community.document_loaders import UnstructuredFileLoader
from langchain_community.document_loaders import DirectoryLoader
from langchain.prompts import PromptTemplate
from langchain.chains.question_answering import load_qa_chain
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.vectorstores import Chroma

api_key = "YOUR googleai studio KEY"
embeddings = GoogleGenerativeAIEmbeddings(model="models/embedding-001", google_api_key=api_key)
model = GoogleGenerativeAI(model="gemini-pro", temperature=0.1, google_api_key=api_key)

## index documents                                                                                                                                                                                                
loader = DirectoryLoader("/home/Documents/")                                                                                                                                                           
docs = loader.load()                                                                                                                                                                                             
text_splitter = RecursiveCharacterTextSplitter(chunk_size=10000, chunk_overlap=200)                                                                                                                              
texts = text_splitter.split_documents(docs)                                                                                                                                                                      
db = Chroma.from_documents(documents=texts, embedding=embeddings,                                                                                                                                       
                           persist_directory="./chroma_db")                                                                                                                                             

## load db
db = Chroma(persist_directory="./chroma_db", embedding_function=embeddings)

qa = "summarize [the topic] in papers"
keys = qa
retrieved_docs = db.similarity_search(keys, k=10)

prompt_template = """                                                                                                                                                                                             
Answer the question as detailed as possible from the provided context,                                                                                                                                            
make sure to provide all the details, if the answer is not in                                                                                                                                                     
provided context just say, "answer is not available in the context",                                                                                                                                              
don't provide the wrong answer\n\n                                                                                                                                                                                
Context:\n {context}?\n                                                                                                                                                                                           
Question: \n{question}\n                                                                                                                                                                                          
Answer:                                                                                                                                                                                                           
"""

# Create Prompt
prompt = PromptTemplate(template=prompt_template, input_variables=['context', 'question'])

# Load QA Chain
chain = load_qa_chain(model, chain_type="stuff", prompt=prompt)

# Get Response
response = chain({"input_documents": retrieved_docs, "question": qa}, return_only_outputs=True)
print(response['output_text'])
```

