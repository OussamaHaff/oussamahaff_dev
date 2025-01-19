+++
title = "AI Tooling for Software Engineering - What ICs Could Learn from Farmers!"
date = "2025-01-19"  
author = "Oussama Hafferssas"
cover = "img/05_ai_tooling_for_swe/ai-tooling-in-swe.webp"
description = "Observations, analogy and conclusion about the evolution of software engineering industry in the Era of AI tooling."
tags = ["Opinion", "AI", "Machine Learning","Software Engineering"]
+++


[TOC levels=1-3]: #

# Table of Contents
- [Long Intro](#long-intro)
- [Observations on IC Tasks](#observations-on-ics-core-tasks)
- [ICs and Farmers - An Analogy](#ics-and-farmers---an-analogy)
    - [ICs VS Farmers - Before the Revolution](#ics-vs-farmers---before-the-revolution)
    - [ICs VS Farmers - After the Revolution](#ics-vs-farmers---after-the-revolution)
- [Conclusion](#conclusion)
  - [The Next Evolution](#the-next-evolution)
  - [Human Brains Are Here to Stay](#human-brains-are-here-to-stay)
- [Epilogue](#epilogue)

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

| Classic Software Engineering (Previous +40 years) |              Evolved Software Engineering (Next +40 years)               |                                                    Accelerators                                                     |                                              Blockers                                              |
|:-------------------------------------------------:|:------------------------------------------------------------------------:|:-------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------------------:|
|           Ask Google or Stack Overflow            |                  Prompt a commercial or an in-house LLM                  |                       Less overhead to find the best result (even considering hallucinations)                       |                     Cost of having an internal LLM trained on the right data.                      |
|  Write solution design or documentation manually  |    Prompt a commercial or an in-house LLM **+** Light manual writing     |                                           Avoid the white paper syndrome                                            |                     Cost of having an internal LLM trained on the right data.                      |
|             Manual navigation in IDE              |                    Prompt the IDE in natural language                    | High cognitive load caused by codebases getting very large (Android and iOS) or too sparse (Backend micro-services) |          Edge AI (On-Device Models) still needs lots of memory to produce better results.          |
|            Ask a colleague about code             |                    Prompt the IDE in natural language                    |                                  Instant response helps protect the flow of focus.                                  |                                                 //                                                 |
|                  Coding manually                  |   Hugely assisted by an LLM within IDE **+** Minimum manual adjustment   |         Stay in the flow and avoid cognitive load resulted from navigating outside to look for information.         | Cost of having an internal LLM that has the right context to produce good quality PRODUCTION code. |
|               Code review manually                | Hugely assisted by an Agent (LLM as a judge) **+** Lighter manual review |                                Instant feedback **+** Holistic view if the codebase.                                |         Cost of having an internal LLM that has the right context to good quality reviews.         |
|              Debugging code manually              |            Hugely assisted by an LLM  **+** Manual debugging             |       Automated [rubberducking](https://en.wikipedia.org/wiki/Rubber_duck_debugging) + Accelerated debugging.       |             Non-homogenous developer environment and run environment needed to debug.              |


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


# Conclusion

## The Next Evolution

{{< figure
src="/img/05_ai_tooling_for_swe/new-sofware-enngineering-timeline.webp"
position="center"
width=2080
height=1080
style="border-radius: 8px;" 
caption="Timeline of the potential evolution of Software Engineering"
captionPosition="center" >}}
                                     
I think that we are in the middle of a major and a blazing fast evolution (AKA a revolution) in the software industry
after-which we will see the following:

- **Larger codebases to handle** - Just as a single farmer with modern equipment can now manage much larger farms than 
their ancestors could, software engineers using LLMs can potentially handle larger codebases and complete tasks more quickly.

- **Dependency and loss of core knowledge** - In farming, many modern farmers have become overly dependent on technology 
and in many cases lost traditional farming knowledge. When systems fail, this knowledge gap becomes critical. 
Engineers might become too dependent on LLMs for problem-solving, losing the ability to think through problems 
independently or understand fundamental principles. 

- **Rocket speed onboarding** - Learnings new tech stacks, making PoCs to explore new concepts is just way easier 
for ICs now (and also for people from outside the field) removing a big blocker in T-shaping or even pushing 
ICs to [comb-shaping](https://certibanks.com/KnowledgeArea.aspx?articleid=11).

- **Raise of the generalists** - Farmers where expected to know how to harvest some species of plants and specialise based
on their local weather, land and seeds availability, but now they are expected to be able to switch between plants
using standard seeds and tooling and also handle bigger lands. The same applies on software engineers as a result of 
the rocket speed onboarding with new tech stacks, software engineers will be expected not the stay in their speciality
and to be more of generalist.

- **Project teams will be squeezed** - As a result of raise the of the potential 
productivity gain when these tools get better, 1 or 2 engineers will be able to do a team job which will impact 
the overall size of teams across industry.

- **Titles will be flattened** - As a result of having less engineers in teams and the fact that it will be
even harder to draw a line between current IC levels junior/mid/senior/staff/principals, unless companies are willing to 
invest in the exercise of evaluation the output of their engineers bases on intuition, IMO companies will just move to
a flattened structure with fewer titles. Probably as simple as 2 levels, **_Software Engineer_** and
**_Lead Software Engineer_**.

- **Different adoption speeds** - IMO, the adoption of this evolved software engineer practice in the industry will depend 
on which **tier** (A compensation-focused classification published by
_Gergely Oroscz_ [here](https://newsletter.pragmaticengineer.com/p/trimodal-nature-of-tech-compensation)) 
a company is situated in:


  - Tier 3 companies would be the fastest to adopt and build their own ML tooling for ICs. 
  - Tier 1 companies could be the second and adopt more 0-code ML tools built by startups. 
  - Tier 2 companies could be the slowest to embrace this change.


## Human Brains Are Here to Stay

With all of this, one could be easily pushed to say that Software Engineers will be replaced, which is something 
I don't see coming yet based on our farmer analogy. 

Modern farmers were not 100% replaced after the industrial revolution, but their roles have evolved to be more efficient.
A side effect of that is that we have fewer farmers nowadays. I expect the same to happen for software engineers.

Software engineers who stay are the ones who manage to evolve, they wil find themselves writing lots of prompts,
reviewing tons of ML tools generated code, doing more holistic and complex system design and orchestrating the work
of AI agents.

Those new tasks depend on the understanding of outside world by reasoning and adapting to it. Which is what the human 
brain will always outpace what any ML tool/algorithm can do.


# Epilogue

In this blogpost, I didn't judge if modern farming is good, neither the evolved state of software engineering.
I didn't explore deeper drivers leading humanity to be constantly pushed to be more productive or more efficient
for profit maximisation and give opinion on them. 

Instead, I intended to make it a neutral cause-and-effect analysis. Thank you for taking the time and neurons to read it!
