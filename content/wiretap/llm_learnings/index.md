+++
title = "Building LLMs: Insights"
date = 2024-06-07
[taxonomies]
tags = ["LLM", "AI", "Engineering"]
+++

[What We Learned from a Year of Building with LLMs (Part I)](https://www.oreilly.com/radar/what-we-learned-from-a-year-of-building-with-llms-part-i)

I still find LLMs mysterious in some ways and at times it doesn't steer towards the answers I'm expecting. This article does help demystify my assumptions of how things work, a structure that can be followed, and goes a bit further without going deep into the weeds.

> We recommend starting with prompting when developing new applications. It’s easy to both underestimate and overestimate its importance. It’s underestimated because the right prompting techniques, when used correctly, can get us very far. It’s overestimated because even prompt-based applications require significant engineering around the prompt to work well.

Prompt engineering does take time but it does pay off especially when scaled across a community as seen in the frameworks like [Fabric](https://github.com/danielmiessler/fabric).

> Focus on getting the most out of fundamental prompting techniques. A few prompting techniques have consistently helped improve performance across various models and tasks: n-shot prompts + in-context learning, chain-of-thought, and providing relevant resources.

> Structured input and output help models better understand the input as well as return output that can reliably integrate with downstream systems.

> Have small prompts that do one thing, and only one thing, well.

The chain-of-thought technique can be referred as a step-by-step learning process to enable complex reasoning abilities. I find it helpful in situations where you get stuck with a generative piece of code of which does not make sense. It is easier to reason your way through a final result rather than relying on a zero shot prompt. You'll see this quite often if you start diving into malware development.

> The quality of your RAG’s output is dependent on the quality of retrieved documents, which in turn can be considered along a few factors.

> The first and most obvious metric is relevance. This is typically quantified via ranking metrics such as Mean Reciprocal Rank (MRR) or Normalized Discounted Cumulative Gain (NDCG).

> Like traditional recommendation systems, the rank of retrieved items will have a significant impact on how the LLM performs on downstream tasks.

> Second, we also want to consider information density. If two documents are equally relevant, we should prefer one that’s more concise and has lesser extraneous details.

> Finally, consider the level of detail provided in the document. The additional detail could help the LLM better understand the semantics of the table and thus generate more correct SQL.

RAG can be precise in output based on the context given. However, RAG does struggles with specific keyword-based search such as names, acronyms, ID and etc. Implementation of vector databases attempt to solve this problem but..

> We’ve been communicating this to our customers and partners for months now. Nearest Neighbor Search with naive embeddings yields very noisy results and you’re likely better off starting with a keyword-based approach.

It's not perfect nor it may not be the right solution to begin with. A hybrid search technique may be the better approach to improve the relevance of search results by fusing the traditional keyword-based search technique and modern way of ranking of results into vector embeddings. As a result:

> In most cases, a hybrid will work best: keyword matching for the obvious matches, and embeddings for synonyms, hypernyms, and spelling errors, as well as multimodality (e.g., images and text).

What about instances where you want to add new knowledge to an LLM? Is it better to retrain a model or keep RAG data as relevant as possible?

> Beyond improved performance, RAG comes with several practical advantages too. First, compared to continuous pretraining or fine-tuning, it’s easier—and cheaper!—to keep retrieval indices up-to-date. Second, if our retrieval indices have problematic documents that contain toxic or biased content, we can easily drop or modify the offending documents.

I heard that RAG is going out of style because as LLM context windows grows in size, there is no need for RAG anymore.

> While it’s true that long contexts will be a game-changer for use cases such as analyzing multiple documents or chatting with PDFs, the rumors of RAG’s demise are greatly exaggerated. First, even with a context window of 10M tokens, we’d still need a way to select information to feed into the model. Second, beyond the narrow needle-in-a-haystack eval, we’ve yet to see convincing data that models can effectively reason over such a large context. Thus, without good retrieval (and ranking), we risk overwhelming the model with distractors, or may even fill the context window with completely irrelevant information.

> Finally, there’s cost. The Transformer’s inference cost scales quadratically (or linearly in both space and time) with context length. Just because there exists a model that could read your organization’s entire Google Drive contents before answering each question doesn’t mean that’s a good idea.

This only goes to show LLM tech stack is still maturing and I think most developers are throwing whatever solution to a problem and see how it performs. As an example, working on agent workflows seems complicated to be fine tuning:

> Small tasks with clear objectives make for the best agent or flow prompts. It’s not required that every agent prompt requests structured output, but structured outputs help a lot to interface with whatever system is orchestrating the agent’s interactions with the environment.

<blockquote>
Some things to try:

* An explicit planning step, as tightly specified as possible. Consider having predefined plans to choose from (c.f. https://youtu.be/hGXhFa3gzBs?si=gNEGYzux6TuB1del).

* Rewriting the original user prompts into agent prompts. Be careful, this process is lossy!

* Agent behaviors as linear chains, DAGs, and State-Machines; different dependency and logic relationships can be more and less appropriate for different scales. Can you squeeze performance optimization out of different task architectures?

* Planning validations; your planning can include instructions on how to evaluate the responses from other agents to make sure the final assembly works well together.

* Prompt engineering with fixed upstream state—make sure your agent prompts are evaluated against a collection of variants of what may happen before.
</blockquote>

IDK, for a mere mortal that is tinkering with this stuff on and off during weekends, it's time consuming and everchanging. I'll stick with the agentless stuff for now and leave the over engineering stuff to the pros.