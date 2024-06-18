+++
title = "From Prompt Injection to Data Exfiltration"
date = 2024-06-17
[taxonomies]
tags = ["LLM", "prompt injection"]
+++

[GitHub Copilot Chat: From Prompt Injection to Data Exfiltration](https://embracethered.com/blog/posts/2024/github-copilot-chat-prompt-injection-data-exfiltration/)

> GitHub Copilot Chat VS Code Extension was vulnerable to data exfiltration via prompt injection when analyzing untrusted source code.

> This means that using carefully crafted instructions in a source code file, an attacker can cause the LLM to return hyperlinks to images which will then be automatically retrieved. This outbound image retrieval request can be used to exfiltrate data by having the LLM append additional information from the chat context as a query parameter.

> An attacker can access the previous conversation turns and append information from the chat history to an image URL. When Copilot renders the HTML and the image URL, the data is sent to the attacker.

I've been seeing this similar vulnerability across custom rolled plugins or internally built LLM client interfaces that support any models under the sun. Consider this attack vector when reviewing and testing LLM applications.