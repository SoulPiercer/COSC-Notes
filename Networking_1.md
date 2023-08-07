# Networking Day 1

https://net.cybbh.io/public/networking/latest/index.html
# Internet Host
ssh student@10.50.45.7 -X
password: password
# CTFD-2
 http://10.50.23.147:8000/
 user/pass: NIRI-505-M

## Mathematics of Networking

![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/efad4117-0cac-4634-99a6-160816eccc93)

## OSI Model
  
    Please do not throw sausage pizza away
    


## Double tagging:
  
      trying to bypass switch configuration. 
## (2) Data Link Layer
    Data Link Sub-Layers
            MAC (Media Access Control) 
            LLC (Logical Link Control)
            
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/ab91af55-1485-4232-8219-5d179e8cda1a)

![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/748fb98c-8b34-49c6-bd4b-397cae0992bb)
#### arp cache
        arp -a
        ip neigh  ### Replacing arp ###
ARP HEADER        
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/92966807-355b-48fc-9331-f0cb99c6ed81)


## (3) Network Layer
### IPV4 HEADERS
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/38ff8fc2-33fc-45fb-8e3d-11d2ef0095c3)

      # DSCP Identifies type of traffic
      # Bit shift or *4 to get value of byte

### FRAGMENTATION PROCESS
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/a609715f-c445-4b96-ae88-2c86e3663753)

      Offset is incremented by 185 each time
      Dat Incremented by 1480 Bytes = 1500 - 20 (Internet Header)
      
      
### IPV6 Headers
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/e1545f15-ea06-4733-ab62-95a44fad1f26)


### FingerPrinting
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/a96930d7-9e1b-4d2e-9f79-38b8f1fba22a)

### ICMP HEADER

![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/aad00760-d17b-4704-ade6-53490145dd9c)

## (4) Transport Layer
TCP or UDP

### TCP HEADERS
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/836d9eb8-678a-48ee-a552-03bf2e61ae82)

### TCP FLAGS
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/f0868bda-1e61-4171-ae47-323e45dde3dc)

### TCP CONNECTIONS
![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/bcd088b6-62bf-498a-8603-ecf55941067f)

1) Connection Establishment ( 3-way Handshake )
2) Data Transfer (Established )
3) Connection Termination ( 4-way Termination)

### UDP HEADERS

![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/a30089e2-1380-4364-8c6d-24f9b0b6e355)


## (5) Session Layer
Protocols
  -Socks
  -NetBios
  -PPTP/L2TP (VPN's)
  -RPC

## (6) Presentation Layer
Responsibilities
  -Translation
  -Formating
  -Encoding
  -Encryption
  -Compression

## (7) Application Layer

# FINISH NOTES FTP 
  TCP 20/21
  ACTIVE / PASSIVE
  
# FINISH NOTES SSH
 SSH (TCP 22) 

      TELNET (TCP 23)
      SMTP (TCP 25)
      TACACS (TCP 49) SIMPLE/EXTENDED
      HTTP(S) (TCP 80/443)
      POP (TCP 110)
      IMAP (TCP 143)
      RDP (TCP 3389)
      DNS (QUERY/RESPONSE) (TCP/UDP 53)
      DHCP (UDP 67/68)
      TFTP (UDP 69)
      NTP (UDP123)
      SNMP (UDP 161/162)
      RADIUS (UDP 1645/1646 AND 1812/1813)
      RTP (UDP any above 1023)
      
## NETWORK TRAFFIC SNIFFING


  ### Berkeley Packet Filters (BPF)

        Used with Operators, bitmasking, and TCPDump to create a tool for traffic filtering and parsing
    
    tcpdump {A} [B:C] {D} {E} {F} {G}

    A = Protocol (ether | arp | ip | ip6 | icmp | tcp | udp)
    B = Header Byte offset
    C = optional: Byte Length. Can be 1, 2 or 4 (default 1)
    D = optional: Bitwise mask (&)
    E = Operator (= | == | > | < | <= | >= | != | () | << | >>)
    F = Result of Expresion
    G = optional: Logical Operator (&& ||) to bridge expressions
    
    Example:
    tcpdump 'ether[12:2] = 0x0800 && (tcp[2:2] != 22 && tcp[2:2] != 23)'

  ### Bitwise Masking

  #Filter down to bit ; not just byte

Filter down to IP header Length  greater than 5
ip[0] & 0x0F > 0x05

  ![image](https://github.com/SoulPiercer/COSC-Notes/assets/108113301/fe86730c-e014-4d8d-b9b7-69a6c983f31d)

  Brackets select first byte; 0x0F selects 2nd Nibble in 1st byte.

  Most Exclusive
    
     Filters for flag and fragment offset field with ONLY Don't Fragment flag on.
     ip[6:2] & 0xE000 = 0x4000 

    Filtering on a specific Ip Address
      tcpdump '(ip[12] = 192 && ip[13] = 168 && ip[14] = 14 && ip[15] = 2)'
      ip[12:4] & 0xFFFFFFFF = 0xC0A80E02
      
   Less Exclusive

      Filters for the Don't Fragment flag AND any other bits on.
      ip[6:2] & 0x4000=0x4000

      Filter on S.IP = 192.168.14.0/24 subnet
      ip[12:4] &0xFFFFFF00 = 0xC0A80E00
          
  Least Exclusive

      At least one of the designated bits must be set to not equal 0; all other may be set

      ip[6:2] & 0x1FFF!=0
      tcp[13] & 0x12 != 0 
      
## Layer 2 Switching Technologies

Fast Forward - Only Destination MAC
Fragment Free - First 64 bytes
Store and forward - Entire Frame and FCS

### Double tagging

    0x88A80002 --> 0x81000064
    0x91000002 --> 0x81000064
    


# CTFD Basic Analysis

   All ip and ipv6 with udp 
      
      ip[9]=17||ip6[6]=17

   
