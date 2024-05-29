+++
title = "SSID Confusion: EvilTwin Remix?"
date = 2024-05-29
[taxonomies]
tags = ["wireless", "wpa3", "eviltwin", "mitm"]
+++

# SSID Confusion: EvilTwin Remix?
[SSID Confusion - CVE-2023-52424](https://www.top10vpn.com/research/wifi-vulnerability-ssid/)

From a home perspective, no impact unless you are using WPA3. From an enterprise perspective, it's a new man-in-the-middle attack path against WPA3. Unlike EvilTwin which focuses on spoofing an SSID, SSID Confusion targets the validation mechanism of the SSID name during connection setup. More specifically, an adversary sets up a rogue AP on a different channel, advertising the target network's SSID. The attacker then performs a multi-channel man-in-the-middle attack to make the victim's device believe it has connected to the trusted network. What makes this attack particularly clever is its ability to deceive the target connected to a 5Ghz band into connecting back to the same SSID using 2.4GHz frequency band which lacks robust security features. Easy fix: don't reuse the password for both 2.4Ghz and 5Ghz SSID.