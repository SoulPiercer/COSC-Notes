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
  host -> pivot ip -> tgt ip
  host    tolby       colby

     H> ssh toby@<pivot ip> -L 42100:<tgt ip>:<tgt port> -NT
     H> ssh -p 42100 colby@127.0.0.1
  host - > ip -> ip -> tgt ip
  host    tolby  colby  steve
  
    h> ssh toly@tolby -L 42100:colby:22 -NT
    h> ssh -p 42100 colby@127.0.0.1 -L 42001:steve:25 -NT
    test connection: telent loopback 42100
    





SSH Local and Dynamic Practice
Start
tp1
SSH Remote Port Forwarding

    Syntax

ssh -p <optional alt port> <user>@<remote ip> -R <remote bind port>:<tgt ip>:<tgt port> -NT

or

ssh -R <remote bind port>:<tgt ip>:<tgt port> -p <alt port> <user>@<remote ip> -NT

