# Day 1

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