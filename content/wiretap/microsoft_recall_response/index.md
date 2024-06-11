+++
title = "Microsoft Recall Response to Criticism"
date = 2024-06-10
[taxonomies]
tags = ["microsoft", "privacy", "AI"]
+++

[Update on the Recall preview feature for Copilot+ PCs](https://blogs.windows.com/windowsexperience/2024/06/07/update-on-the-recall-preview-feature-for-copilot-pcs/)

Previous commentary:
* [Microsoft Recall: FML](https://johntuyen.com/wiretap/microsoft-recall/)
* [TotalRecall Recall is worst than I thought](https://johntuyen.com/wiretap/totalrecall-recall/)

> First, we are updating the set-up experience of Copilot+ PCs to give people a clearer choice to opt-in to saving snapshots using Recall. If you don’t proactively choose to turn it on, it will be off by default.

> Second, Windows Hello enrollment is required to enable Recall. In addition, proof of presence is also required to view your timeline and search in Recall.

> Recall snapshots will only be decrypted and accessible when the user authenticates. In addition, we encrypted the search index database.

> Copilot+ PCs will launch with “just in time” decryption protected by Windows Hello Enhanced Sign-in Security (ESS), so Recall snapshots will only be decrypted and accessible when the user authenticates. This gives an additional layer of protection to Recall data in addition to other default enabled Window Security features like SmartScreen and Defender which use advanced AI techniques to help prevent malware from accessing data like Recall.

Overall, decent response to address the critics about the security concerns. On the technical side of things, Windows Hello and ESS is a good choice and so far there aren't any known attack vectors but it's [not fully bulletproof](https://blackwinghq.com/blog/posts/a-touch-of-pwn-part-i/) due to hardware vendors not complying with the [SDCP](https://github.com/microsoft/SecureDeviceConnectionProtocol) standard. 

Lastly, with all this conundrum around privacy concerns, Microsoft should release a Windows version without all this Copilot AI bloatware. Although, I doubt this would happen considering the value it may bring to both parties. Similar to Windows 10/11, power users are going to be running mandatory bloatware removal scripts when starting fresh but even then there is no guarantee that it won't be enabled in the future updates.