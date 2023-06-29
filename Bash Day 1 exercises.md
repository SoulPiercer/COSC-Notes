# Bash Activities
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

  
## Q 2 
Activity:
Copy all files in $HOME/1123 directory that end in .txt and omit andy files containing "~" character, to directory $HOME/CUT
    
    find ~/1123 -name "*.txt" ! -iname '*~*' -exec cp {} ~/CUT/ \;

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

# Q 6
    grep -E -o '^\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b$' StoryHiddenIPs | sort -n | -uniq -c 

    
    
find '/var' -empty -printf "%i %f\n"

# Q 9 

    #!/bin/bash
    a=`openssl passwd -1 -salt bad4u Password1234`
    awk -F: -v "awk_var=$a" 'BEGIN {OFS=":"} {$2=awk_var} {print $0}' ~/PASS/shadow.txt


print file name with inode # 999

    find -inum 999 -printf '%f\n' 2> /dev/null

# Q 13 

  Find all executable files under the following four directories:
      /bin
      /sbin
      /usr/bin
      /usr/sbin
  Sort the filenames with absolute path, and get the md5sum of the 10th file from the top of the list.

    md5sum $(find /bin /sbin /usr/bin /usr/sbin -type f -executable | sort | head | tail -1) | cut -d' ' -f1