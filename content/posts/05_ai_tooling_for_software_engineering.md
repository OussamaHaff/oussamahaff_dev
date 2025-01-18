+++
title = "AI Tooling for Software Engineering - What ICs Could Learn from Farmers!"
date = "2025-01-17"  
author = "Oussama Hafferssas"
cover = "img/05_ai_tooling_for_swe/ai-tooling-in-swe.webp"
description = "Observations, analogy and conclusion about the evolution of software engineering industry in the Era of AI tooling."
tags = ["Opinion", "AI", "Software Engineering"]
+++

# Long Intro

Between excitement and fear, my feelings about the heavy adoption of GenAI tooling in our field as a hands-on software 
engineer were swinging constantly for the past year, which is the case for many of my friends and colleagues.

In many cases, the fear of something new is fueled or augmented by ignorance, so I decided mid-year 2024 to step out of 
my comfort zone being a Staff Engineer in a platform team focusing on Android -_[Avoid the frontend bias](https://leaddev.com/technical-direction/uncover-the-invisible-ceiling)_- 
to invest in learning the new ecosystem of GenAI, benefiting from my academic background in CS 
where I have used both [Discriminative and Generative Models](https://www.datacamp.com/blog/generative-vs-discriminative-models) 
in computer vision (implementing those algorithms by hand using Java!) 
to build a hackathon project to have a code review assistant using LLMs, Graph RAG and LangChain,
and also on a personal level fine-tuning and even to building my own LLM!

Based on this high speed upskilling train 🚅, on observations I made from our industry and also based on my personal 
experience being in a Platform team for the past 3 years, I came to the following observations, analogy and conclusion.


# Observations on IC Tasks

|          Classic Software Engineering           |                       Evolved Software Engineering                       |                                                     Accelerators                                                      |                                               Blockers                                               |
|:-----------------------------------------------:|:------------------------------------------------------------------------:|:---------------------------------------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------------------------------:|
|          Ask Google or Stack Overflow           |                  Prompt a commercial or an in-house LLM                  |                       - Less overhead to find the best result (even considering hallucinations)                       |                     - Cost of having an internal LLM trained on the right data.                      |
| Write solution design or documentation manually |    Prompt a commercial or an in-house LLM **+** Light manual writing     |                                           - Avoid the white paper syndrome                                            |                     - Cost of having an internal LLM trained on the right data.                      |
|            Manual navigation in IDE             |                    Prompt the IDE in natural language                    | - High cognitive load caused by codebases getting very large (Android and iOS) or too sparse (Backend micro-services) |          - Edge AI (On-Device Models) still needs lots of memory to produce better results.          |
|           Ask a colleague about code            |                    Prompt the IDE in natural language                    |                                  - Instant response helps protect the flow of focus.                                  |                                                  //                                                  |
|                 Coding manually                 |   Hugely assisted by an LLM within IDE **+** Minimum manual adjustment   |         - Stay in the flow and avoid cognitive load resulted from navigating outside to look for information.         | - Cost of having an internal LLM that has the right context to produce good quality PRODUCTION code. |
|              Code review manually               | Hugely assisted by an Agent (LLM as a judge) **+** Lighter manual review |                                - Instant feedback **+** Holistic view if the codebase.                                |         - Cost of having an internal LLM that has the right context to good quality reviews.         |
|             Debugging code manually             |           - Hugely assisted by an LLM  **+** Manual debugging            |       - Automated [rubberducking](https://en.wikipedia.org/wiki/Rubber_duck_debugging) + Accelerated debugging.       |             - Non-homogenous developer environment and run environment needed to debug.              |

