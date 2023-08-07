# Networking Day 2
# Sockets

## Stream Sockets
  Connection orented and Sequenced
  Connection establishment and teardown
  TCP ; SCTP ; BLUETOOTH
  
## Datagram Sockets
  Connectionless; designed for quickly sending and recieving data
  UDP
## Raw Sockets
  Direct sending and receiving of IP Packets without automatic protocol-specific formattng.

ss -nl
ss -t
ss -u

## User Space Sockets:
  * Stream Sockets
  * Datagram Socket
    The most common sockets that do not require elevated Privs

        tcp dump or wireshark to read file
        nmap with no switches
        netcat to connect to listener
        netcat to create listener on ports 1024+
        using /dev/tcp or /dev/udp to transmit data
    
## Kernel Space Sockets:
  * Raw Sockets
    Require elevated Privs to access hardware ie.) NIC's
    SUDO  with raw sockets for total packet analysis

        usig tcpdump or wireshark to capture packets on wire
        using nmap for OS identification
        using netcat to creat listner for ports 0-1023
        using Scapy to craft or modify a packet for transmission

# Networking Programming w/ python3

  Python3 Socket Library and socket.socket function

      import socket
          s = socket.socket(socket.FAMILY, socket.TYPE, socket.PROTOCOL)

  socket.socket([*family*[,*type*[*proto*]]]
   familty cnstats should be: AF_INET (default), AF_INET6, AF_UNIX
   type constants shoud be SOCK_STREAM (default, SOCK_DGRAM, SOCK_RAW
   proto constants should be: 0 (default), IPPROTO_RAW

## Libraries 
    Socket = https://docs.python.org/3/library/socket.html
    Struct = https://docs.python.org/3/library/struct.html
    Sys = https://docs.python.org/3/library/sys.html

### Python scripting

    # import socket
    
    # Call a function
    # socket.socket()
    
    # Rename a function library
    # from socket as sock
    # sock.function
    
    #import a specified function so as to call it with out referencing the library
    # from sock import function
    # function()
    
    # Imports everything from socket
    # from socket import *
    # function1()
    # function2()
    
    
#### Stream Socket(TCP)    
    
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/68070a0d-7134-4dbe-8d78-42ebe4f5bbbe)
Run nc command in separate window before running script

#### DATAGRAM Socket 
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/89dfe1e3-5bd0-42f9-9410-466f7d2737eb)
Run nc command in separate window before running script

#### RAW Socket
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/2b00de83-f1c3-43be-bfaa-f45725d1b090)

![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/541006a6-a256-4bb3-956d-da54ab8a7de7)
#### RAW TCP Socket

![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/0eb81167-d62f-4717-bb0f-682ed068be65)
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/67d72f66-4c65-4d74-a6de-a41f41255f61)
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/1de68ed8-f50f-4e96-b375-f281f54f7b5f)


*** Don't forget to change ip_proto field in th IPv4 Header to 6 (TCP)


  
