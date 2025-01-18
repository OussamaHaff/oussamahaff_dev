+++
title = "AI Tooling for Software Engineering - What ICs Could Learn from Farmers!"
date = "2025-01-17"  
author = "Oussama Hafferssas"
cover = "img/05_ai_tooling_for_swe/ai-tooling-in-swe.webp"
description = "Observations, analogy and conclusion about the evolution of software engineering industry in the Era of AI tooling."
tags = ["Opinion", "AI", "Software Engineering"]
+++


[TOC levels=1-3]: #

# Table of Contents
- [Long Intro](#long-intro)
- [Observations on IC Tasks](#observations-on-ics-core-tasks)
- [ICs and Farmers - An Analogy](#ics-and-farmers---an-analogy)
    - [ICs VS Farmers - Before the Revolution](#ics-vs-farmers---before-the-revolution)
    - [ICs VS Farmers - After the Revolution](#ics-vs-farmers---after-the-revolution)

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


# Observations on ICs Core Tasks

| Classic Software Engineering (Previous +40 years) |              Evolved Software Engineering (Next +40 years)               |                                                     Accelerators                                                      |                                               Blockers                                               |
|:-------------------------------------------------:|:------------------------------------------------------------------------:|:---------------------------------------------------------------------------------------------------------------------:|:----------------------------------------------------------------------------------------------------:|
|           Ask Google or Stack Overflow            |                  Prompt a commercial or an in-house LLM                  |                       - Less overhead to find the best result (even considering hallucinations)                       |                     - Cost of having an internal LLM trained on the right data.                      |
|  Write solution design or documentation manually  |    Prompt a commercial or an in-house LLM **+** Light manual writing     |                                           - Avoid the white paper syndrome                                            |                     - Cost of having an internal LLM trained on the right data.                      |
|             Manual navigation in IDE              |                    Prompt the IDE in natural language                    | - High cognitive load caused by codebases getting very large (Android and iOS) or too sparse (Backend micro-services) |          - Edge AI (On-Device Models) still needs lots of memory to produce better results.          |
|            Ask a colleague about code             |                    Prompt the IDE in natural language                    |                                  - Instant response helps protect the flow of focus.                                  |                                                  //                                                  |
|                  Coding manually                  |   Hugely assisted by an LLM within IDE **+** Minimum manual adjustment   |         - Stay in the flow and avoid cognitive load resulted from navigating outside to look for information.         | - Cost of having an internal LLM that has the right context to produce good quality PRODUCTION code. |
|               Code review manually                | Hugely assisted by an Agent (LLM as a judge) **+** Lighter manual review |                                - Instant feedback **+** Holistic view if the codebase.                                |         - Cost of having an internal LLM that has the right context to good quality reviews.         |
|              Debugging code manually              |           - Hugely assisted by an LLM  **+** Manual debugging            |       - Automated [rubberducking](https://en.wikipedia.org/wiki/Rubber_duck_debugging) + Accelerated debugging.       |             - Non-homogenous developer environment and run environment needed to debug.              |


# ICs and Farmers - An Analogy

From the previous table you can see that most of the IC tasks can be hugely assisted by ML tooling, which removes 
lots of the manual burden on engineers.

As an IC engineer who already uses this tooling in its **primitive** state currently, this led me to come to 
the conclusion that ML tooling for engineers, especially individual contributors, will have a huge impact on the role 
as a result of the change in is core tasks. IMO, what happened to modern farmers is the best example! 

Farmers initially did everything by hand (plowing, planting, harvesting) before the industrial revolution 
brought tractors and combine harvesters -sometimes even planes!- software engineers have traditionally performed 
most of the tasks manually, from writing every line of code manually (even to automate tasks), 
to debugging issues line by line, down to documenting everything by hand, before the LLM Revolution we are living currently.

Once this revolution settles down on clear winning models, and clear integrations in current IC workflows 
(Not only assisted coding, but all the tasks mentioned in the table before), I believe that the shape of the IC role
will be transformed to **_ML tooling operator_**, in the same way modern farmers found themselves 
as **_farming machines operators_**!

To explore more the aspects of this analogy, lets lay them down in 2 tables:


### ICs VS Farmers - Before the Revolution

|     Aspect     |                     Farmers                      |                                           Software ICs                                            | 
|:--------------:|:------------------------------------------------:|:-------------------------------------------------------------------------------------------------:|
|   Tools Used   |       Basic hand tools (hoe, plow, scythe)       |                        Known developer tools (text editors to modern IDEs)                        |
|  Work Method   | Manual plowing, planting, and harvesting by hand |                Writing code manually, line-by-line debugging, manual documentation                |
| Physical Limit |   Limited by human physical strength and time    |          Limited by typing speed, mental capacity and sometimes information availability          |
|  Core Skills   |    Deep understanding of soil, crops, seasons    | Deep understanding of algorithms, data structures, performance (Importance depends on the domain) |


### ICs VS Farmers - After the Revolution

|         Aspect         |                                        Farmers                                         |                                                        Software ICs                                                         | 
|:----------------------:|:--------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------------------------------------------------:|
|       Tools Used       |                   Tractors, combine harvesters, automated irrigation                   |                                              LLMs / ML tooling in all IC tasks                                              |
|    Scale of impact     |                     Can manage hundreds of acres instead of a few                      |                                Can maintain larger codebases and handle more complex systems                                |
| New knowledge required |               Machine operation, maintenance, modern farming techniques                |                            Prompt writing, LLM capabilities and evaluation, output verification                             |
|      Core Skills       | Understanding of soil, crops, seasons is needed to select the best machines, products. | Understanding of domain and software engineering practices is needed to select the best AI tooling and derivative products. |
|   Productivity Gain    |               One farmer can produce food for hundreds instead of dozens               |                                  One IC can handle projects that previously required teams                                  |
