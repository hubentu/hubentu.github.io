---
layout: post
title: Question-Answering Engine with CrewAI and SearXNG
author: Qiang; ChatGPT
date: 2024-03-28
description: Building a Cutting-edge Question-Answering Engine with CrewAI and SearXNG
tags: LLM, RAG, Search
categories: NLP
related_posts: false
---

In the rapidly evolving field of AI and information retrieval, the
quest for a more intelligent, efficient, and customizable
question-answering system is never-ending. The integration of CrewAI,
a platform for creating AI agents with specific roles and
capabilities, with SearXNG, a privacy-respecting meta search engine,
opens new avenues for developing such systems. This blog post delves
into how to leverage these technologies to create a customized
question-answer engine capable of searching topics from a meta search
engine and summarizing the findings into a cohesive article.

## Setting the Stage with SearXNG

SearXNG stands as a pivotal component in this setup, serving as the
meta search engine. It is free. The initial step involves launching
SearXNG using Docker, ensuring it's configured to output results in
JSON format for easy parsing and integration. This step is crucial for
enabling the seamless flow of information between the search engine
and the AI agents responsible for processing the search queries and
generating summaries.

```bash
$ docker run --rm \
             -d -p 8080:8080 \
             -v "${PWD}/searxng:/etc/searxng" \
             -e "BASE_URL=http://localhost:$PORT/" \
             -e "INSTANCE_NAME=my-instance" \
             searxng/searxng
```

Add `json` output format to `searxng/settings.yml`.


## Customizing the AI with CrewAI

CrewAI introduces a revolutionary approach to building AI systems,
allowing for the creation of specialized agents with distinct roles,
goals, and capabilities. In this scenario, two types of agents are
crafted: the Senior Researcher and the Writer. The Senior Researcher
is designed to delve deep into the chosen topics, leveraging the
SearXNG search tool to uncover relevant information and groundbreaking
technologies. The Writer, on the other hand, focuses on narrating
compelling scientific reports or articles based on the information
gathered by the researcher.

The demo is modified from the [Getting
Started](https://docs.crewai.com/how-to/Creating-a-Crew-and-kick-it-off/)
example from `crewai`.

```python
import pprint
from langchain_community.utilities import SearxSearchWrapper
import os
from crewai import Agent
from langchain.agents import Tool
from langchain_google_genai import GoogleGenerativeAI
from crewai import Crew, Process
from crewai import Task


search = SearxSearchWrapper(searx_host="http://localhost:8080")
## res = search.results("What is the scRNASeq?", num_results = 10, engines=["google scholar", "arxiv"])                                                                                                           

search_tool = Tool(
    name="search ng",
    description="Search the internet(search_query: 'string')",
    func=search.run,
)

api_key = ""
model = GoogleGenerativeAI(model="gemini-pro", temperature=0.1, google_api_key=api_key)


# Creating a senior researcher agent with memory and verbose mode                                                                                                                                                  
researcher = Agent(
    role='Senior Researcher',
    goal='Uncover groundbreaking technologies in {topic}',
    verbose=True,
    memory=True,
    backstory=(
        "Driven by curiosity, you're at the forefront of "
        "innovation, eager to explore and share knowledge that could change "
        "the world."
    ),
    allow_delegation=True,
    llm=model,
    tools=[search_tool]
)

# Creating a writer agent with custom tools and delegation capability                                                                                                                                              
writer = Agent(
    role='Writer',
    goal='Narrate compelling scientific reports about {topic}',
    verbose=True,
    memory=True,
    backstory=(
        "With a flair for simplifying complex topics, you craft "
        "engaging narratives that captivate and educate, bringing new "
        "discoveries to light in an accessible manner."
    ),
    tools=[search_tool],
    allow_delegation=False,
    llm=model
)
# Research task                                                                                                                                                                                                    
research_task = Task(
    description=(
        "Identify the next big trend in {topic}."
        "Its research hot topics and potential applications"
        "Focus on identifying opportunities and challenges and the overall narrative."
        "Your final report should clearly articulate the key points"
    ),
    expected_output='A comprehensive 3 paragraphs long report on the {topic}.',
    tools=[search_tool],
    agent=researcher,
    llm=model
)

# Writing task with language model configuration                                                                                                                                                                   
write_task = Task(
    description=(
        "Compose an insightful article on {topic}."
        "Focus on the latest trends and how it's impacting the area."
        "This article should be easy to understand, engaging, and positive."
        "Add link references if cited."
    ),
    expected_output='A 4 paragraph article on {topic} advancements formatted as markdown.',
    tools=[search_tool],
    agent=writer,
    async_execution=False,
    llm=model,
    output_file='new-blog-post.md'
)

# Forming the tech-focused crew with enhanced configurations                                                                                                                                                      
crew = Crew(
  agents=[researcher, writer],
  tasks=[research_task, write_task],
  process=Process.sequential
)

# Starting the task execution process with enhanced feedback                                                                                                                                                      
result = crew.kickoff(inputs={'topic': 'scRNASeq in Cancer reseach'})
print(result)

```


**scRNA-Seq: Revolutionizing Cancer Research with Novel Insights**

Single-cell RNA sequencing (scRNA-Seq) has emerged as a transformative
technology in cancer research, unveiling previously hidden cellular
complexities within tumors. It empowers scientists to study individual
cells, providing a comprehensive understanding of tumor
heterogeneity. This granular approach has led to groundbreaking
discoveries in cancer stem cell identification and characterization,
as well as the intricate interactions within the tumor
microenvironment.

**Unlocking Therapeutic Potential**

scRNA-Seq's ability to identify and characterize cancer stem cells
holds immense therapeutic potential. These cells, responsible for
tumor initiation and progression, can now be targeted with
precision. By understanding their unique gene expression profiles,
researchers can develop therapies that specifically eliminate cancer
stem cells, preventing tumor recurrence. Additionally, scRNA-Seq sheds
light on the tumor microenvironment, the ecosystem surrounding cancer
cells that influences tumor growth and metastasis. By dissecting the
complex interplay between immune cells, stromal cells, and blood
vessels, researchers can design immunotherapies that effectively
harness the body's own defenses against cancer.

**Challenges and Future Directions**

While scRNA-Seq offers unprecedented opportunities, challenges
remain. The high cost of experiments and the computational complexity
of data analysis hinder its widespread adoption. However, as
technology advances and costs decrease, scRNA-Seq will become more
accessible, democratizing its use in cancer research. The future holds
exciting prospects for scRNA-Seq, with potential applications in
personalized medicine, therapy selection, and the development of novel
cancer treatments. Its continued evolution promises to revolutionize
cancer research, leading to improved patient outcomes and a deeper
understanding of this complex disease.
