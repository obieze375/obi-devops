[[Index]] 


  
  

## pwd command

```bash
pwd : prints current working directory


Example:

paul@debian8:~$ pwd

/home/paul

```

  

## cd

  

• cd -> changes your current directory + also return to your home dir

```bash

Example:

paul@debian8$ cd /etc

paul@debian8$ pwd

/etc

```

  
  

## cd – command continued

```bash

paul@debian8$ cd

paul@debian8$ pwd

/home/paul

```

  

## cd .. – Return to previous directory

```bash

paul@debian8$ pwd

/usr/share/games

paul@debian8$ cd ..

paul@debian8$ pwd  

/usr/share

```

  

## Path Completion

  
```bash
The tab key can help you in typing a path without errors. Typing cd /et

followed by the tab key will expand the command line to cd /etc/.

When typing cd /Et followed by the tab key, nothing will happen

because you typed the wrong path (upper case E).

  

You will need fewer key strokes when using the tab key, and you will

be sure your typed path is correct!
```  

## ls – command

  

## ls – lists contents of directory

  

```bash

Example:

stuff summer.txt

paul@debian8:~$ ls

allfiles.txt dmesg.txt services

paul@debian8:~

```

  

## ls –a - Shows all files including hidden ones

  

```bash

allfiles.txt .bash_profile dmesg.txt .lesshst stuff .. .bash_history

.bashrc services .ssh summer.txt

• Example:

• paul@debian8:~$ ls -a

paul@debian8:~$

```

  

## ls -hal

  

## Shows all files with read and write permissions

```bash
ls –ltr
```

  

## ll – Lists contents of directory and gives date

```bash

obi@eze:~$ ll

total 189456

drwxr-xr-x 21 obi  obi       4096 Jan 25 18:30 ./

drwxr-xr-x  3 root root      4096 Jan 10 15:03 ../

-rw-r--r--  1 obi  obi        220 Jan 10 15:03 .bash_logout

-rw-r--r--  1 obi  obi       3771 Jan 10 15:03 .bashrc

drwx------ 15 obi  obi       4096 Jan 10 15:57 .cache/

-rwxr-xr-x  1 obi  obi         42 Jan 10 17:40 command_sub.sh*

-rwxr-xr-x  1 obi  obi         42 Jan 10 17:41 command_substitution.sh*

drwxr-xr-x 17 obi  obi       4096 Jan 10 18:28 .config/

```

  

## ls -R - List Ditrectory and subdirectory structure

  

```bash
ls -R
```

  
  
  

## Search for files in Directories

```bash

 ll <*STRING_TO_SEARCH*>

```

## Locate a file

```bash
locate passwd
```

## Working with files

  

```bash
file – determines file type of file

Syntax: 

file <filename>
```

## Check File Type  

```bash

paul@laika:~$ file pic33.png

pic33.png: PNG image data, 3840 x 1200, 8-bit/color RGBA, noninterlaced

paul@laika:~$ file /etc/passwd

/etc/passwd: ASCII text

paul@laika:~$ file HelloWorld.c

HelloWorld.c: ASCII C program text

```

  

## touch – creates empty files

  

```bash

paul@debian7:~$ ls -l total 0

paul@debian7:~$ touch file42

paul@debian7:~$ touch file33

paul@debian7:~$ ls –l

total 0

-rw-r--r-- 1 paul paul 0 Oct 15 08:57 file33

-rw-r--r-- 1 paul paul 0 Oct 15 08:56 file42

```

## touch - Create Multiple files

```bash 

touch file {1,2,3}


```
  

## File Creation Commands

```bash

vim

  

nano

  

cat


The Ctrl d key combination will send an EOF (End of File) to the running

process ending the cat command
  
``` 


## File Compression
 
 gzip -

  

# -f option :

  

Sometimes a file cannot be compressed. Perhaps you are

trying to compress a file called “myfile1” but there is already a file called

“myfile1.gz”. In this instance, the “gzip” command won’t ordinarily work.

  

To force the “gzip” command to do its stuff simply use -f option:

  

```bash

  2.$ gzip -f myfile1.txt

```

  



  

The above command would end up with a file called “mydoc.txt.gz” and



  

```bash

  “mydoc.txt”.

```

  

## 3. -L option : This option displays the gzip license.



```bash

  
  $ gzip -L filename.gz

```

  

## Unzip files

  

1) Extract a tar.gz archive

  

The following command shell helps to extract tar files out a tar.gz archive.


```bash

tar -xvzf bigfile.tar.gz

  

x – Extract files

v – Verbose, print the file names as they are extracted one by one

z – The file is a “gzipped” file

f – Use the following tar archive for the operation!
  

```

  

2) Extract files to a specific directory or path

  

We can extract the files into a specified directory by using the parameter “-C”.

  



The tar command doesn’t create any files or folders.


```bash

  
  tar -xvzf bigfile.tar.gz -C /folder/subfolder/

```

  

## Extract a single file

  

To extract a single file out of an archive just add the file name after the command like this

```bash
tar -xz -f Music.tar.gz “./new/one.mp3”
```


To extract more than one file out of an archive


```bash
tar -xv -f Music.tar.gz “./new/two.mp3” “./new/three.mp3”
```

  

## Use unzip command to unzip a zip file

The syntax is:

  
```bash

unzip {file.zip}

```
  

Use the following syntax if you want to extract/unzip to a particular destination directory:

  
  
```bash

unzip -d /dest/directory/ {file.zip}
```

  
  



  
  

## Check contents of File


```bash

cat /etc/passwd

root:x:0:0:root:/root:/bin/bash

bin:x:1:1:bin:/bin:/sbin/nologin

narad:x:500:500::/home/narad:/bin/bash

```

  

## Check difference between 2 files

```bash
diff filename1 filename2
```

  

## rm – Deletes file forever

```bash

paul@debian7:~$ ls

BigBattle.txt file33 file42 SinkoDeMayo

paul@debian7:~$ rm BigBattle.txt

paul@debian7:~$ ls

file33 file42 SinkoDeMayo

paul@debian7:~

```

  

## rm –i – Deleting file with a check

  

```bash
paul@debian7:~$ rm -i file33

rm: remove regular empty file `file33'? Yes
```


  


## rm-f – Delete files

```bash
 • VERY STRONG WAY TO DELETE FILES:

rm -f /* - Deletes all files in directory
```
  

  

## cp – Copy One file

  

```bash
paul@debian7:~$ ls

file42 SinkoDeMayo

paul@debian7:~$ cp file42 file42.copy

paul@debian7:~$ ls

file42 file42.copy SinkoDeMayo
```

  

## Copy to another Directory

  

```bash
paul@debian7:~$ mkdir dir42

paul@debian7:~$ cp SinkoDeMayo dir42

paul@debian7:~$ ls dir42/

SinkoDeMay
```

 

  

## cp –r – Copy complete Directories

```bash
paul@debian7:~$ ls

dir42 file42 file42.copy SinkoDeMayo

paul@debian7:~$ cp -r dir42/ dir33

paul@debian7:~$ ls

dir33 dir42 file42 file42.copy SinkoDeMayo

paul@debian7:~$ ls dir33/

SinkoDeMayo
```
 


## copy multiple files to directory

```bash
paul@debian7:~$ cp file42 file42.copy SinkoDeMayo dir42/

paul@debian7:~$ ls

dir42/ file42 file42.copy SinkoDeMay

cp – i – To prevent overwriting of files

paul@debian7:~$ cp -i SinkoDeMayo file42

cp: overwrite `file42’? N

paul@debian7:~$  
```

  

## More Copy notes

```bash
cp filename/directory - copy file and directory

cp filename directory/directory - copy and move file

cp filename directory/directory/filename.bak - creates backup file
```


 

  

## mv - Rename files with mv

```bash

paul@debian7:~$ mv file42 file33

paul@debian7:~$ ls

dir33 dir42 file33 file42.copy SinkoDeMayo

paul@debian7:~$

```

  
  

## Running bash script

```bash

paul@debian7~$

./ - to run bash script

```

  

## figlet

  

```bash

• sudo apt-get install figlet

```

  

## mkdir – Makes new directory

  

## Creates Directory

  

```bash

paul@debian8:~/mydir$ mkdir otherstuff

paul@debian8:~/mydir$ ls -l

total 8 drwxr-xr-x 2 paul paul 4096 Sep 17 00:08 otherstuff

drwxr-xr-x 2 paul paul 4096 Sep 17 00:08 stuff

```

  

## mkdir –p – Creates Parent with new directory

  

```bash
paul@debian8:~$ mkdir -p mydir2/mysubdir2/threedirsdeep

paul@debian8:~$ cd mydir2

paul@debian8:~/mydir2$ ls -l

total 4 drwxr-xr-x 3 paul paul 4096 Sep 17 00:11 mysubdir2

paul@debian8:~/mydir2$ cd mysubdir2

paul@debian8:~/mydir2/mysubdir2$
```

  

## Search through Directory -  

  

```bash
sudo du -a /dir/ | sort -n -r | head -n 20
```


## rmdir – Deletes Directory

```bash
paul@debian8:~/mydir$ rmdir otherstuff

paul@debian8:~/mydir$ cd ..
```  


  


## rmdir –p – Deletes Directory

  

```bash  

paul@debian8:~$ rmdir -p test42/subdir

@debian8:~

```

  

## move file to another location

  

```bash

mv directory directory - moves file to another location

```

  

## cat - Check contents of file

  

```bash

cat /etc/hosts 

cat /etc/hosts | grep <string_your're_looking_for>  (To search for a value through a file)
  
```
  

## Pattern searching

```bash
grep -v pattern file.txt
```

## Search for string and line number in file

```bash

grep -inr 'string' filename/dir

```

## Find command

  

## Find Files Using Name in Current Directory

```bash
  

find . -name tecmint.txt

./tecmint.txt

  

```

  

## Find Files Under Home Directory

```bash

find /home -name tecmint.txt

/home/tecmint.txt!

```

  
  

## Editing Files with Vi

  

## Vi Command Mode and Navigation

  
```bash

k  - Up one line.  

j -  Down one line.    

h  - Left one character.      

l  - Right one character.  

w  - Right one word.

b  - Left one word.

^  - Go to the beginning of the line.

$  - Go to the end of the line.

```

    


  

## Vi Insert Mode


```bash
i Insert at the cursor position.

I Insert at the beginning of the line.  

a Append after the cursor position.  

A Append at the end of the line.

```  


## Vi Line Mode


```bash

:w Writes (saves) the file.    

:w! Forces the file to be saved.  

:q Quit.  

:q! Quit without saving changes.  

:wq! Write and quit.  

:x Same as :wq.

```  

## Vi Line Mode


```bash
:n Positions the cursor at line n.

:$ Positions the cursor on the last line.

:set nu Turn on line numbering.

:set nonu Turn off line numbering.

:help [subcommand] Get help.

```  



## Vi Modes



```bash

Mode Key

Command Esc

Insert i I a A
Line :

```


  

## Vi - Repeating Commands

● Repeat a command by preceding it with a

number.

```bash

○ 5k = Move up a line 5 times

○ 80i = Insert 80 times

○ 80i_ = Insert 80 "_" characters

```  

## Vi - Deleting Text



```bash
x Delete a character.

dw Delete a word.

dd Delete a line.  

D Delete from the current position.

```






## Vi - Changing Text


```bash

r Replace the current character.  

cw Change the current word.      

cc Change the current line.

c$ Change the text from the current position.  

C Same as c$.  

~ Reverses the case of a character.


```




## Vi - Copying and Pasting

```bash
yy Yank (copy) the current line.  

y Yank the .  

p Paste the most recent deleted or yanked text.

To set paste if having formatting issues - (:set paste + shift I ) and then paste

4yy - Pastes 4 lines down page

```



## Vi - Undo / Redo  

```bash

u - Undo

```

```bash

Ctrl-R - Redo

```


## Vi - Searching  

```bash
/ - Start a forward search.  

? - Start a reverse search.
```




  

## Vi - Recover swap file  

```bash

vim -r

```
  
  

## Demo - vi

  

```bash
---TODO: Overlay with characters that I'm

using.---- (Might be hard, maybe come back and

do that later????)

key-mon --nomouse --scale=2.1
```bash

## Substitution Command

```bash

:%s/NEW VALUE TO INPUT/g  

:%s/NEW VALUE TO INPUT/gic

```  

  

## Substitution Command II

```bash

:s/first value at beginning of line/

```


  
  

## Mass Delete and move to the bottom of file

```bash

dG

```  



  

## Move to the bottom of the file:  

```bash

Shift + g

```



## To reclaim previous command used

```bash

press shift key + ‘:’ and press the up arrow key

```


## Set Numbering:

```bash

set nu

```

## Check for tabs:

```bash

:set list - for yamls there should be no tabs or whitespaces between $ signs

```

  

## Remove tab checker:

```bash

:set nolist

```   

## Go to line:

  
```bash
:644
```

## Create new Blank line

  
```bash

'o'

```


  

## Undo changes

```bash

u

```  


  
  

## Paste from particular line number to another line number

```bash

:6,12 co .

```
6 is the 1st line, 12 is the endding line and place the cursor at the location you want to paste

## Vim Lab 

Enter the line-based visual mode of Vim. Your screen output may be different than these
examples. Remove the first three lines of the editing_final_lab.txt file. 

Use the arrow keys to position the cursor at the first character in the first line. Enter the linebased visual mode with Shift+V. Move down by using the down arrow key twice to select
the first three lines. Delete the lines by typing x.

![](assets/vim1.png)

Enter the visual mode of Vim. Remove the last seven characters from the first column on the
first line. Preserve only the first four characters of the first column.

Use the arrow keys to position the cursor at the fifth character of the first column on the first
line. Enter the visual mode by typing v.
Use the arrow keys to position the cursor at the last character of the first column on the first
line. Delete the selection by typing x.

![](assets/vim2.png)

Enter the visual block mode of Vim. Repeat the operation of the previous step, but this time
select from the second to the last line. Preserve only the first four characters of the first
column.

Use the arrow keys to position the cursor at the fifth character of the second line. Enter the
visual mode by using the control sequence Ctrl+V. Use the arrow keys to position the cursor
at the last character of the first column on the last line. Delete the selection by typing x.

![](assets/vim3.png)

## Enter the visual block mode of Vim and remove the fourth column of the file.

Use the arrow keys to position the cursor at the first character of the fourth column. Enter
the visual block mode by using Ctrl+V. Use the arrow keys to position the cursor at the last
character and row of the fourth column. Delete the selection by typing x.

![](assets/vim4.png)


## Enter the visual block mode of Vim to remove the time column, leaving the month and day columns on all lines.

Use the arrow keys to position the cursor at the first character of the current seventh column.
Enter the visual block mode by typing Ctrl+V. Use the arrow keys to position the cursor at
the last character of the seventh column on the last row. Delete the selection by typing x. 

![](assets/vim5.png)


## Enter the visual line mode of Vim and remove the rows that contain the Desktop and Public strings.

Use the arrow keys to position the cursor at any character on the Desktop row. Enter visual
mode with uppercase V. The full line is selected. Delete the selection by typing x. Repeat the
operation for the row with the Public string.

![](assets/vim6.png)

## Save your changes and exit the file.

To save and exit the file, enter the last-line :wq command.

![](assets/vim7.png)

## Back up the editing_final_lab.txt file and append the date (in seconds) at the end of the file name preceded with an underscore (_) character. Use the lab_file shell variable.

Use the cp command to back up the editing_final_lab.txt file. Use the $(date +
%s) command at the end of the backup name preceded with an underscore (_) character to
make the name unique. 

```bash

[student@workstation ~]$ cp $lab_file \
editing_final_lab_$(date +%s).txt


```

## Append a dashed line to the editing_final_lab.txt file. The dashed line should contain 12 dash (-) characters for this lab to be graded correctly. Use the lab_file shell variable.

Use the echo command with 12 dashes and append the output to the
editing_final_lab.txt file. 

```bash
[student@workstation ~]$ echo "------------" >> $lab_file


```


List the content of the Document directory and redirect the output to the
editing_final_lab.txt file. Use the lab_file shell variable.

Use the ls command to list the Document directory and pipe the output to the tee -a
command to append the output to the editing_final_lab.txt file.
[student@workstation ~]$ ls Documents/ | tee -a $lab_file
lab_review.txt



## Confirm that the directory listing is at the bottom of the lab file. Use the lab_file shell variable.

```bash
[student@workstation ~]$ cat $lab_file
drwx 3 student 17 Mar 4 .ansible
-rw- 1 student 18 Nov 5 .bash_logout
-rw- 1 student 141 Nov 5 .bash_profile
-rw- 1 student 492 Nov 5 .bashrc
drwx 9 student 4096 Mar 8 .cache
drwx 8 student 4096 Mar 8 .config
drwx 2 student 6 Mar 8 Documents
drwx 2 student 6 Mar 8 Downloads
-rw- 1 student 0 Mar 8 editing_final_lab.txt
drwx 2 student 25 Mar 4 .grading
drwx 4 student 32 Mar 8 .local
drwx 2 student 6 Mar 8 Music
drwx 2 student 6 Mar 8 Pictures
drwx 2 student 77 Mar 4 .ssh
drwx 2 student 6 Mar 8 Templates
drwx 3 student 18 Mar 4 .venv
drwx 2 student 6 Mar 8 Videos
------------
lab_review.txt


```




[[Index]] 



