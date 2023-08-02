# Networking Day 1

https://net.cybbh.io/public/networking/latest/index.html


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


  
