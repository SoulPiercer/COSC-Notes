# Bash Activities
# Day 1
## Q 1 Brace-Expansion
  Activity: 
  Using Brace-Expansion, create the following directories within the $HOME directory:

  1123
  1134
  1145
  1156
    
    mkdir $HOME/11{23,34,45,56}

## Q 1.2
  Activity:

Use Brace-Expansion to create the following files within the $HOME/1123 directory. You may need to create the $HOME/1123 directory. Make the following files, but utilze Brace Expansion to make all nine files with one touch command.

Files to create:

  1.txt
  2.txt
  3.txt
  4.txt
  5.txt
  6~.txt
  7~.txt
  8~.txt
  9~.txt

    touch ~/1123/{1..5}.txt & touch ~/1123/{6..9}~.txt

## Q 1.3
  Activity: 
  Using the find command, list all files in $HOME/1123 that end in .txt.

    find ~/1123 -name *.txt

## Q 1.3 Challenge

Challenge Activity:

List all files in $HOME/1123 that end in .txt. Omit the files containing a tilde (~) character.

While this activity can be accomplished with only find, it can also be combined with grep as well.

    find ~/1123 -name "*.txt" | grep -v "~.txt"

  
## Q 2 ; find -exec 
Activity:
Copy all files in $HOME/1123 directory that end in .txt and omit andy files containing "~" character, to directory $HOME/CUT
    
    find ~/1123 -name "*.txt" ! -iname '*~*' -exec cp {} ~/CUT/ \;
  # {} is a place holder for input from find
## Q 3
  Activity:

Using ONLY the find command, find all empty files/directories in directory /var and print out ONLY the filename (not absolute path), and the inode number, separated by newlines.
Example Output

123 file1
456 file2
789 file3

    find '/var' -empty -printf "%i %f\n"

## Q 4
Activity:

Using ONLY the find command, find all files on the system with inode 4026532575 and print only the filename to the screen, not the absolute path to the file, separating each filename with a newline. Ensure unneeded output is not visible.

    find -inum 4026532575 -printf"%f\n"    

## Q 5
  Activity:
  Using only the ls -l and cut Commands, write a BASH script that shows all filenames with extensions ie: 1.txt, etc., but no directories, in $HOME/CUT.
  Write those to a text file called names in $HOME/CUT directory.
  Omit the names filename from your output.

    ls -l ~/CUT | cut -d':' -f2 | cut -d' ' -f2 | cut -d'.' -f1- -s > ~/CUT/names

## Q 6
Activity:

Write a basic bash script that greps ONLY the IP addresses in the text file provided (named StoryHiddenIPs in the current directory); sort them uniquely by number of times they appear.

It is not important to have a regular expression that only catches fully valid IP addresses. It is more important that you become familiar with creating and using regular expressions. Below, there are some useful websites that you can use to visually see what your regular expression pattern is matching on.

    grep -E -o '^\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b$' StoryHiddenIPs | sort -n | -uniq -c 

    
    
find '/var' -empty -printf "%i %f\n"

## Q 7
  Activity:

  Using ONLY the awk command, write a BASH one-liner script that extracts ONLY the names of all the system and user accounts that are not UIDs 0-3.
  Only display those that use /bin/bash as their default shell.
  The input file is named $HOME/passwd and is located in the current directory.
  Output the results to a file called $HOME/SED/names.txt

    awk -F: '($3 > 3)' ~/passwd | awk -F: '($NF == "/bin/bash") {print $1}' > ~/SED/names.txt

## Q 8
  Activity:

  Find all dmesg kernel messages that contain CPU or BIOS (uppercase) in the string, but not usable or reserved (case-insensitive)
  Print only the msg itself, omitting the bracketed numerical expressions ie: [1.132775]

    dmesg | egrep "CPU|BIOS" | egrep -i -v "usable|reserved" |cut -d] -f2-
    
## Q 9 
  Activity:

  Write a Bash script using "Command Substitution" to replace all passwords, using openssl, from the file $HOME/PASS/shadow.txt with the MD5 encrypted password: Password1234, with salt: bad4u
  Output of this command should go to the screen/standard output.
  You are not limited to a particular command, however you must use openssl. Type man openssl passwd for more information.

  
    #!/bin/bash
    a=`openssl passwd -1 -salt bad4u Password1234`
    awk -F: -v "awk_var=$a" 'BEGIN {OFS=":"} {$2=awk_var} {print $0}' ~/PASS/shadow.txt


## Q 10
  Activity:

  Using ONLY sed, write all lines from $HOME/passwd into $HOME/PASS/passwd.txt that do not end with either /bin/sh or /bin/false.

    sed -e '/\/bin\/sh/d' -e '/\/bin\/false/d' ~/passwd > ~/PASS/passwd.txt
# Day 2
## Q 11
  Activity:

  Using find, find all files under the $HOME directory with a .bin extension ONLY.
  Once the file(s) and their path(s) have been found, remove the file name from the absolute path output.
  Ensure there is no trailing / at the end of the directory path when outputting to standard output.
  You may need to sort the output depending on the command(s) you use.

    find ~ -iname "*.bin" -printf "%h\n" | sort | uniq

## Q 12
  Activity:

  Write a script which will copy the last entry/line in the passwd-like file specified by the $1 positional parameter
  Modify the copied line to change:
      User name to the value specified by $2 positional parameter
      Used id and group id to the value specified by $3 positional parameter
      Home directory to a directory matching the user name specified by $2 positional parameter under the /home directory
      The default shell to `/bin/bash'
  Append the modified line to the end of the file

    #!/bin/bash 
    tail -1 $1 | awk -F: -v "awk_a=$1" -v "awk_b=$2" -v "awk_c=$3" '{OFS=":"} {$1=awk_b} {$3=awk_c} {$4=awk_c} {$6="/home/"awk_b} {$NF="/bin/bash"} {print $0}' >> $1
    
##  Q 13 
  Actvity:
  
  Find all executable files under the following four directories:
      /bin
      /sbin
      /usr/bin
      /usr/sbin
  Sort the filenames with absolute path, and get the md5sum of the 10th file from the top of the list.

    md5sum $(find /bin /sbin /usr/bin /usr/sbin -type f -executable | sort | head | tail -1) | cut -d' ' -f1

## Q 14
  Activity:

Using any BASH command complete the following:

  Sort the /etc/passwd file numerically by the GID field.
  For the 10th entry in the sorted passwd file, get an md5 hash of that entry’s home directory.
  Output ONLY the MD5 hash of the directory's name to standard output.

    a=$(awk -F: '{OFS=":"} {print $4}' /etc/passwd | sort -n | head | tail -1)
    grep $a /etc/passwd | awk -F: '{OFS=":"} {print $6}' | md5sum | cut -d' ' -f1    

## Q 15 
  

Activity:

  Write a script which will find and hash the contents 3 levels deep from each of these directories: /bin /etc /var
  Your script should:
      Exclude named pipes. These can break your script.
      Redirect STDOUT and STDERR to separate files.
      Determine the count of files hashed in the file with hashes.
      Determine the count of unsuccessfully hashed directories.
      Have both counts output to the screen with an appropriate title for each count.

Example Output:

Successfully Hashed Files: 105
Unsuccessfully Hashed Directories: 23

    #!/bin/bash
    touch ~/out
    chmod 777 ~/out
    touch ~/err
    chmod 777 ~/err
    a=~/out
    b=~/err
    md5sum $(find /bin /etc /var -maxdepth 3) 2>>$b 1>>$a  
    
    successful=$(cat $a | wc -l)
    unsuccessful=$(egrep -i "is a directory" $b | wc -l)
    echo "Successfully Hashed Files: $successful"
    echo "Unsuccessfully Hashed Directories: $unsuccessful"    
