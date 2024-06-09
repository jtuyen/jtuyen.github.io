+++
title = "Modern WAF Bypass Techniques"
date = 2024-06-08
[taxonomies]
tags = ["web application", "appsec"]
+++

[Modern WAF Bypass Techniques on Large Attack Surfaces](https://www.youtube.com/watch?v=0OMmWtU2Y_g)

Takeaway and thoughts:
* WAF Bypass Philosophy: simple bypasses are the best bypasses.
* Increase of request size limits can almost always universally bypass WAFs due to the universal design decision.
* The burp plugin [nowafpls](https://github.com/assetnote/nowafpls) help pads HTTP requests to bypass WAF. Honestly, you can probably get away with Match and Replace feature.
* [IP Rotate](https://portswigger.net/bappstore/2eb2b1cb1cf34cc79cda36f0f9019874) burp plugin rotates IP addresses from multiple regions using API gateways which help with bypassing rate limiting protection features. This is a must have if you want to use intruder against a target.
* [Fireprox](https://github.com/ustayready/fireprox) is similar to IP Rotate but a CLI version of which you can pipe in your other tools like `ffuf`.
* [Shadowclone](https://github.com/fyoorer/ShadowClone) this is a great tool and IMO the next gen of Axiom. No need to run heavy and slow VPS instances no more. Task distribution using serverless functions is more nimble, scalable, and cheaper to complete. With the free monthly compute usage that cloud providers hand out like candy, it's simple to test out theories against targets.
* Bypassing WAF via the WAF using shared certificates or WAF IP whitelisting. Great trick and the thought process is similar to bypassing conditional access rules.
* Bypassing on-premise or reverse proxy based WAFs via H2C smuggling is an underrated bypass method.