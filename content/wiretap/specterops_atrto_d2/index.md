+++
title = "Adversary Tactics RTO - Day 2"
date = 2024-06-25
[taxonomies]
tags = ["red team", "specterops"]
+++

Day 2 of training was focused on adversary detection workflows and methodologies, credential abuse, situational awareness around AD, payload mechanics, lateral movement, and exploiting SQL databases. There is a lot to cover and as mentioned yesterday, it's like drinking from a firehose of information and trying to catch up during lab time. Couple of quick takeaways that I thought was insightful:

> Everything is stealthy when no one is looking.

Comically accurate. On the flipside, imagine if you had blue teams monitoring your test environment when you have an active engagement, it's more stressful and harder to accomplish your objectives. Ask me how that went 😭

> Double hop problem when using kerberos authentication

When you execute code with WMI/WinRM, you'll receive a token with a network logon type, meaning the credentials are not actually sent to the host. This means that you can't "double-hop" and execute any additional network connections from this compromised host. The workaround is steal/create a token, pass the hash as the user, spawn a new process as the user, or use an existing ticket.

> https://github.com/0xthirteen/SharpRDP

I never heard of this method before and I found it quite interesting to have part of the toolset. It's a non-graphical mechanism for authenticated remote command execution via RDP. No more slow and noisy SOCKS pivoting just to execute a command with RDP.