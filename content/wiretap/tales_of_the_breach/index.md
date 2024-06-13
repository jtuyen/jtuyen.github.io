+++
title = "Tales of the Breach with Field CISO"
date = 2024-06-12
[taxonomies]
tags = ["threat actors", "red team", "dark web"]
+++

[Tales of the Breach With Field CISO Jason Haddix](https://streamyard.com/watch/W46eD36gq2a9)

Thoughts and takeaways:
* Almost 40% of breaches are started by leaked credentials found offered on the dark web. 18% of breaches came from phishing and 18% came from exploit of vulnerabilities.
* Most modern threat actors don't bother with sophisticated attacks that can trigger EDR/AV. What they do most is searching through applications that has web interfaces which mean hiding within HTTP(S) traffic as long as possible. This is why I tend to preach the idea of building payloads around reverse proxying a network connection to understand the lay of the land. In my experience, it's easier to sneak by on the network level rather than splintering off unusual processes.
* The presenter categorized levels of sophisication and order of operations based on the threat actor. It ranges from skiddies searching or buying fresh creds, adversaries performing AitM, N/0-day exploits, and the hardest to detect of them all is insider threats.
* Threat actors are looking through various management software and knowledge bases to understand what is available to them and see if there are hardcoded credentials that can be further leveraged. Items that are being targeted: knowledge bases, dev and project management, SCM, code repositories, build servers, dev platforms, config managerment, operations and monitoring, IaaC platforms, and secrets managers. Most of these has some form of web interface or knowledge has been dumped cleartext into a readable knowledge base for everyone to read. Believe me, it happens more than you think.
* [deepdarkCTI](https://github.com/fastfire/deepdarkCTI) - Collection of Cyber Threat Intelligence sources from the Deep and Dark Web. Kudos to all the CTI spies that keep this fresh as possible, it's not easy at all.
* Airgap your DCJs, nuff said..