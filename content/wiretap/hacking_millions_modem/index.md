+++
title = "Hacking Millions of Modems"
date = 2024-06-05
[taxonomies]
tags = ["API", "web application", "obfuscation"]
+++

[Hacking Millions of Modems (and investigating who hacked my modem)](https://samcurry.net/hacking-millions-of-modems)

Potentially related to the other story that I mentioned about [China's ORBs network](https://johntuyen.com/wiretap/china-orbs-network/). Interesting how the author accidently stumbled upon a replay of his own HTTP network traffic and connected the two dots together at a much later time that it may have been used for a C&C traffic. It kind of got me thinking if I were to be in this same situation, would I have notice that my modem has been compromised? Hard to say because external traffic monitoring isn't common from a home perspective especially if you have FTTP (Fiber to the Premises). In addition, if your day-to-day internet usage isn't affected by visible/noisy MitM type behaviors (thanks to SSL/TLS) or sluggish network performance/reliability, most people wouldn't suspect a thing or fall back to power cycling the modem which doesn't completely fix the root issue. Depending on the ISP, you are at mercy at remote push updates as the ISP need to adhere a configuration standard. It is wise of owning your modem and delegate the routing process to a router with monitoring and alerting capabilities like pfSense. Modem/router combos tend to suck anyway.