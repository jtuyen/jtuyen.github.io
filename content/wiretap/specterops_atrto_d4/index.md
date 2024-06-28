+++
title = "Adversary Tactics RTO - Day 4"
date = 2024-06-27
[taxonomies]
tags = ["red team", "specterops"]
+++

It's the last day and it was a humbling experience. Today we focused on Bloodhound operations and opsec considerations, tapping into DPAPI and understanding the extraction mechanisms for local/remote scenarios (underrated technique), and finally more dizzying details about kerberos attacks focusing on delegation methodologies. Here are couple of personal takeaways:

> BloodHound is not comprehensive and it has blind spots.

This has always been my take about Bloodhound. Bloodhound in the real world doesn't reflect the way you'll see in labs nor as simple to execute in mature environments that has conducted Bloodhound assessments in the past. As mentioned in the class, SharpHound should still be used to understand the lay of the land but keep data collection as minimal as possible by scoping the collection method. I like this take on it: perform a full collection on a sacrifical host/account that is unaffiliated with the main campaign.

> DPAPI: Master Key Extraction with the Domain Backup Key. If you have Domain Admin or equivalent privileges and you steal the DPAPI domain backup key, you can decrypt any domain user master key (and then associated DPAPI-protected data) forever! This key never changes, protect it like you would the krbtgt.

Two words: Holy shit 🤯. I find it neat that you can remotely retrieve a decrypted master key from a DC using MS-BKRP protocol.

> Defensive debrief: Sysmon and HELK provides logging visibility to endpoints

This might be a controversial take but red teamers should know how defensive mechanics work to be a better operator in future engagements. Sysmon and HELK stack would give a better understanding of weighing benefits and risks of operational security and situational awareness.