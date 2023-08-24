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
