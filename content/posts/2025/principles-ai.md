---
title: "Principles of AI Use"
date: 2025-09-14T15:16:00.000Z
author: Ryan Robinson
description: "'AI' might be useful in some contexts, if the result is validatable and you understand what you are modelling."
tags:
    - AI
---

Let's set aside a lot of the other ethical questions about "AI" - by which I mean large language models - for a minute. I never really recommend setting aside ethical questions, but I do want to write about something else right now. Are these "AIs" useful? What for? What are they counterproductive for?

As I've thought about this, I have so far settled on two main principles for the types of tasks that I think are a suitable fit for large language models, commonly referred to as "AI":

## Validatable

**Guideline number one: only use it for tasks where there is a human with adequate knowledge able to validate whether it actually accomplished the desired goal**. Otherwise, it might be what you wanted 90% of the time, but you have no way of knowing what the other 10% is. An analogy I saw on Mastodon was that of a boat: a boat that gets you 90% of the way can be useful, but only if you're a strong enough swimmer to finish the other 10%. Otherwise, now you're just stuck in the middle of the water with no way to finish your travel. The analogy if you aren't even able to identify which is the 10% it got wrong is a little rougher, because it would have to be something like not even realizing that you're drowning.

So here's some uses which do qualify:

- Generate code in response to a well-defined prompt by a programmer who can then review the code and plug it in to their dev environment to see if it does what they wanted it to do, running it against manual and automated tests. That can be pretty useful for that stage of generating a first draft of code.
- Write a first draft of a document from some bullet points you provide, and then you review the results before doing anything else with it.
- Have it analyze your own writing or speaking for you. I've heard podcasters mention the idea of feeding in a bunch of their work and asking it to help them identify any ways they could improve. Sometimes LLMs are referred to as "spicy autocorrect" and sure, that's a derogatory phrase but it does name one thing that it is pretty good at.
- Conducting a faster natural language search that can better understand your question and combine several sources together for you, with links back to those sources so you can verify anything that isn't quite right. One of the common examples is planning a vacation, and it might help with a big picture first overview that helps you see how all the pieces can fit together, but you're still going to need to validate all those pieces, like whether that proposed attraction is even open right now. This was the aspect that I started seeing value in a lot faster with the earlier public models, not in the results but in the ability to understand my question even when I don't know the exact jargon keywords.

There are also some things which definitely do not qualify:

- Using it in place of a therapist. You might be able to quickly validate that it made you feel a little less lonely, but that's not the same thing as being made more mentally well. This has been in the news a lot and I am confident in saying that it might make you feel better short term simply because you feel safe expressing yourself (you probably shouldn't, as a lot of the models will use your input to train the model, but I know if you're in crisis you aren't thinking about that). But it has no expertise, no humanity, and no awareness of who you are or your context other than a statical calculation of what words should come next in a response to your query.
- Summarizing somebody else's writing that you've never read before. You have no way of knowing what it got right and what it got wrong without at least skimming the whole thing yourself. I recently read about a Canadian cabinet minister proudly talking about how he fed a proposed bill into a service that turned it into a podcast he could listen to. That should be terrifying. He has no idea what's actually in the bill. It's his job to make major legislative decisions and lead within his portfolio, and he can't even be bothered to read the proposed legislation? At least have a human expert staffer summarize it for you.
- Having it completely write and send messages or documents for you without your careful review.
- Any use in education where the purpose of the work is to learn and develop the skills. Even when an LLM gives you the "right answers" it has completed defeated the real purpose in the same way that copying off your friend does. It's like having a machine lift weights for you at the gym; sure the weights got moved but that wasn't the actual goal. The actual goal was to develop your muscles and the weights were just a tool to help you get there.

Finally on this point, I'll add a bit more nuance to the guideline: sometimes it is validatable, but the validation is going to take longer than if it you done it yourself. Some research has shown that yes, it can save a lot of time in the brainstorming and drafting stage, but then adds a lot more time requirement to the editing stage, such that it is about the same amount of time in total. So the question is not simply whether it is validatable, but whether it is validatable in less time than it would have taken to do it all yourself.

## Know the Model

**Guideline number two: Don't use a model that is designed for something different than you are trying to get out of it**. The big one currently: don't use a language model for things other than modelling language. It's not a fact engine. It's not a math engine. It's a language model.

If you want to model language, that makes sense.

If you want it to answer a factual question, that's the wrong tool for the job. If you want it to be your friend or therapist, that's the wrong tool for the job. If you want it to solve math problems or tell you how many r's there are in the word strawberry, that's the wrong tool for the job. There might be other models that are built for those kinds of problems, or it might be that machine learning techniques are never the right tool for that kind of job, but you should not assume a language modelling tool to be great at anything other than modelling language.

The use of the term "AI" is a big part of what is misleading here, in my opinion, as most people assume that means it can do everything at a superhuman level. It can't. Certain models are built to do certain things, and the majority of what we are talking about as general "AI" are language models. Unfortunately those who are not highly technically literate hear "AI" and are imagining superintelligent machines, either benevolent or evil, from science fiction. The corporations trying to sell you on relying on it more and more are not in any rush to correct this misconception, and some of them may even genuinely believe they're only a few months away from achieving the sci-fi version.

## The Ethical Questions

Ok, I know I said we could set those aside for this post, but I'll just quickly name a few questions you should consider:

- Is it a shortcut to learning something that increases your skills? Or are you offloading cognitive processes because it saves some time in the short-term? If the latter, are you comfortable losing those skills? What will you do when the [enshittification](https://en.wikipedia.org/wiki/Enshittification) inevitably occurs, with raised prices and/or harmful bias introduced?
- Do you know if it is training on the data you input to the query? Are you comfortable with the possibility that your query might become an answer to somebody else? Most of the business-class subscriptions will guarantee that it won't train on your queries, but the personal and free ones usually will be training on it so you really need to be careful what you put in.
- Are you comfortable with the environmental consequences with the demands for more water and power? As I understand it, this isn't nearly the biggest part of our climate crisis, but it's not nothing, so you still need to decide if it is worth it.
- How will it impact content creators over time? Rhetoric about it being a "plagiarism machine" is overblown, but there is still a power differential with these extremely rich companies getting even more rich based on content written by others who do not get the same benefits. How will it change journalism on the internet, if people don't feel the need to ever open the actual article and see ads or prompts for a paywall? And will we get so much content generated by language models that it drowns out the better content prepared carefully by humans?

There is a lot to think about. While it will be interesting to see how things evolve, we also need to remember that we get some small vote in how it evolves. Nothing is inevitable unless we choose societally to make it inevitable; any claim otherwise is pure marketing hype.
