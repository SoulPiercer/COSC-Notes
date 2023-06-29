# Bash Notes

## https://cted.cybbh.io/tech-college/pns/public/pns/latest/guides/bash_sg.html

## CTF IP from vpn: http://10.50.34.146/
## Test on Wednesday 7/5/2023

## 

## Day 1
*** Get slack login*** 
Not object oriented
    only data type is strings
    File extensions are meaningless
    
    ! is a reference to history
    !! runs last command in history
    !(number) runs command(number) in history

Ctrl + arrow will move through words, alt + f or alt + b navigates words 
Ctrl + r runs search through history
Ctrl + l clears screen

# Linux Commands
### touch
        creates file
#### options
    -t ; changes timestamp

## curl ; client url request
    access a webpage in shell
### curl cht.sh/(command)
    cheat sheet for commands
### mkdir
### rm ; remove a file
    rm -r remove files recursively
### rmdir; delete a directory
### ls ; list files
    ls -l ; list permissions
### echo ; returns to screen
### cat
### head
### tail
### more / less
### locate
     searches database for linux native commands ie: # locate find
### wheris
    searches for binary files, executables, and config files AND man
    page page directories [ /usr/share/man/* ];
    $whereis man ; $manpath
    does not search user directories: /home /* OR various other locations easily
    searched by `find' AND `locate' ie: #
### find
    -type ; specifies type of file
    -exec ; executes a command on whats found
        find /var/log/ -iname *.log -exec ls -al {} 2>/dev/null \;
            curly brackets are used to reverence results of find 
            \; finishes line
    -name ; searches rom pwd recursively fo files
	-path		searches in designated path
### grep 
    looks for patterns
    grep [pattern][file]
    grep -v invers match
### egrep ; extended grep
    cat /etc/passwd | egrep "root|student" 
        finds root or sutdent in /etc/passwd
### ps
    ps -elf 
    -- forest ; shows parent processes
### kill  / killall
    kill -9 {pid}
    killall ; kills all of a proccess
### cut
	cut -d: -f1         displays only portion of line before 1st instance of delimiter ``:''
	cut -d: -f1-        " and any following strings up to the very next instance ``:''
	cut -d: -f1- -s     " " but don’t print any lines not containing delimiter ``:''
	cut -f3             displays only the 3rd field delimited by space
	cut -f2-4           displays only fields 2 through 4 delimited by space
	cut -c3-10          displays only the 3rd through 10th characters of each line
### awk
    awk -F: '{print $1}'         displays 1st field delimited by a ":"
    awk '{print $2}'             displays 2nd field, delimited automatically by space
    awk '{print $0}'             displays all string data that matches
    awk -F: '($3 == 0) {print $1}' /etc/passwd
                            displays 1st field (username) IF the 3rd field (UID) is equal to "0"
    awk -F: '{OFS='-'} {print $0}
                                Displays all string data replacing delimter
    awk '{print $NF}'           displays only the last field of every line

### sort    

### uniq
    sorts content uniqely 
    run sort before uniq 
    cat sort.txt | sort | uniq -c | sort -nr   : counts instances of each item sorted in reverse based on number of cocurences

### sed 
	sed 's/regexp/replacement/'
 	sed '/\/bin\/bash/d' /etc/passwd # careful editing this file. ; deletes
	sed -i -e 's/ANCHOVIES/SAUSAGE/g' pizaster.htm
             # replaces every instance of "ANCHOVIES" with "SAUSAGE" on pizaster.htm
	swapping text in file
 	-e adds expression
  	can use regex
   	/d deletes line with pattern
    	-i permenantly changes file
     	have to escape characters i delimeter is in patterhn

      
    	Using ONLY sed, write all lines from $HOME/passwd into $HOME/PASS/passwd.txt that do not end 	with either /bin/sh or /bin/false.

	TIP: When designating a path in a sed command, you must escape the path characters if they 	are to be interpreted as part of the string

	sed '/\/bin/d' file.txt

      	ex: sed -e '/\/bin\/sh/d' -e '/\/bin\/false/d' ~/passwd > ~/PASS/passwd.txt
       
    
## Variables
    $ references variables
## Redirectors
    > redirect st out 
    >> appned
    2> ; errors
        2> /dev/null


## brace expansion {}
    touch file{a,b,c}.txt ; crreates 3 files
    touch file{1..10}.txt ; creates 10 different files
    rm file{a,b,c}
    

## Math:
    Done inside of  double parenthesis (())

# Scripting:
#!/bin/bash
if [[c condition }}; then
    # do these commands
elif [[ condition]]; then
    # do these
else
    #then run these
fi      

  1 #!/bin/bash
  2 
  3 if [[ -f /etc/passwd ]]; then
  4     echo "/etc/passwd file exists"
  5 elif [[ ! -f /etc/passwd ]]; then
  6     echo "/etc/passwd ile doesnt exists"
  7 else
  8     echo "something went wrong :("
  9 fi  
# Day 2

## Special Parameters
	
	$* ; The positional parameters, starting from one.
	
	$@ ; The positional parameters, starting from one.
		
	$# ; The number of positional parameters.
	
	$? ; The exit status of the previous command.
	
	$- ; The current option flags of the shell.
		
	$$ ; The process ID of the shell.
		
	$! ; The process ID of the most recent background job.
		
	$0 ; is set to the name of the script.
	
	$_ ; First command in a script, is the path/name of the script as invoked.
		Otherwise, it is the last parameter passed to the most recent command.
		
	
## Functions 
	function MyFunction { 
 		echo " this is MyFunction"
   	}
	myfunc() {
 		echo "myfunc"
   	}

     	function AFunc() {
      		echo "Another Function"
	}
 	MyFunction
  	myfunc
   	AFunc
    
	read variable ; takes input from a user and assigns it to a variable

#!/bin/bash

function getuserchoice() {
	echo "Make a choice [1,2,3]"
 	readh userchoice
  	case in $userchoice in 
   		(fart) echo "fart" ;;
		(poopr) echo "poop" ;;
  	esac
}
getuserchoice

 1 #!/bin/bash
  2 A=$1
  3 echo The story  of Robert the $A
  4 echo The $A was at the $A store.
  5 



' ' takes things literal
