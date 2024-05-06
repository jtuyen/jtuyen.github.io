+++
title = "SickOS 1.2"
date = 2019-02-14
[taxonomies]
tags = ["vulnhub"]
+++

1. Initiated nmap scans.

	* Port 22 and 80 are open.

2. Initiated `dirbuster` and `nikto` scans.

	* `dirbuster` showed that it found a `/test` folder. Navigating to it shows a blank directory.

3. I got stuck with wondering what I should do here. I looked at the video for a hint and they executed this command:

	```bash
	curl -v -X OPTIONS http://sickos/test
	```

	* This cURL clue is something I should have guessed if I thought about it harder. It's like probing with telnet except it's a URL.

	* The cURL result came back as `PUT` allowable. Going to try and upload a shell:

	```bash
	nmap -p 80 sickos --script http-put --script-args http-put.url='/test/portscan.sh',http-put.file='portscan.sh'
	```

## Today I learned

* Good for future reference of where the wordlists are located"

```bash
/usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
```

* `Dirbuster` in headless mode is useless when you need information fast because of how it displays information in the terminal.

* This command says file has been uploaded but it only creates the filename but zero bytes transferred. Use the nmap PUT method instead.

```bash
curl -v -X PUT -d 'nikto3.log' http://sickos/test/nikto3.log
```

* `setookit`

* No need to create a bash script, just use nc to scan the port ranges.

```bash
nc -zv -w 1 localhost 1-1024
```

* [https://nmap.org/nsedoc/scripts/http-put.html](https://nmap.org/nsedoc/scripts/http-put.html)

## Improvement for next time

* I should have known that the ports were locked down because I couldn't retrieve files using `wget`. Next time if I do have lowpriv shell with access to `nc`, use the port scan switches. Although, it didn't work for me as it kept on timing out. I think only port 443 is allowed but it's occupied by my own `nc` port to catch the shell. If this is the case, keep using the `nmap` `PUT` method as a transfer method on port 80.

* Learning more about privesc for linux. More specifically, I should learn about where to look for escalation possiblities such as application exploitation or simple cronjobs. So far I've been hunting for kernel exploits and this time it didn't work out but no exploits were able to execute on this system.