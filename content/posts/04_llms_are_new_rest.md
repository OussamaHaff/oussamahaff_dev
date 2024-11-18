+++
title = "Interacting with LLMs is the new REST!"
date = "2024-11-18"  
author = "Oussama Hafferssas"
cover = "img/04_llms_are_new_rest/llms-are-new-rest.svg"
description = "I shared in this short blog post a personal opinion on acquiring knowedge around interacting with LLMs for software engineers."
tags = ["LLM", "REST", "Dev"]
+++



It has been a while since I have put my keyboard at the service of blog posts! 
A lot has changed in the software engineering industry since my last blog post, so I’ll keep it short & concise.

Setting up a Large Language Model locally on any machine is now incredibly accessible through tools like _[Ollama](https://github.com/ollama/ollama)_. 
Similarly, open-source LLMs, ranging from large to small, are readily available with commercial-free and 
permissive licenses. Everything can be easily containerised.

After experimenting with a local LLM for a few months on my laptop and realising the compound benefits 
of such a setup as a user and especially as a software engineer, I came to this conclusion:

For non-technical users, LLMs are basically the new web (even with hallucinations). 
However, for software engineers, it’s even more critical. **Programmatically interacting with LLMs 
should be comparable to being efficient in handling the basics of RESTful APIs**!

Most software engineers are expected to have at least a basic understanding of REST, 
including HTTP methods like `PUT`, `POST`, `GET`, structured URLs, and clear data formats for requests and responses 
to establish communication between two pieces of software.

In my opinion, the same principles apply between two pieces of software, one of which is a LLM. 
This includes spawning a LLM locally or on a cloud service, programmatically communicating with it, 
and prompting it using _[LangChain](https://github.com/langchain-ai/langchain)_ (or equivalent), 
to performing basic RAG (Retrieval Augmented Generation) tasks, up to fine-tuning. 
IMO these operations will become fundamental building blocks for the next generation of software.

I intentionally excluded the process of building a Large Language Model, handling autonomous AI agents, 
and MLLMs (Multimodal LLMs) from the scope of this personal view for now. 
However, this could change rapidly in the foreseeable future.


It’s easier than you think to start. Give it a try!

> [*Comment*](https://github.com/hfrsoussama/oussamahaff_dev/issues/new/choose)
> *using Github issues to avoid cross-site trackers.*
