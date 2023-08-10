# Day 5

## Network Tunneling

### SSH
  Used for tunneling

  * SSH Port Forwarding

          allows for tunneling of other services
  * Local Port Forwarding
    Creates a local port (1111) on the local host that forwards to a target machine’s port 80.
    
          ssh -p <optional alt port> <user>@<pivot ip> -L <local bind port>:<tgt ip>:<tgt port> -NT
          ssh student@172.16.82.106 -L 1111:localhost:80 -NT
    
          or

          ssh -L <local bind port>:<tgt ip>:<tgt port> -p <alt port> <user>@<pivot ip> -NT
          ssh -L 1111:localhost:80 student@172.16.82.106 -NT
          Test connection from the host machine:
                wget -r localhost:1111
    
    * SSH Local Port Forwarding Through a Local Port
   
          Internet Host:
          ssh student@172.16.1.15 -L 1111:172.16.40.10:22 -NT
          ssh student@localhost -p 1111 -L 2222:172.16.82.106:80 -NT
          firefox localhost:2222
    * SSH Dynamic Port Forwarding
   
          ssh -D <port> -p <alt port> <user>@<pivot ip> -NT
        * Proxychains default port is 9050
        * Allows the use of scripts and other userspace programs through the tunnel.
        * SSH Dynamic Port Forwarding 1-Step
     
              Blue Private Host-1:
              ssh student@172.16.82.106 -D 9050 -NT

              proxychains ./scan.sh
              proxychains ssh student@10.10.0.40

        * SSH Dynamic Port Forwarding 2-Step

              Blue Private Host-1:
              ssh student@172.16.82.106 -L 1111:10.10.0.40:22 -NT
              ssh student@localhost -D 9050 -p 1111 -NT

              proxychains curl ftp://www.onlineftp.ch
              proxychains wget -r www.espn.com
              proxychains ./scan.sh
              proxychains ssh student@172.16.101.2

### Chainging tunnels
* Three hosts
  * host -> pivot ip -> tgt ip
  * host    tolby       colby

         H> ssh toby@<pivot ip> -L 42100:<tgt ip>:<tgt port> -NT
         H> ssh -p 42100 colby@127.0.0.1
  allows for ssh connection to colby from internet host machine.
  * 4 Hosts
  * host - > ip -> ip -> tgt ip
  * host    tolby  colby  steve
  
        h> ssh toly@tolby -L 42100:colby:22 -NT
        h> ssh -p 42100 colby@127.0.0.1 -L 42001:steve:23 -NT
        test connection: telent loopback 42100
            ehlo

### Remote Port Forwarding -R
Since we have ssh connection to colby, but not steve, we need to create a tunnel to forward traffic from port 42199 on colby to steve open an ssh connection on steve 
      
      steve> ssh colby@colby ip -R 42199:127.0.0.1:22 -NT

ssh into colby and then forward traffic from port 42101 on internet host to loopback:42199 on colby

      h> ssh -p 42100 colby@127.0.0.1 -L 42102:127.0.0.1:42199 -NT

  Test ssh conncetion to steve
  
      h> ssh -p 42102 steve@127.0.0.1

Creat remote port forwarding from steve to dj

      steve> ssh steve@127.0.0.1 -R 42198:dj:25 -NT 
      steve> telnet 127.0.0.1 42198
          ehlo
Local Port forwarding from host to steve to allow connection to dj
  
      h> ssh -p 42102 steve@127.0.0.1 -L 42103:127.0.0.1:42198 -NT
      h> telnet 127.0.0.1 42103
          ehlo

      
Now, we can ssh from the local host into steve, from there, we can use remote port forwarding to connect to additional hosts down the line. 

  * 5 Hosts
  * host -> ip -> ip -> ip -> tgt ip
  * host    toly  colby  steve  dj

         


### Remote Port FOrwarding

      ssh -p <optional alt port> <user>@<remote ip> -R <remote bind port>:<tgt ip>:<tgt port> -NT

      or
      
      ssh -R <remote bind port>:<tgt ip>:<tgt port> -p <alt port> <user>@<remote ip> -NT

      
