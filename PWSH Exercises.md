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
  1.) Create a variable that holds random number between 25-50
  
    $var1 = Get-Random -Minimum 25 -Maximum 50
  2.) Create a variable that holds a random number btwn 1-10
  
    $var2 = Get-Random -Minimum 1 -Maximum 10
  3.) Create a variable that holds the sum of two values
  
    $sum = $var1 + $var2 
  4.) Create a varialbe that holds the diference on two values
  
    $sub = $var1 - $var2
  5.) Create a variable that holds the product of two values
  
    $prod = $var1 * $var2
  6.) Create a variable that holds the quotient of two values
  
    $quo = $var1 / $var2
  7 - 10.) Replace the variables in text with their values in the following format:

  "var1" + "var2" = "sum"
  "var1" - "var2" = "sub"
  "var1" * "var2" = "prod"
  "var1" / "var2" = "quo"

    '{0} + {1} = {2}' -f $var1, $var2, $sum 
    '{0} - {1} = {2}' -f $var1, $var2, $sub
    '{0} * {1} = {2}' -f $var1, $var2, $prod
    '{0} / {1} = {2}' -f $var1, $var2, $quo

## Reverse Arrays
  Create an array containing a range with a random starting and stopping point. The starting point will be a random number from -10 through 0. The ending point will be a random number from 1 through 20.
  
    $start = get-random -Minimum -10 -Maximum 0
    $end = get-random -Minimum 1 -Maximum 20
    $myarray = $start .. $end
    $myarray
   Create a variable that contains the contents of the array in reverse
 
    $reversearray = $myarray | Sort-Object -Descending
    $reversearray

## Arrays & Hash Tables
  Make two empty arrays
  
    $employee1 = @{}
    $employee2 = @{}
    
  Add Key - Value for each employee 
  
    $employee1= [ordered]@{"First" = "Mary"; "Last" = "Hopper"; "ID" = 001; "Job" = "Software Developer"}
    $employee2= [ordered]@{"First" = "John"; "Last" = "Williams"; "ID" = 002; "Job" = "Web Developer"}
    
  Add new Key "Username" which holds a contraction of the employee’s first initial then last name then ID. (i.e. mhopper001).
  
    $employee1["username"] = $employee1.first.substring(0,1)+$employee1.last + $employee1.id
    $employee2["username"] = $employee1.first.substring(0,1).tolower() + $employee1.last.tolower() + $employee1.id
    
  Update Job Value 
  
    $employee1.Job = "Software Lead"
    
  Create new Employee
  
    $employee3 = [ordered]@{"First" = "Alex"; "Last" = "Moran"; "ID" = 003; "Job" = "Software Developer"}
    
  Add new Key "Status" to each employee
  
    $employee1["Status"] = "Management"
    $employee2["Status"] = "Intermediate"
    $employee3["Status"] = "EntryLevel"

## The Pipeline
  1.) Display start time of earliest and latest running processes
  
    Get-Process | select name, StartTime -First 1 -Last 1

  2.) Identify cmdlet that returns current date and time then use "Select-Object" to display only current day of week
  
    Get-Date | select -Property Dayofweek

  3.) Identify a cmdlet that displays a list of installed hotfixes

    Get-HotFix

  4.) Extend the expression to sort the list by install date and dsiplay only the install date and hotfix id
  
    Get-HotFix | sort -Property InstalledOn | select InstalledOn , HotFixId



  5.) Extend the expression further, but sort by description, include description, hotfix id and install date
  
    Get-HotFix | sort -Property Description | select Description , InstalledOn , HotFixId


# Day 2 

## Custom Object 

    $computersystem = ((Get-WmiObject -class win32_computersystem).name )
    $operatingsystem = ((Get-WmiObject -class win32_operatingsystem).name)
    $version = ((Get-WmiObject -class win32_operatingsystem).version )
    $manufacturer = ((Get-WmiObject -class win32_Bios).manufacturer)
    $deviceid = ((Get-WmiObject -class win32_logicaldisk) | select -Property path )
    
    (Get-WmiObject -class win32_logicaldisk) | select -Property path
    add-member -InputObject $customobject noteproperty Win32_ComputerSystem $computersystem -Force
    add-member -InputObject $customobject noteproperty OperatingSystem $operatingsystem 
    add-member -InputObject $customobject noteproperty Version $version 
    add-member -InputObject $customobject noteproperty Manufacturer $manufacturer
    add-member -InputObject $customobject noteproperty Disks $deviceid -Force
    (Get-WmiObject -class win32_logicaldisk) | Select-Object -Property path

## Comparison and Condition


        $test1 = {$line1 | Select-String -pattern "MT5437" -SimpleMatch }
        if (& $test1) {
        write-host "MT5437"
        }
        else{
        write-host "No model Number found"
        }
        $test2 = {$line2 | Select-String -pattern "MT5437" -SimpleMatch }
        if (& $test2) {
        write-host "Model number"
        }
        else{
        write-host "No model Number found"
        }
