+++
title = "Sauna"
date = 2020-03-16
[taxonomies]
tags = ["hackthebox"]
+++

1.	`nmap` scans show ports 80, 135, 53, and 389 are opened.

2.	I started with enumerating the SMB shares and see if anything would show up. Unfortunately, there wasn't any accessible shares.

	![htb-sauna-smbmap.png](htb-sauna-smbmap.png)

3.  Next service to be enumerated is DNS and see if I could get any zone transfers by guessing or PTR records. Again, there wasn't any DNS records that can be found.

	![htb-sauna-dig.png](htb-sauna-dig.png)

4.	Next service to be enumerated is LDAP using `ldapsearch`. I did find up finding a username "Hugo Smith". I tried guessing the password to see if I would login but no luck.

	![htb-sauna-ldapsearch.png](htb-sauna-ldapsearch.png)

5.	Next, I would proceed with the HTTP service. I found it was hosting a banking site called Egotistical Bank. Running `gobuster` in the background, I started to inspect the site code and eventually I found this page that could be in use for enumerating possible usernames.

	![htb-sauna-httpserver.png](htb-sauna-httpserver.png)
	
6.	I started to compile a list of possible usernames that most companies would use such as:
	- hsmith
	- hugos
	- hugosmith

7.	I used a tool called `kerbrute` python script to help enumerate the list of users that I created to validate the possible usernames. As you can see, it validated the user `hsmith`. Eventually I created the other possible users listed on the website and enumerated again. The second user found was `fsmith`.
	
	![htb-sauna-kerbrute.png](htb-sauna-kerbrute.png)

8.	Using the Impacket script, `GetNPUsers.py`, I used `fsmith` username and see if the ASREPRoast attack would work on this account. This attack looks for users without required Kerberos pre-authentication. The python script tries to harvest the non-preauth AS_REP responses for brute forcing. As you can see, an AS_REP response has been acquired.

	![htb-sauna-getnp.png](htb-sauna-getnp.png)

9.	Using `hashcat`, I saved the AS_REP response hash to a file and proceeded to deploy brute force attack. The hash has been cracked.

	```
	hashcat -m 18200 -a 0 ~/Documents/hackthebox/sauna/fsmith.txt /usr/share/wordlists/rockyou.txt --force
	```

	![htb-sauna-hashcatfsmith.png](htb-sauna-hashcatfsmith.png)

10.	With this new username and password, the next logical step is trying to connect using WinRM using `evil-winrm`. Once connected, I was able to obtain user.txt flag.

	![htb-sauna-winrm.png](htb-sauna-winrm.png)

### Privilege Escalation

1.	I began enumerating the Windows machines using `Winpeas`. It immediately found AutoLogon credentials in the registry for `svc_loanmgr`. You can also find this registry manually using a command. Using this newly found username and password, I used `evil-winrm` to login.

	![htb-sauna-winpeas.png](htb-sauna-winpeas.png)
	
	![htb-sauna-registry.png](htb-sauna-registry.png)
	
2.	Enumerating the system even further didn't find anything too obvious so tried to get the TGT ticket of other possible accounts but I ran into clock skew errors.

	![htb-sauna-gettgt.png](htb-sauna-gettgt.png)

3.	I had to remove `ntp` command from my system because it was conflicting with `timesyncd`.

	```
	apt purge ntp
	```
	
	Once `ntp` has been removed, I reconfigured my `timesyncd.conf` file to sync with Sauna.
	
	```
	sudo vim /etc/systemd/timesyncd.conf
	
	sudo systemctl restart systemd-timesyncd
	```
	
	![htb-sauna-ntpfix.png](htb-sauna-ntpfix.png)
	
	![htb-sauna-timesyncd.png](htb-sauna-timesyncd.png)
	
4.	Once the clock skew errors has been resolved, I used `GetUserSPNs.py` script to deploy Kerberoasting attack and see if I could obtain any TGS tickets from other users. How kerberoasting attack work is attempting to harvest TGS tickets for services that run on behalf of user accounts in the AD, not computer accounts. As you can see, `hsmith` TGS ticket has been obtained.

	![htb-sauna-getuserspn.png](htb-sauna-getuserspn.png)
	
	![htb-sauna-hsmith.png](htb-sauna-hsmith.png)

5.	I sent the TGS ticket to `hashcat` for brute forcing. The hash has been cracked and had the same password as `fsmith`.

	![htb-sauna-hashcathsmith.png](htb-sauna-hashcathsmith.png)

6.	After spending some time poking around using the `hsmith` credentials and enumerate services with it, I didn't find anything special about the account.

	![htb-sauna-otherremoteports.png](htb-sauna-otherremoteports.png)
	
7.	After digging further, I decided to see if I can dump the Administrator password using `secretsdump.py`. I got lucky. I sent the hash to `hashcat` and I was unable to crack the hash using `rockyou.txt`.

	![htb-sauna-secretsdump.png](htb-sauna-secretsdump.png)
	
	![htb-sauna-hashcatadministrator.png](htb-sauna-hashcatadministrator.png)

8.	Eventually, I was able to login as Administrator using `evil-winrm` using pass-the-hash technique. The root flag has been obtained.

	![htb-sauna-root.png](htb-sauna-root.png)











