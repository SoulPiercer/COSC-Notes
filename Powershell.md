# PowerShell Day 1
### xfreerdp /u:student /v:10.50.29.77 -dynamic-resolution +glyph-cache +clipboard
https://cted.cybbh.io/tech-college/pns/public/pns/latest/powershell/pe_arrays_hash_tables.html
## Commands 
	
	Get-Command
	Get-Verb
	
	get-command -verb get
	get-command -noun process
	
	tasklist
	Get-Process
	
	Get-EventLog -LogName Application
	Get-EventLog -LogName *
	
	
	Get-Alias
	
	update-help -Force -ErrorAction SilentlyContinue
	get-help *print*
	get-help Get-Process -Examples
	
	get-help about_*
	
	get-help Get-Process -online
	
	Get-childitem -path C:\Windows -Filter *.exe -recurse -name

""" Aliases: """ 
#Not persistent through system unless saved to profile.

Get-Alias

# Find all aliases for a given command
Get-Alias -Definition Get-ChildItem

#creates an alias, edit that runs notepad.exe
set-alias edit notepad.exe

#remove an alias
remove-item alias:edit


Get-Process | Get-Member -MemberType Method

#Dot Notation
(Get-Process).Name

Get-process | Format-List -Property name, id
#kill a process 
 ( Get-Process notepad).Kill()


 get-service | Format-Table Name, status

 #find all options
 Get-Service | Format-Table *

 #Writing to the host
 function fruit-host{
 write-host "Banana"
 write-host "Orange"
 write-host "Pear"
 write-host "Apple"
 }
 fruit-host | sort 
 #Wont sort since it is written to host instead of output


 #Writing to the output
  function fruit-output{
 write-output "Banana"
 write-output "Orange"
 write-output "Pear"
 write-output "Apple"
 }
 fruit-output | sort 
 #will sort since its in output


 ## Variables
 Must be called an declared with $
 Get-Variable

## Data Types

 .GetType() returns BaseType
    $x = Get-Process
    
    $a = "HELLO WORLD"
    
    ($x).GetType()
    ($a).GetType()
    
    IsPublic IsSerial Name                                     BaseType                                                                                                                                                                                              
    -------- -------- ----                                     --------                                                                                                                                                                                              
    True     True     Object[]                                 System.Array                                                                                                                                                                                          
    True     True     String                                   System.Object 

    $a -is [array]
    False
    $a -is [string]
    True

## Arrays

    $x[0..3]
    
    Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName                                                                                                                                                                                            
    -------  ------    -----      -----     ------     --  -- -----------                                                                                                                                                                                            
        526      20     1972       5668       4.66    460   0 csrss                                                                                                                                                                                                  
        172      10     1708       5080       0.25    544   1 csrss                                                                                                                                                                                                  
        376      15     1908       6112       8.45   1668   2 csrss                                                                                                                                                                                                  
        417      16     3948      20408       1.66   4180   2 ctfmon 

  Empty array : $array = @()
  $multiarray = @((1,2,3,4),(5,6,7,8))

 ### Hash Table
    $mylist = @{First = "John"; Last = "Doe"; mid = "Bon"; Age = 35}
    $mylist =[ordered] @{First = "John"; Last = "Doe"; mid = "Bon"; Age = 35}
    $mylist.keys # Return all the keys
    $mylist.values # Return all values
    $mylist.last # Return specific value with key
    $mylist["Last","Age","First"] # return value for each key listed
    $mylist.date = Get-date # Adds new entry "date" = snapshot of date
    $mylist["location"] = "Fort Gordon" # adds new key value pair or changes it
    $mylist.Remove("location") # Removes location
    
 ## Pipelining
 $_ or $PSItem     object in current pipeline
   select-object 
   

$x, $y = 55, 45
Compare-Object $x $y

## Test-path variable:x
  remove-variable x

# Day 2 
## Commands
		gci
		get-service
		formattable
		sort-object
		get-member
		select-object
		group-object
		get-random
		where-object
		measure-object
		compare-object

## Script Blocks

	# Script Block 
    $myblock = {  get-service | ft name , status } 

#Call a sript block

    	& $myblock
	$a = 1
	$b = { 1+1 }
	$a += &$b
	$a 
	
## Sorting and Grouping
	
	gci | sort 
	gci | sort -Property length -Descending  | select Name , length -First 10
	
#Find Objects methods and properties
	
 	gci | Get-Member
	
#Sorting and grouping combined
	
 	gci | sort extension | ft -GroupBy extension
	
#Grouping

	get-service | Group-Object status 
### Pipeline Variable == $_  
	
 	#grabs whatever is passed through pipeline
	gci | Group-Object {$_.length -lt 1KB}
	
#Type casting

	1, 5, 3, 2, 4 | sort # Sorts as integers
	'1', '5', '3', '2', '4' | sort # sorts as strings
	'1', '5', '3', '2', '4' | sort -property {[Int]$_} #sort as integers
	
	
	1..10 | sort -Property { Get-Random }
	1..10 | Get-Random
	
#select-object Only objects in pipline are those specified
#Where-object will allow for more selection from entire pipeline. 
	
 	Get-Process | select -First 10
	
#Property values
	
 	#with headers
	Get-Process | select name, id
	
	#Without Headers
	Get-Process | select -Expandproperty name
	
	
## Filter results
	
	Get-Service | Where-Object{$_.status -eq 'running'}
	
	gci *.ps1 | Where-Object{$_.Length -gt 50}
	
	Get-Process | where {$_.Company -like 'micro*'} | ft name , description, company
	
## Additional Commands 
### get-unique

	# sort | get-unique *** needs to be sorted first***
	1,1,1,4,6,6,7,2,3,4,5,6,7 | sort | Get-Unique
	
### measure-object

	gci | Measure-Object -Property length
	(gci).count
	gci| where{$_.mode -like 'd'} 
	gci | Measure-Object -Property length -Average -Maximum -Minimum -sum
	
#Compare Object
### Compare-Object

	'test string' > test.txt
	$before = gci
	'42' > test.txt
	$after = gci
	Compare-Object $before $after -Property length, name

## Object Creation
### New-Object
	$mytruck = new-object object 
	$mytruck | Get-Member
 
### Add-Member
#Adding properties using add-member

	Add-Member -MemberType NoteProperty -Name Color -Value Red -InputObject $mytruck
	Add-Member -me NoteProperty -in $mytruck -Na Make -value Ford
### Positional Parameters

	#Short handed positional parameters
		#cmdlet     Parameter variable   MemberType   name   value
		Add-Member -InputObject $mytruck NoteProperty Model "F-150"

#adding properties through a pipeline
	$mytruck | Add-Member NoteProperty Cab SuperCrew


#### Adding Methods
	Add-Member -MemberType ScriptMethod -InputObject $mytruck -name Drive -Value {"Going on a road trip"}
	$mytruck.drive()

##Positional

	Add-Member -InputObject $mytruck ScriptMethod accelerate { "skinny pedal on the right" }

##Pipeline

	$mytruck | Add-Member ScriptMethod park { "finding a spot"}

#### [PScustomobject]
	$soldier = [PScustomobject]@{
        	"FirstName" = "Joe"
        	"LastName" = "Mama"
        	"Rank" = "Recruit"
        	"MOS" = "666"
        }
## PsProvider

	#PsProvider
	Get-PSProvider
	Get-PSDrive
	#Create PSDrive
	New-PSDrive -name HKU -PSProvider Registry -Root HKEY_USERS
	

	"""Remove-PSDrive -Name HKU"""
	Get-PSDrive HKU
	cd HKU:
	
	
## Conditions
### -eq / -ne
	#equals / not equals
	-eq / -ne 
	#case insensitive 
	-ieq 
	#case sensitive 
	-ceq
 	
	4 -eq 4
	# returns true
	
	7, 8, 9 -eq 8
	# returns 8
	
### -match | -like
	#Match 
	-like # used with wildcards
	-match # used with regex
	
### -replace 
	# Replacement 
	-replace # can use regex
### Containement
	#containment
	-contains / -notcontains
	-in / -notin
	
	#type
	-is / -isnot
	
	
	
	
### -gt | -lt
	$num = 5
	(($num -gt 1) -and ($num -lt 10))
	
	(($num -gt 1) -or ($num -lt 4))
	
	$x = 11
	if ($x -gt 10) { "$x is larger than 10"}
	
	$x = 7

## If Statements 

	if ($x -eq 5) {
	    write-host "Condition is true $x is equal to 5" -ForegroundColor Blue
	
	}
	else {
	write-host " condition is false; $x is not equal to 5" -ForegroundColor DarkRed
	}
	
	
	#switch 
	## Needed for exercises
	## Most likely not on test
	
	$val = "meg"
	Switch($val) {
	Peter {"Thats the father"}
	Lois {"Thats the mother"}
	Stewie {"Thats the youngest child"}
	Chris {"Thats the Oldest child"}
	default { "Nobody cares about you $val"}
	}
	
	
## Loops
### ForEach-Object

	#ForEach-Object ### Executes code on each object sent through pipeline
	
	$nums = 1, 2, 3 ,4, 5
	$nums | ForEach-Object{$_ * 2}
	
	$list = 'a', 'b', 'c'
	$list | ForEach-Object{$_.toupper()}
	
	gci | ForEach-Object { $_.length / 1kb }
	
### ForEach

	#ForEach, no pipeline
	#needs to wait for gci results
 
	foreach ($i in gci C:\ -Recurse) {$i.name}
	#Results as it iterates through 
 
	gci C:\ -Recurse | ForEach-Object{$_.name}
	
	foreach($num in 1..5){$num * 2}
	
	
	$teams = "Lions", "Tigers", "Red Wings", "Pistons"
	ForEach($team in $teams){
	  $team.insert(0, "Detroit ")
	}
	
	
## While

	$num = 0
	while ($num -lt 3) {
	    $num
	    $num ++
	    }
	
	$var = ""
	while($var -ne "Marines") {
	    $var = read-host "Which branch of the military is the best?"
	}
	
## Do While 

	## Will run at least once 
	$num = 0
	do {
	    $num
	    $num++
	}while($num -lt 3)
	
## Do Until

	$num = 0
	do {
	    $num
	    $num++
	}until($num -gt 3)
	
	
## For Loops

	for ($num =1; $num -le 3; $num++){$num}
	
	
	for($i = 0; $i -le 255; $i++) {
	    write-host 192.168.0.$i
	}
	
	
	
	
## Reading and writing to a file

	"Hello" > hello.txt
	set-content -Path .\example.txt -Value "A string" # Overwrites file
	Add-Content -Path .\example.txt -Value "another one" # Appends to a file
	Get-Content .\example.txt # Reads file
	cat .\example.txt 
	
	
## PowerShell Scripts
	
	'"Odoyle Rules!"' > 1-script.ps1
	.\1-script.ps1
	
	
	#Here-strings
	
	@'
	"Odoyle rules!"
	"More code"
	gci
	get-service
	'@ > 2-script.ps1
	.\2-script.ps1

 
