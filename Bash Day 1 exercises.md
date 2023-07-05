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
  *** {} is a place holder for input from find ***
## Q 3 : find -empty - printf
  Activity:

Using ONLY the find command, find all empty files/directories in directory /var and print out ONLY the filename (not absolute path), and the inode number, separated by newlines.
Example Output:
inode filename
%i   %f
123 file1
456 file2
789 file3

    find '/var' -empty -printf "%i %f\n"
  *** %i grabs inode number | | %f grages filename ***
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

     egrep -o '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' StoryHiddenIPs | sort | uniq -c | sort -nr

    
    
find '/var' -empty -printf "%i %f\n"

## Q 7
  Activity:

  Using ONLY the awk command, write a BASH one-liner script that extracts ONLY the names of all the system and user accounts that are not UIDs 0-3.
  Only display those that use /bin/bash as their default shell.
  The input file is named $HOME/passwd and is located in the current directory.
  Output the results to a file called $HOME/SED/names.txt

    awk -F: '($3 > 3) && ($NF == "/bin/bash") {print $1}' ~/passwd  > ~/SED/names.txt

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

    # Alternate solution: sort with delimeter of : and fourth column , numerically
    #sort -t: -k4 -n /etc/passwd | head | tail -1 | cut -d: -f6 | md5sum | cut -d' ' -f1
    # sort -t: -k4 -n passwd | awk -F: 'NR==10 {print $6}' | md5sum |  cut -d' ' -f1
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

# Q 16 


Activity:

Design a script that detects the existence of directory: $HOME/.ssh

  Upon successful detection, copies any and all files from within the directory $HOME/.ssh to directory $HOME/SSH and produce no output. You will need to create $HOME/SSH.
  Upon un-successful detection, displays the error message "Run ssh-keygen" to the user.

NOTE: If the $HOME/.ssh directory does not exist, one may run the ssh-keygen command. Accept all defaults for the purposes of this exercise. This is not necessary for passing the activity but can be used for testing on your local machine.

    if [ -d $HOME/.ssh ] ; then
      cp -r $HOME/.ssh $HOME/SSH
    else
      echo "Run ssh-keygen"
    fi

# Q 17 
Activity:

  Write a script that determines your default gateway ip address. Assign that address to a variable using command substitution.

  NOTE: Networking on the CTFd is limited for security reasons. ip route and route are emulated. Use either of those with no switches.

  Have your script determine the absolute path of the ping application. Assign the absolute path to a variable using command substitution.
  Have your script send six ping packets to your default gateway, utilizing the path discovered in the previous step, and assign the response to a variable using command substitution.
  Evaluate the response as being either successful or failure, and print an appropriate message to the screen.
    
    default=$(ip route | cut -d' ' -f3 | head -1)
    
    pingd=$(which ping)
    
    pingresults=$($pingd -c 6  $default | egrep -o "6 received")
    # awk '/loss/{print $6}' prints 6th field if string 'loss' is found in line. 
    # can replace grepping for string. 
    if [[ $pingresults == "6 received" ]] ; then      
      echo 'successful' 
    else 
      echo 'failure' ;
    fi

# Q 18



Activity:

  Create the following files in a new directory you create $HOME/ZIP:
      file1 will contain the md5sum of the text 12345
      file2 will contain the md5sum of the text 6789
      file3 will contain the md5sum of the text abcdef
  Create a zip file containing the three files above, without being stored inside a directory in the zip file. Name the zip file $HOME/ZIP/file.zip
      HINT: "Junk" the paths
  Utilize tar on $HOME/ZIP/file.zip to archive it into a file called $HOME/ZIP/file.tar.gz which should not include directories. Use the gzip option in tar, DO NOT use a seperate gzip binary.
      HINT: You might need an option to change directories first.
    
    mkdir $HOME/ZIP
    cd $HOME/ZIP
    touch file{1..3}
    echo "12345" | md5sum | cut -d' ' -f1 > file1
    echo "6789" | md5sum | cut -d' ' -f1 > file2
    echo "abcdef" | md5sum | cut -d' ' -f1 > file3
    zip -j file.zip file{1..3}
    tar -czvf file.tar.gz file.zip

# Q 19



Activity:

  Design a basic FOR Loop that iteratively alters the file system and user entries in a passwd-like file for new users: LARRY, CURLY, and MOE.
  Each user should have an entry in the $HOME/passwd file
  The userid and groupid will be the same and can be found as the sole contents of a file with the user's name in the $HOME directory (i.e. $HOME/LARRY.txt might contain 123)
  The home directory will be a directory with the user's name located under the $HOME directory (i.e. $HOME/LARRY)
      NOTE: Do NOT use shell expansion when specifying this in the $HOME/passwd file.
  The default shell will be /bin/bash
  The other fields in the new entries should match root's entry
  Users should be created in the order specified

    file="$HOME/passwd" 
    for x in {LARRY,CURLY,MOE} ; do
         ugid=$(cat ~/$x".txt")
         head -1 $file | awk -F: -v "awkname=$x" -v "id=$ugid" '{OFS=":"} {$1=awkname} {$3=id} {$4=id} {$6="$HOME/"awkname} {$NF="/bin/bash"} {print}'  >> $file
    mkdir ~/$x
    done

# Q 20

    

Activity:

Write a bash script that will find all the files in the /etc directory, and obtains the octal permission of those files. The stat command will be useful for this task.
Depending how you go about your script, you may find writing to the local directory useful. What you must seperate out from the initial results are the octal permissions of 0-640 and those equal to and greater than 642, ie 0-640 goes to low.txt, while 642 is sent to high.txt.
Have your script uniquely sort the contents of the two files by count, numerically-reversed, and output the results, with applicable titles, to the screen. An example of the desired output is shown below.
    NOTE: There is a blank line being printed between the two sections of the output below.

EXAMPLE OUTPUT:

Files w/ OCTAL Perm Values 642+:
  424 777
  365 644
   15 755

Files w/ OCTAL Perm Values 0-640:
    4 640
    3 440
    2 600
    1 444

    for x in $(stat -c %a $(find /etc -type f)) ; do
    if [ $x -ge 642 ] ; then 
      echo $x  >> high.txt
    else
      echo $x >> low.txt
    fi  
    
    done
    
    echo -e "Files w/ OCTAL Perm Values 642+:\n$(sort high.txt | uniq -c | sort -nr)\n"
    echo -e "Files w/ OCTAL Perm Values 0-640:\n$(sort low.txt | uniq -c | sort -nr)\n"


