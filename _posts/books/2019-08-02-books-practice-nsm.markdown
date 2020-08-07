---
title: "The Practice of Network Security Monitoring"
date: 2019-08-02 16:16:37 -0400
categories: monitoring
tags: books
cover: /assets/images/books/practice-nsm/practice-nsm-logo.png
---

`Currently reading`{: .info }

[Amazon Link](https://amzn.to/2YB9Tqk)

![bookcover](/assets/images/books/practice-nsm/practice-nsm-bookcover.jpg){:width="200px"}

### 1: NSM Rationale

* Does NSM prevent intrusions?

	* Short answer is no because _prevention eventually fails_.

	* All organizations will suffer breaches in some form

	* The way you have to look at it is this, determined adversaries will inevitably breach your defences and your goal is buy enough time for them to gain persistence in target networks

	* If you buy enough time and you have proper defences setup to detect these intrusions, you will have time to respond and contain these attacks before hackers can finish the job

	* CIRT = Computer Incident Response Team

	* If you can detect it, why can't you prevent it? It's hard for organizations to keep up with sophisticated tactics and attacks. Time and knowledge is usually a factor

* The importance of time: case study

	![case-study](/assets/images/books/practice-nsm/practice-nsm-casestudy.png)

	* This timeline showcases an incident that happened in real case scenario. If the detection and containment took place within the first 4 weeks of the attack, database access and exfiltration would have been prevented

* What is the difference between NSM and Continous Monitoring?

	* NSM is _threat-centric_, meaning adversaries are the focus of the NSM operation

	* CM is _vulnerability-centric_, focusing on software vulnerabilities and misconfigurations

		* "Continuous" means checking system configurations every so often. "Monitoring" means determining whether systems are compliant with controls and checks how much it deviates from the standard

	* CM should complement with NSM, not a substitute or a variant of NSM

* How does NSM compare with other approaches?

	* NSM is not a blocking, filtering, or denying technology. NSM focuses on _visibility_, not control like firewall, IPS, AV, whitelisting, DLP or DRM technologies

	* Security team should be aware when security controls fail and that's where NSM fills in the gap. NSM is one way to show failed controls more visible

	![practice-nsm-blockmechanisms](/assets/images/books/practice-nsm/practice-nsm-blockmechanisms.png)

* Why does NSM work?

	* To understand this paradox, start from the perspective of the defender. When one sheepdog, guarding hundreds of sheep, faces a pack of wolves, at least some of the sheep will not live to see another day. Adversary wins.

	* From the intruder's perspective, assume the attacker is not looking into a hit-and-run scenario. Rather, they want to gain persistence in a network and remain undetected to gather information at will. The adversary is like a wolf hiding in a flock of sheep, hoping the sheepdog fails to find the wolf, day after day, week after week

	* NSM is a tool that provides this visibility to make the environment hostile to persistent adversaries

* How NSM is set up

	* You need to understand your network architecture and make decisions about where you most need visibility

	* Doesn't say much about where to place NSM yet. So far it mentions DMZ

* Installing a Tap

	* A tap is a dedicated hardware for accessing network traffic to extend visibility

	* [Net optics](http://netoptics.com)

* When NSM won't work

	* Most organizations do not have NSM's installed on wireless traffic because wireless traffic is encrypted. Devices connected to the wifi network won't be subjected to NSM. The NSM will only see the client network activity from jumping node-to-node, not network level

	* CIRTs generally do not conduct NSM on cellular traffic, it's outside of technical and legal mandate

	* CIRTs do observe smartphones connecting to WLAN networks

	* In cloud or hosted environments, NSM may be deployed and data is owned by the service provider. Same goes with ISPs

* Is NSM legal?

	* _No matter what, do not begin any NSM operation without obtaining qualified legal advice_

	* US law: "Wiretap act"

	* EU law: is more complicated as they have strict laws around NSM

* How can you protect user privacy during NSM operations?

	* Manage NSM operations to focus on the adversary and not on authorized user activity

	* Separate the work of CIRTs from forensic professionals:

		* CIRTs should perform analysis, watch malicious activity, and protect authorized users and organizations. Focusing on external threats

		* Forensics should perform investigations, monitor fraud, and abuse from authorized users. Focusing on internal threats

* A sample NSM test

	* [testmyids](http://testmyids.com)

	* Types of NSM data
	
		1. Full content

			* Exact copies of the traffic as seen on the wire

			* Lower OSI layers

			* Use `wireshark` to inspect data graphically

		2. Extracted content

			* Higher OSI layers

			* Review any transmission of data between any devices

			* Reconstruct data streams or web browsing session using [Xplico](http://xplico.org)

			* Useful to extract binaries from network streams for further analysis or reverse engineering

		3. Session data

			* A record of the conversation between two network nodes. Focusing on the call details of network activity

			* [Bro](http://bro.org)

			* [Argus](https://www.qosient.com/argus/)

			* [Sguil](https://bammv.github.io/sguil/index.html)

			* Session data contains source IP, source port, destination IP, destination port, protocol, app bytes sent by source and destination

		4. Transactional data

			* If full content data records every aspect of a phone call, and session data tells you only who spoke and for how long, then transaction data is a middle ground that gives you the gist of the conversation

		5. Statistical data

			* Describes the traffic resulting from various aspects of an activity

			* `capinfos` is a statistical tool that's bundled within `wireshark`

			* Stats can be used to view packet lengths and percentages to indicate malicious activities

		6. Metadata

			* Like statistical data, metadata extracts key elements from network activity and then leverage some external tool to understand it

			* Who owns the IP? Does their presence indicate a problem for us? Inspecting the domain, IP address, and routing data will answer these questions

			* [RobText](http://robtex.com)

			![practice-nsm-robtex](/assets/images/books/practice-nsm/practice-nsm-robtex.png)

		7. Alert data

			* Alerts are based on patterns of bytes, or counts of activity, or even more complicated options that look deeply into packets and streams on the wire

			* [Snorby](http://snorby.org) - web-based tool

			* [Sguil](https://bammv.github.io/sguil/index.html) - "thick client" that users install on their desktop

* What's the point of all this data?

	* The best way to use network-centric data to detect and respond to intrusions is to collect, analyze, and escalate as much evidence as your technical, legal, and political constraints allow

	* The most sophisticated intruders know how to evade IDS signatures and traditional analysis. With NSM data, you can have the best chance of foil these sorts of adversaries

* NSM drawbacks

	* Network encrypted data traffic denies access to analyze content

	* Network architecture such as NAT can obscure source and destination IP addresses

	* Highly mobile platforms may never use a segment that is monitored by NSM platform

	* Extreme traffic volumes may overwhelm NSM platform

	* Privacy concerns may limit access to what traffic should be monitored required for NSM to be effective

### 2: Collecting Traffic

* Example network:

	![practice-nsm-samplenetwork](/assets/images/books/practice-nsm/practice-nsm-samplenetwork.png)

	* Consist of 4 zones

	* The point of this diagram is figuring out where to place NSM sensors to start collecting network traffic

* Traffic flow in a simple network

	* First you need to understand network traffic flow as it will give you an idea where to place NSM sensors

	* Monitoring wired traffic is easier to observe rather than wireless traffic. Wireless traffic is more difficult because it's likely to be encrypted at a low level

	* DMZ network is a bit more complex because the source of the activity could be a computer on the local DMZ network or one on the internet

	* This is what the traffic flow looks like when DMZ network wants to connect to a DNS server on the internet. Pay attention on how traffic traverses

	![practice-nsm-dmzout](/assets/images/books/practice-nsm/practice-nsm-dmzout.png)

	* This is what the traffic flow looks like when a request from a user on the internet wants to connect to a webserver hosted on Vivian's Pets sample network.

	![practice-nsm-dmzin](/assets/images/books/practice-nsm/practice-nsm-dmzin.png)

* Possible locations for NSM

	* Several other reasons for traffic to flow into or out of Vivian's Pets network that may influence the placement of NSM sensors

		* Users on internal network might access resources in DMZ network

		* Users on wireless network might access resources in DMZ network

		* Systems in the DMZ network might access resources in the internal network

		* Systems in the wireless network might access resources in the internal network

	![practice-nsm-possiblelocations](/assets/images/books/practice-nsm/practice-nsm-possiblelocations.png)

	* C,D,E are good locations for NSM operations because it can monitor ingress and egress traffic. Or is it?? How do you decide which spot is best? IP addressing will determine that answer

* IP addresses and Network Address Translation

	![practice-nsm-ipassignment](/assets/images/books/practice-nsm/practice-nsm-ipassignment.png)

	* External router has 2 interfaces: 198.51.100.1 (external), 192.168.1.2 (internal)

	* Firewall has 4 interfaces: 192.168.1.3 (external router/internet), 172.16.0.4 (wireless), 192.168.2.5 (DMZ), 10.0.0.6 (internal)

* Address translation
	
	* Computers on the Internet can’t talk to the wireless, DMZ, or internal networks directly, some sort of device—a firewall or gateway router—is used to perform some form of translation to allow a company’s computers to talk to the Internet, and vice versa

	* Translation allows private IP addresses to "pretend" to be public addresses for the purpose of Internet connectivity

* Network Address Translation

	* Understanding translation is key to making NSM platform deployment decisions

	![practice-nsm-nat](/assets/images/books/practice-nsm/practice-nsm-nat.png)

	* When traffic flows through the firewall, the firewall rewrites, or translates, the IP address of the web server to a different value—in this case, 192.168.1.100. The firewall maintains a table that tells it that the address it created for web server 192.168.1.100 is the same as 192.168.2.100. Similarly, when traffic flows through the external gateway, the external gateway rewrites what it sees as the web server’s IP address (192.168.1.100) to 198.51.100.100

* Network Address Translation for Wireless and Internal Networks

	* While wireless and internal computers initiate traffic to the Internet, they should not accept traffic from the Internet unlike DMZ network

	* Translation method used is called Network Port Address Translation (referred to as NPAT or PAT)

		* Each translation device rewrites the wireless or internal source IP address to be a single IP value, and uses changing source ports to differentiate among sending computers

	* No need for multiple public IP addresses, only a single IP address is suffice

* Choosing the best place to obtain network visibility

	* Now armed with more knowledge about how network translation works, options C,D,E are not good choices to deploy NSM platform because it doesn't show the true source IP

	* Location C and D

		* All NPAT'd traffic has a source IP address of 192.168.1.3. NPAT obscures the true source IP address

	* Location E

		* All NPAT’d traffic has a source IP address of 198.51.100.1. NPAT obscures the true source IP address

	* Location for DMZ network

		* Because DMZ uses NAT, the diagram shows a one-to-one mapping between IP addresses which make it easy to determine

		* Some DMZ networks don't use one-to-one, handle these configurations accordingly

	* Location for viewing the wireless and internal network traffic

		* Because PNAT is used, there is no easy answer to where NSM platform should be placed

	* There is no single place where we can monitor the true source IP addresses from DMZ, wireless and internal networks. Instead, we use three locations to capture what we need to look for:

	![practice-nsm-gbh](/assets/images/books/practice-nsm/practice-nsm-gbh.png)

	* To see the true source IP addresses from the wireless network, deploy an NSM platform at G

	* To see the true source IP addresses from the internal network, deploy an NSM platform at B

	* To see the true source IP addresses from the DMZ network, deploy an NSM platform at H

	* If you want to monitor the true destination IP addresses, it's best to deploy at E, as close to the internet as possible

* Using switches for traffic monitoring

	![monitor-switch](/assets/images/books/practice-nsm/practice-nsm-monitorswitch.png)

	* These 3 switch interfaces are uplinks to the firewall that see all traffic passing through the switch to and from the firewall

		* Configure these switch ports to send copies of traffic to an unused switch port

		* Cisco = Switched Port Analyzer (SPAN)

		* Juniper and Dell = Port Mirroring

* Using a network tap

	* Rather than configuring switch ports, network administrators can deploy physical tap hardware at locations G, B, and H, with one tap at each location

	![network-tap](/assets/images/books/practice-nsm/practice-nsm-networktap.png)

* Capturing traffic directory on a client or server

	* Collecting data from a firewall or router is not a viable long term solution because the lack of onboard storage

	* Collecting NSM data on an endpoint such as a laptop or server makes sense. Laptop and workstations data collection might be limited for long time data though

* SPAN ports or taps?

	* The 3 tap solution seen above is the simplest because it avoids introducing single point of failure

	* 3 tap solution isn't the only solution available, vendors offer a wide variety of configurations

	* Prefer network taps over SPAN ports because of potential SPAN misconfiguration issues and difficulties with managing "oversubscribe" traffic to the mirror ports

* Typical NSM platforms have the following characteristics:

	* A large RAID array configuration

		* Buy the maximum amount of hard drive space you can afford. RAM comes second.

	* Minimum of 4GB of RAM, add 1GB more RAM to every interface connected to SPAN/TAP port

	* One CPU core per monitored interface

* Estimate full content data storage requirements

	* Hard drive storage for one day = Average network utilization in Mbps × 1 byte/8 bits × 60 seconds/minute × 60 minutes/hour × 24 hours/day

	* For example, say your network’s average utilization of a 1Gbps link is 100Mbps. Here’s how to use the formula:

	```
	100Mbps × 1 byte/8 bits × 60 seconds/minute × 60 minutes/hour × 24 hours/day = 1,080,000MB per day or 1.08TB per day
	```

	* 1.08TB per day is also 12.5MB per second, or 750MB per minute, or 45GB per hour

	* Next, decide how many days of traffic you want to store. If you want to store 30 days of full content data, at 1.08TB per day, you will need 32.4TB per 30 days

	* Other considerations you need to think about:

		* Estimate the hard drive space used by NSM databases. Database storage requirement is one-tenth that of the full content data storage needs. If we’re going to store, say, about 33TB of full content data, we should allocate another 3.3TB for database needs

		* Storing text file data will use about one-twentieth of the full content data numbers. That’s about 1.6TB of space

		* In summary, if we want to store 30 days’ worth of NSM data for a network averaging 100Mbps, it’s safe to allocate about 38TB of hard drive space.

* Ten NSM platform management recommendations

	* Limit shell access to the NSM system only to administrators. Everyone else uses the GUI to retrieve data

	* Don't share the root account (duh). Enable 2FA

	* Administrator sensor over secure communications like OpenSSH

	* Don't use the same system that manages normal IT or user assets. Separate account and machines to manage sensors

	* Equip sensors with remote-access cards

	* Limit exposure of services on teh sensor and patch frequently

	* Export logs from the sensor to a logging platform for monitoring

	* If possible, put sensor's management interface on private network reserved for management only

	* If possible, use full disk encryption to protect data on sensor

### 3: Standalone Deployment

* This CHfocuses on installing Security Onion (SO) in its simplest configuration: as a stand-alone platform

* Stand-alone or Server Plus sensors?

	* Stand-alone - Single-box solution that collects and presents data to analysts. Watch only a single segment, or several segments using a single sensor

	![standalone](/assets/images/books/practice-nsm/practice-nsm-standalone.png)

	* The dashed lines show network connectivity from the network taps at locations G, B, and H to the listening network interface cards (NICs) on the NSM platform. The solid line shows network connectivity from the internal network switch to the management NIC of the NSM platform.

	* Server plus sensors - Acts as a distributed platform, with sensors collecting data and a server aggregating and presenting data to analysts

	![serverplus](/assets/images/books/practice-nsm/practice-nsm-serverplus.png)

	* The sensors do not need to reside within the local network; they can be deployed globally as long as they can connect back to the central server via the network

	* Some organizations enable sensors with a VPN, while others deploy the management interfaces for each system (server and sensors) on public networks to make them universally reachable

* Installation of SO

	* Skipping this part because it's straight forward and outdated

	* Two NICs are needed for the installation

### 4: Distributed Deployment

* This CHis mostly about installing and configuring SO using Server Plus distributed model. Straight forward installation instructions so I'll be writing brief notes here

* Use the .iso images provided by the SO project to build an SO server, and then using the same .iso file to build an SO sensor.

* SO Server considerations

	* Lots of RAM to support MySQL database

	* SO sensors store .pcap files before sending to SO server, think about the required space on SO sensors as well

* SO Server installation is a straight forward process and outdated like CH3

	* All you have to remember is selecting sensor option and follow the wizard to connect sensor to SO server

* Verify that sensors are working

	```bash
	sudo service nsm (status/restart)
	```

* Using a standard .iso image from the Ubuntu Server distribution to replace the SO project .iso file. Use SO project Personal Package Archive (PPA) to build an SO server and an SO sensor

* Connecting to the SO server and configuring X forwarding. This means you will screen the X window session remotely as though it's a local machine

	```bash
	richard@ubuntu:˜$ ssh -X serverdemo@192.168.2.128
	The authenticity of host '192.168.2.128 (192.168.2.128)' can't be established.
	ECDSA key fingerprint is 7f:a5:75:69:66:07:d9:1a:90:e5:42:1a:91:5a:ab:65.
	Are you sure you want to continue connecting (yes/no)? yes
	Warning: Permanently added '192.168.2.128' (ECDSA) to the list of known hosts.
	serverdemo@192.168.2.128's password: ******
	Welcome to Ubuntu 12.04.2 LTS (GNU/Linux 3.2.0-37-generic x86_64)

	* Documentation: https://help.ubuntu.com/

	System information as of Sun Feb 10 10:02:59 EST 2014

	System load: 0.0 Processes: 94
	Usage of /: 7.2% of 35.20GB Users logged in: 1
	Memory usage: 7% IP address for eth0: 192.168.2.128
	Swap usage: 0%

	Graph this data and manage this system at https://landscape.canonical.com/
	Last login: Sun Feb 10 09:59:57 2014
	/usr/bin/xauth: file /home/serverdemo/.Xauthority does not exist
	serverdemo@serverdemo:˜$ sudo sosetup
	[sudo] password for serverdemo: 
	```

### 5: SO Housekeeping

* Update can be done via gui or `apt-get`

* Limiting access to SO

	* `sudo ufw access`

	* Limit the number of services that listen on public interfaces and instead bind to non-public interfaces such as localhost

	* You can setup an SSH proxy from an authorized remote client to connect to the SO server

* Connecting via a SOCKS proxy

	* Setting up a SOCKS proxy will allow you to remote access an application listening only on localhost

	* Steps for Putty

		1. Connection > SSH > Tunnels

		2. Select `dynamic` and `auto`. Click Add then Open

		3. In your browser, configure proxy setting to 127.0.0.1

	* Steps for Linux client

		* `ssh -L 9876:localhost:9876 username@SO SRVIP

* Changing the firewall policy

	* `sudo ufw allow 9876/tcp`

	* `sudo ufw deny 9876/tcp`

* Managing SO data storage

	* `/nsm` - stores logs and full content data. Uses a lot of space in comparison to DB

	* `/var/lib/mysql` - SO's database

* Managing sensor storage

	* SO cronjob are automatically run every hour to clean up `/nsm/` log directory

	* `/usr/bin/nsm_sensor_clean`

* Checking database drive usage

	* login using `mysql -u root`

* Managing the Sguil database

	* `sguil-db-purge` script to manage `securityonion_db` database

		* Run the script runs, it removes data older than 365 days

	* `/etc/nsm/securityonion.conf`

		* `DAYSTOKEEP` variable adjusts the time to manage log size