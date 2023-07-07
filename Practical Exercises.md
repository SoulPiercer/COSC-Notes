# Practical Exercises

# Day 1 
## Find Cmdlets
update-help -Force -ErrorAction SilentlyContinue
Get-Process
Get-Service
write-output 
Get-Variable 
""" Get-Command  get-verb """
Write-Output

## Running Cmdlets
Get-Process
Get-Process -name s*
Get-Alias gal , dir , echo , ? , % , ft
Get-NetFirewallRules
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
