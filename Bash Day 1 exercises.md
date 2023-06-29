# Q 1 Brace-Expansion
  Activity: Using Brace-Expansion, create the following directories within the $HOME directory:

  1123
  1134
  1145
  1156
    
    mkdir $HOME/11{23,34,45,56}
  
# Q 2 
  Copy all files in $HOME/1123 directory that end in .txt and omit andy files containing "~" character, to directory $HOME/CUT
    find ~/1123 -name "*.txt" ! -iname '*~*' -exec cp {} ~/CUT/ \;

# Q 5
  
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
