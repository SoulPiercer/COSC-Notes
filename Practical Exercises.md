# Practical Exercises

# Day 1 
## Find Cmdlets
  1.) Update Help system
  
    update-help -Force -ErrorAction SilentlyContinue
  2.) Viewing and manipulating of processes 
    
    Get-Process 
  3.) List of Services
  
    Get-Service
  4.) Write or output objects/text --> screen
  
    write-output / write-host
    
  5.) Modify, list, and delete variables
  
    Get-Variable / remove-variable / clear-variable / set-variable / New-variable 
  6.) Find and list cmdlets
  
    """ Get-Command / get-verb / get-noun """
  7.) Prompt user for input
    
    Read-host

## Running Cmdlets
  1.) Display list of running Processes

    Get-Process
  2.) Display list of all running processes that start with letter "s"
  
    Get-Process -name s*
  3.) Find cmdlet and purpose for aliases
  
    Get-Alias gal , dir , echo , ? , % , ft
  4.) Display list of Windows firewall rules 
  
    Get-NetFirewallRules
  5.) Create a new alias called "gh" for Get-Help
  
    set-alias gh Get-Help 
    Get-Alias gh

## Variables
    $var1 = Get-Random -Minimum 25 -Maximum 50
    $var2 = Get-Random -Minimum 1 -Maximum 10
    $sum = $var1 + $var2 
    $sub = $var1 - $var2
    $prod = $var1 * $var2
    $quo = $var1 / $var2
    '{0} + {1} = {2}' -f $var1, $var2, $sum
    '{0} - {1} = {2}' -f $var1, $var2, $sub
    '{0} * {1} = {2}' -f $var1, $var2, $prod
    '{0} / {1} = {2}' -f $var1, $var2, $quo

## Reverse Arrays
    $myarray = -5..12
    $myarray
    $reversearray = $myarray | Sort-Object -Descending
    $reversearray

## Arrays & Hash Tables
    $employee1 = @{}
    $employee2 = @{}
    $employee1= [ordered]@{"First" = "Mary"; "Last" = "Hopper"; "ID" = 001; "Job" = "Software Developer"}
    $employee2= [ordered]@{"First" = "John"; "Last" = "Williams"; "ID" = 002; "Job" = "Web Developer"}
    $employee1.username = "M" + $employee1["Last"] + "001"
    $employee2.username = "JWilliams002"
    $employee1.Job = "Software Lead"
    $employee3 = [ordered]@{"First" = "Alex"; "Last" = "Moran"; "ID" = 003; "Job" = "Software Developer"}

    $employee1["Status"] = "Management"
    $employee2["Status"] = "Intermediate"
    $employee3["Status"] = "EntryLevel"

## The Pipeline
    Get-Process | select name, StartTime -First 1 -Last 1
    
    Get-Date | select -Property Dayofweek
    get-help get-date
    get-date
    Get-HotFix | sort -Property InstalledOn | select InstalledOn , HotFixId
    Get-HotFix | sort -Property Description | select Description , InstalledOn , HotFixId
