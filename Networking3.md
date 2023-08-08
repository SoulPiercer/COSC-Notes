# Day 3
# Network Reconnaissance

![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/3da8dd9b-467e-478b-8477-64be5a3afe85)


1.) Passive External
    OSINT:
      Zone Transfers
      DNS Lookups
      Web Recon
      Google Searches
      Shodan
2.) Active External
    NMAP scans
    Ping scans
    traceroute
    bannergrabbing
3.) Passive Internal  
    Arp cache
    Running Services
    Known Routes
    Interface IP's
    Open Ports
    other users on box
    DNS checks /etc/hosts
    and /etc/rsolv.conf
4.) Active Internal
    DNS Queries
    Ping sweep /ARP Requests

## Passive

whois (domain)


Dig is a tool that returns key DNS information and can be used to supplement the query for specific records.

instructor@net1:~$ dig ccboe.net
instructor@net1:~$ dig ccboe.net MX
instructor@net1:~$ dig ccboe.net SOA

Netcraft

## Scanning
nmap -A -T4 --min-rate 10000 -Pn <target Host/netork> -vvvv

### netcat script:
scan.sh

    #!/bin/bash
    echo "Enter network address (e.g. 192.168.0): "
    read net
    echo "Enter starting host range (e.g. 1): "
    read start
    echo "Enter ending host range (e.g. 254): "
    read end
    echo "Enter ports space-delimited (e.g. 21-23 80): "
    read ports
    for ((i=$start; $i<=$end; i++))
    do
        nc -nvzw1 $net.$i $ports 2>&1 | grep -E 'succ|open'
    done
    # (-v) running verbosely (-v on Linux, -vv on Windows),
    # (-n) not resolving names. numeric only IP(no D.S)
    # (-z) without sending any data. zero-I/O mode(used for scanning)
    #(-w1) waiting no more than 1second for a connection to occur
    # (2>&1) redirect STDERR to STDOUT. Results of scan are errors and need to redirect to output to grep
    # (-E) Interpret PATTERN as an extended regular expression
    # ( | grep open) for Debian to display only open connections
    # ( | grep succeeded) for Ubuntu to display only the open connections

### Passive Internal Commands
  cat /etc/hosts
  cat /etc/resolv.conf
  netstat -ntulp      # Check open ports (deprecated)
  ss -ntulp           # Check open ports
  ps -elf / top       # Check all running processes
  w/who               # Identify users logged onto box
  ifconfig            # Interface IP's (deprecated)
  ip address          # Interface IP's
  ip neighbor         # Arp Cache
  ip route            # ip routes

## Mapping

0.0.0.0:[port #] will allow access to port from any ip address

6010, 6011, 6012 == x11 forwarding

# Host Discovery: 
Find: Hostname ; IpAdress ; ports

## Ping sweep
for i in {1..254}; do ping -c 1 -W 1 10.1.1.$i | grep 'from'; done
## Nmap
nmap -vv ip/cider
#Default is top 1000 most used ports, scarcely use default
#Most devices will use 21,23,80 unless otherwise given a hint
nmap -Pn 172.16.20.0/24 -p 21,22,23,80 | egrep "open" -C4
nmap -Pn 172.16.20.0/24 -p 21,22,23,80 | egrep "filtered" -C4

Banner Grabbing:
  nc  <ip> <port>

## Web Requests:
wget -r <ip>    # gets content of a webpage and saves it file
wget -r ftp:<ip>
curl <ip>     # gets url of webpage
# Internal Discovery

## ftp server
ftp <ip>
user: anonymous, no password
## Routers:
  sh config   # bgp 10 to find external networks  # 
  show int    # Find internal network connections
  hostname
  ss -ntlp
  
