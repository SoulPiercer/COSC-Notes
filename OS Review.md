OS Review




Windows
Will have to manually download sysinternals.

netstat -ano
tasklist
get-process

sysinternals tools:
  autoruns
  tcpview

Persistence:
  Powershell profiles
  Windows Registry 5.4 registry
  
Linux:
view network connections: netstat -ano
view open ports with processes: ps -elf
Suspicious processes-

- run with high PID
- Misspelling

Persistence:
crontabs | cronjobs| /etc/profile | /etc/inittab 
