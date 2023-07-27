# Day 1

#1 10.50.43.15  ----- Personal Admin IP

Admin Workstation ==== xfreerdp /u:andy.dwyer /v:10.50.43.15 -dynamic-resolution +glyph-cache +clipboard

CTFD ==== http://10.50.24.129:8000/

username: NIRI-M-505
password: N1ckn@ckpattywack

class: M23505
Password: password


## OS Module public repo
https://os.cybbh.io/public/os/latest/index.html

## Windows venv
xfreerdp /u:student /v:10.50.29.77 -dynamic-resolution +glyph-cache +clipboard

### Powershell
    get-nettcpconnection
    get-verb
    
    get-command
    Get-Command -type Cmdlet | sort -Property Noun  | ft -groupby Noun
    
    get-process | get-member
    get-process | gm | where-object{ $_.membertype -match "Property"} # returns more than just "Property"
    get-process | gm | where-object{ $_.membertype -eq "Property"} # returns exactly properties

    start-process notepad.exe
    stop-process -name notepad
    (get-process notepad*).kill()

    get-process | select name,id,path | where-object { $_.id -lt 1000}
    (get-process | select name,id,path | where-object { $_.id -lt 1000}).count #returns count of process with a pid < 1000

    ## Cim Classes & instances
    get-cimclass *
    
    get-ciminstance -classname win32_logicaldisk # or 
    get-wmiobject -class win32_logicaldisk
    
    get-ciminstance -classname win32_logicaldisk -filter "drivetype=3" | get-member
    get-wmiobject -class win32_logicaldisk -filter "drivetype=3"

	(Get-CimInstance -ClassName Win32_Service | where-object{$_.name -eq "Legoland"}).Description
    
    get-executionpolicy -list
    set-executionpolicy -ExecutionPolicy Unrestricted -scope CurrentUser


    
### Automatic Variables 2.3.2 Student Facilitation Guide
    


    

## Powershell Profiles

  <# Method for creating persistence #>
  
  $profile alias and functions
  evertime powershell runs, whatever is stored in the powershell scripts here will be run.
    $profile 

  # test for profile in one of four specified location
  test-path $profile.allusersallhosts
  # create new pwsh profile
    new-item -itemtype file -path $profile -force


### Windows Remoting

Create a new location to remote into
Get-Item WSMan:\localhost\client\TrustedHosts 
Set-Item WSMan:\localhost\Client\TrustedHosts -Value "Server01, domain-controller, workstation2" -concatenate # append multiple items - Without concatenate, will overwrite list

### Remoting Commands
  Temporary sessions
#creating a new pssession
$session = new-pssession -computername workstation2 -credential andy.dwyer

Invoke-Command -ComputerName File-Server {Get-Service}                                      # Creates 1-to-1 Temporary Session
Invoke-Command -ComputerName File-Server,Domain-Controll,Workstation2 {Get-Service} -asjob  # Running a Temporary Session as a Job

Receive-Job <job #> 


# 6 .NET API Framework



Download a File with Powershell

    $url = "http://downloads.volatilityfoundation.org/releases/2.6/volatility_2.6_win64_standalone.zip"
    $output = "$PSScriptRoot\volatility_2.6_win64_standalone.zip"
    $start_time = Get-Date
    
    $wc = New-Object System.Net.WebClient # (1)
    $wc.DownloadFile($url, $output) # (2)
    
    
    (New-Object System.Net.WebClient).DownloadFile($url, $output)

  (1) Create a new Webclient object
	(2) Use the Downloadfile method to download a file



# Day 2
## Windows Registry
HKLM\hardware is the volatile hive

GUI : C:\windows\regedit.exe
cli : C:\windows\system32\reg.exe 

Powershell: 
	Root Hive Keys are loaded as powershell drives
 Extract Info:
 	
  	get-item # gets all values listed under subkey
   	get-itemproperty 
    	get-childitem # retrieves all subkeys 
  Change information:
  	
   	set-itemproperty, remove-itemproperty, new-item, new-itemproperty
   
Mount a Remote Regisry via Regedit

Using Powershell Registry
	Query : Analyze : Modify : create
Using PSDRIVE
	new-PSDrive -name HKU -PSProvider Registry -Root HKEY_USERS
 
Every Key is read at startup : changes will be made at reboot 



## Alternate Data streams
		echo "Always do your best" > reminder.txt
		Set-Content .\reminder.txt -Value "social security numbers" -Stream secret.info
		Get-Item reminder.txt -Stream * 
		Get-Content reminder.txt -Stream secret.info 
		

# Day 3
# Linux
ssh -J andy.dwyer@ADMIN user@ip


use strings on binary file to make it human readable



# Day 4

# Windows Boot Process

## BCDEDIT Demo

	Display current BCD config:
 		bcdedit.exe
  	Backup BCD Configuration : 
		bcdedit /export C:\SAVEDBCD
  
c:\Windows\system32\sc.exe

.\tasklist.exe /svc

reg queries 


	.\bcdedit.exe
Back it up: 
	
 	.\bcdedit.exe /export C:\BCD.bk
 
  
## Persistence     
	services.exe(0) --> SCM Starts all services automatically with highest priviledges.
	Registry Keys



# Day 6
sysinternals
	accesschk.exe

# UAC

	net use * http://live.sysinternals.com
 	z:
	
 	new-psdrive -name "SysInt" -PSProvider FileSystem -Root "\\live.sysinternals.com\Tools"

	$wc = New-Object System.Net.WebClient 
	# $wc.DownloadFile($url, $output)
	$wc.DownloadFile("https://download.sysinternals.com/files/SysinternalsSuite.zip","$pwd\SysinternalsSuite.zip")
 
 	./strings.exe C:\Windows\System32\*.exe -accepteula | select-string -simplematch "autoelevate"

   	./sigcheck -m C:\Windows\System32\slui.exe -accepteula | Select-String -SimpleMatch "level"

# Day 7 Linux Process Validity

	
Interacting with services on Systemd --> systemctl
		systemctl list-units 
  		systemctl list-units --all
		systemctl start/stop/restart <servicename.service>
  		sytsemctl status <servicename.service / PID of service>


# Day 8 Windows Artifacts, Auditing and Logging
## Getting User SID:
	
 	get-wmiobject win32_useraccount | select name,sid (PowerShell Local and domain users)

	Get-LocalUser | select Name,SID (PowerShell Local Users)

	wmic useraccount get name,sid (CMD.EXE ONLY)

*Determine name of SID:*
	
 	wmic useracount where 'sid="***"' get name

 View Executable Files run: Paste output into cyberchef using ROT13

  	Get-ItemProperty 'REGISTRY::HKEY_USERS\*\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{CEBFF5CD-ACE2-4F4F-9178-9926F41749EA}\Count'

View Shortcut files executed: Paste output into cyberchef using ROT13

 	Get-ItemProperty 'REGISTRY::HKEY_USERS\*\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{F4E57C4B-2036-45F0-A9AB-443BCFE33D9F}\Count'


## Windows Background Activity Monitor (BAM)

 	Get-Itemproperty 'HKLM:\SYSTEM\CurrentControlSet\Services\bam\UserSettings\*' (Windows 1709 & 1803)

	Get-Itemproperty 'HKLM:\SYSTEM\CurrentControlSet\Services\bam\state\UserSettings\*' (Windows 1809 and newer)

 	Get-ItemProperty 'registry::HKLM\SYSTEM\CurrentControlSet\Services\bam\UserSettings\*'

## Recycle Bin:

	gci 'C:\$RECYCLE.BIN' -Recurse -Verbose -Force | select *

	gci 'C:\$RECYCLE.BIN' -Recurse -Force

 


$RXXXXXX - content of deleted files

$IXXXXXX - original PATH and name

Location:

C:\$Recycle.bin (Hidden System Folder)

Get content of file in recylcing bin:
	
 	Get-Content 'C:\$Recycle.Bin\S-1-5-21-1584283910-3275287195-1754958050-1005\$R8QZ1U8.txt' 

## Prefetch:
	Created when when an application is run rom a specific location for the first time
 	
	gci -Path 'C:\Windows\Prefetch' -ErrorAction Continue | select * | select -first 5

## Jump Lists

allos users to "jump" or access items they have requently or recently used quickly and easily

	gci -Recurse C:\Users\*\AppData\Roaming\Microsoft\Windows\Recent -ErrorAction Continue | select FullName, LastAccessTime
or

- Make sure sysinternals is mounted or unzipped
- 	net use * http://live.sysinternals.com

- Gci C:\users\student\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations | % {z:\strings.exe -accepteula $_} >> c:\recentdocs.txt

## Recent Files

List Files in Recent Docs

	gci 'REGISTRY::HKEY_USERS\*\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs'

Convert File Hex to Unicode

	[System.Text.Encoding]::Unicode.GetString((gp "REGISTRY::HKEY_USERS\*\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.txt")."0")

Convert all of a users values from HEX to Unicode

    Get-Item "REGISTRY::HKEY_USERS\*\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.txt" | select -Expand property | ForEach-Object { [System.Text.Encoding]::Default.GetString((Get-ItemProperty -Path "REGISTRY::HKEY_USERS\*\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.txt" -Name $_).$_)}


## Browser Artifacts

History will record the access to the file on the website that was accessed via a link.

	.\strings.exe 'C:\Users\<username>\AppData\Local\Google\Chrome\User Data\Default\History'

Find FQDNs in Sqlite Text Files

	$History = (Get-Content 'C:\users\<username>\AppData\Local\Google\Chrome\User Data\Default\History') -replace "[^a-zA-Z0-9\.\:\/]",""
	
	$History| Select-String -Pattern "(https|http):\/\/[a-zA-Z_0-9]+\.\w+[\.]?\w+" -AllMatches|foreach {$_.Matches.Groups[0].Value}| ft

## Logs

Eventlogs
	-Application
 	-Security
  	-System
   	-CustomLog
	
Powershell 

	    View newest 10 System Logs

        Get-EventLog -LogName System -Newest 10 | ft -wrap

    View the entire message field in the Security Log

        Get-Eventlog -LogName Security | ft -wrap

    Search logs with mutiple criteria

        get-winevent -FilterHashtable @{logname="security";id="4624"} | select -first 5 | ft -wrap

 Powershell Transcript 
 
    Allows the capture of the input and output of Windows PowerShell commands into text-based transcripts.

        Start-Transcript

    View Powershell console History

        Get-History

    View entire powershell History

        Get-Content "C:\users\$env:username\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt"

# Day 11 Windows Active Directory Enumeration

	
