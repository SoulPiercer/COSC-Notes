
# Q 2 
  Copy all files in $HOME/1123 directory that end in .txt and omit andy files containing "~" character, to directory $HOME/CUT
    find ~/1123 -name "*.txt" ! -iname '*~*' -exec cp {} ~/CUT/ \;

# Q 5
  
  Using only the ls -l and cut Commands, write a BASH script that shows all filenames with extensions ie: 1.txt, etc., but no directories, in $HOME/CUT.
  Write those to a text file called names in $HOME/CUT directory.
  Omit the names filename from your output.

    ls -l ~/CUT | cut -d':' -f2 | cut -d' ' -f2 | cut -d'.' -f1- -s > ~/CUT/names
    
find '/var' -empty -printf "%i %f\n"

# Q 9 

    #!/bin/bash
    a=`openssl passwd -1 -salt bad4u Password1234`
    awk -F: -v "awk_var=$a" 'BEGIN {OFS=":"} {$2=awk_var} {print $0}' ~/PASS/shadow.txt


print file name with inode # 999
find -inum 999 -printf '%f\n' 2> /dev/null

grep -E -o '^\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b$' StoryHiddenIPs | sort -n | -uniq -c 
