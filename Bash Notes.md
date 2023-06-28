# Bash Notes

## https://cted.cybbh.io/tech-college/pns/public/pns/latest/guides/bash_sg.html

## Test on Wednesday 7/5/2023

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
                              



        
