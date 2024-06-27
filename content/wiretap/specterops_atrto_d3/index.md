+++
title = "Adversary Tactics RTO - Day 3"
date = 2024-06-26
[taxonomies]
tags = ["red team", "specterops"]
+++

Day 3 of training was focused on operational security tradecraft, lateral movement between forest trusts, all things kerberos, and certificate abuse paths. Today was a meaty session and a lot of knowledge drops of how beacons work, things to avoid and how to blend into the network. Building a high-level cheatsheet and mindmap would be beneficial for future reminders during engagements. Couple of quick takeaways for today:

> Enumerate, Evaluate, Evade | Observe, Orient, Decide, Act

Simple to follow but hard to execute. Understanding risks and rewards during operation comes with experience, not everyone sees the same way.

> RC4 will be deprecated in the future Windows releases (2025?)

In the near future, it will be harder to conduct Kerberos attacks that involve RC4 encryption. Which brings to the next point:

> Kerberos and ticket forgery strategies

Without a strong grasp of understanding how the authentication mechanics work, it's hard to intuitively understand what the opsec implications are when navigating the recon, privesc, and lateral movement phases. Without the foundational knowledge, your skill ceiling as a red team operator is capped. Throughout the class, this book by [James Forshaw](https://a.co/d/0dXUT5sh) has been mentioned several times as a must read masterpiece on this specific topic if you want to get better navigating around as an operator.