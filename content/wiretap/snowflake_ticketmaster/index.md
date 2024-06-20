+++
title = "Snowflake Data Breach"
date = 2024-06-19
[taxonomies]
tags = ["snowflake", "breach", "infostealer", "malware"]
+++

[Hackers Details How They Allegedly Stole Ticketmaster Data From Snowflake](https://www.wired.com/story/epam-snowflake-ticketmaster-breach-shinyhunters)

> About 165 customer accounts were potentially affected in the recent hacking campaign targeting Snowflake’s customers, but only a few of these have been identified so far.

> Wired, however, has independently confirmed that it was a Snowflake account; the stolen data included bank account details for 30 million customers, including 6 million account numbers and balances, 28 million credit card numbers, and human resources information about staff, according to a post published by the hackers. Lending Tree and Advance Auto Parts have also said they might be victims as well.

Quite significant considering what has announced publicly so far. I hope others have data salted and hashed at the very minimum.

> The hacker who spoke with WIRED says that a computer belonging to one of EPAM’s employees in Ukraine was infected with info-stealer malware through a spear-phishing attack. It’s unclear if someone from ShinyHunters conducted this initial breach or just purchased access to the infected system from someone else who hacked the worker and installed the infostealer. The hacker says that once on the EPAM worker’s system, they installed a remote-access Trojan, giving them complete access to everything on the worker’s computer.

Hackers installed a RAT on a machine? Two scenarios that came to mind is this user's machine has local admin privileges enabled or the adversaries were clever enough to privesc without alerting AV/EDR. Both of which are a clear indication that device compliance isn't strongly enforced.

> Using this access, they say, they found unencrypted usernames and passwords that the worker used to access and manage EPAM customers’ Snowflake accounts

Infostealers are growing in popularity over the years and rapidly growing due to malware-as-a-service offerings. It is as simple as generate a payload and fire it off via email or host it on an endpoint. The main purpose of this type of malware is about data exfiltration of sensitive data as fast as they can before it's detected by AV/EDR. You only need to be right once and unsophisticated adversaries wouldn't care if the payload has been detected, they have the data they wanted and it's going to be sold in bulk on the dark web. Whether the compromised organization has the ability to identify and deal with such situations can be a crap shoot (lack of expertise) and that's what unsophisicated adveraries are banking on. If you don't take action on it, it's a matter of time someone will be purchasing bulk credentials and try it out.

> The hacker says the credentials were stored on the worker’s machine in a project management tool called Jira.

Of course, we all do 😅 /s

> Snowflake accounts didn’t require multifactor authentication (MFA) to access them.

And this is the downfall for Snowflake and it's customers. It should've been enforced right from the get go. Instead, I'm guessing Snowflake made it optional and companies who used their service did not perform a third-party risk assessment before going into production. IMO both parties are to blame here.

>  But in cases where Snowflake credentials weren’t stored on the worker’s system, the hacker claims they sifted through stockpiles of old credentials stolen in previous breaches by hackers using infostealer malware and found additional usernames and passwords for Snowflake accounts

> Mandiant said that about 80 percent of the victims it identified in the Snowflake campaign were compromised using credentials that had previously been stolen and exposed by infostealers.

> Mandiant suggested that multiple contractors were breached to gain access to Snowflake accounts, noting that contractors—often known as business process outsourcing (BPO) companies—are a potential gold mine for hackers

> noting that the majority of the credentials the hackers used in the Snowflake campaign came from repositories of data previously stolen by various infostealer campaigns, some of which dated as far back as 2020.

This won't be the last story we will hear about compromising contractors as a way to gain initial access to a company they are working for. From what I can recall, the last major story was [Target](https://krebsonsecurity.com/2014/02/target-hackers-broke-in-via-hvac-company/) being compromised via HVAC company. This is why segregation and defense in depth is important and not an after thought.