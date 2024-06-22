+++
title = "Dependency Confusion"
date = 2024-06-22
[taxonomies]
tags = ["web application", "appsec", "supply chain"]
+++

[How I Hacked Into Google's Internal Corporate Assets](https://observationsinsecurity.com/2024/04/25/how-i-hacked-into-googles-internal-corporate-assets)

> I focused on open-source and supply chain attacks. Over the period of a week I discovered multiple vulnerabilities, and gained control of (read: “command injection on”) numerous in-scope bug bounty assets.

> Six of these assets were Google’s internal corporate assets, many more belonged to a self-driving car company’s build CI/CD pipeline and others I can’t share the details on. These findings were mostly critical with high impact.

> Many of the applications and software we use every day as consumers (ie. banking apps, government services, social media, etc) heavily depend on these open source libraries, often referred to as “dependencies”.

> significant issue arises when an external attacker identifies and registers a public package that an organization is exclusively using internally.

> programming languages like Node.js and Python are programmed to automatically retrieve a public version of the package if no local version is available – or if the public version is higher.

That covers what dependency confusion vulnerability is about. It seems easy but it's somewhat trivial depending on how the CI/CD is configured of which the author later mentions without much details. I'm guessing code execution is one thing but getting the data outside is another problem?

> It is likely that many large organizations are vulnerable to dependency confusion in some way and I still believe this to be true even after my exploits – in fact, more-so now. Although I don’t have evidence, I’ve had enough anecdotal conversations to get the impression that not many tech companies are taking an organized and risk based approach to uplifting the security of their CI/CD pipelines. This is not something that security attestations like SOC2 or ISO27001 cover but it is a critical area of risk for software development.

> As an aside, I went to a security conference recently, where a security vendor was selling a tool to solve this exact problem. You don’t need to buy a tool to solve this problem and it reinforced to me that this issue is not well understood by security teams – and that is the second fundamental reason why I figured this particular endeavor worthy of some time and effort. 

This part is a neat insight from a industry professional and it's how I approach things as well. Figuring out what is not heavily covered in standards and go after those weak security controls.

> This is significant because the names of these internal dependencies can align to names of publicly available names; names that are contained in functions in externally accessible code or even the names of externally accessible apis or services. Extending on this hypothesis, developers might use names that are associated with core products or features. Because of this, a degree of discovering these in public javascript and/or brute forcing them is possible

This is where bulk of the recon scanning for possible dependency names come into play. Scraping live and archived data to figure out what the possible filename convention could be and try the attack out.

> Organizations impacted the most were those running continuous delivery and deployment. It didn’t impact software that had a slower development cycle, this is because with continuous delivery and deployment, artifacts are called regularly and automatically and so risk of compromise within a given timeframe is much more likely. 

> This also impacts developers. I’d often have developers installing my packages on their local development machines due to a general lack of awareness or vigilance around open source risks. 

I'm going to give this a try in my next engagement, it does stir up some ideas of what can be possible.