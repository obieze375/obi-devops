
[[Index]] 

## DEBUGGING SHELL PROGRAMS

------------------------------------------------------------------------------

  
  ~~~~

  • bash -n scriptname  # don't run commands; check for syntax errors only

  • set -o noexec       # alternative (set option in script)

  

  • bash -v scriptname  # echo commands before running them

  • set -o verbose      # alternative (set option in script)

  

  • bash -x scriptname  # echo commands after command-line processing

  • set -o xtrace       # alternative (set option in script)

  

  • trap 'echo $varname' EXIT  # useful when you want to print out the values of variables at the point that your script exits

  

  • function errtrap {

    es=$?

    echo "ERROR line $1: Command exited with status $es."

    }

  

  • trap 'errtrap $LINENO' ERR  # is run whenever a command in the surrounding script or function exits with non-zero status

  

  • function dbgtrap {

    echo "badvar is $badvar"

  }

  

  • trap dbgtrap DEBUG  # causes the trap code to be executed before every statement in a function or script

  • # ...section of code in which the problem occurs...

  • trap - DEBUG  # turn off the DEBUG trap

  

  • function returntrap {

     echo "A return occurred"

     }

  

  • trap returntrap RETURN  # is executed each time a shell function or a script executed with the . or source commands finishes executing

  • ./

  
  

  • -a file                   # file exists or its compilation is successful

  • -d file                   # file exists and is a directory

  • -e file                   # file exists; same -a

  • -f file                   # file exists and is a regular file (i.e., not a directory or other special type of file)

  • -r file                   # you have read permission

  • -s file                   # file exists and is not empty

  • -w file                   # your have write permission

  • -x file                   # you have execute permission on file, or directory search permission if it is a directory

  • -N file                   # file was modified since it was last read

  • -O file                   # you own file

  • -G file                   # file's group ID matches yours (or one of yours, if you are in multiple groups)

  • file1 -nt file2           # file1 is newer than file2

    file1 -ot file2           # file1 is older than file2

  ~~~~
  

# For Loops

~~~~

for x in {1..10}

do

  statements

done

  

~~~~

  
  

~~~~

for name [in list]

do

  statements that can use $name

done

~~~~

  

~~~~

for (( initialisation ; ending condition ; update ))

do

  statements...

done

~~~~

  

# Functions

  

The function refers to passed arguments by position (as if they were positional parameters), that is, $1, $2, and so forth.

$@ is equal to "$1" "$2"... "$N", where N is the number of positional parameters. $# holds the number of positional parameters.

  

~~~~

function functname() {

  shell commands

}

~~~~

  

Example:

~~~~

!/bin/bash

  

Basic function

print_something () {

  echo Hello I am a function

   }

  print_something

  print_something

  

~~~~

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-functions.php>

  
  
  
  
  

  • unset -f functname  # deletes a function definition

  • declare -f          # displays all defined functions in your login session

  
  

~~~~

#!/bin/bash

# Passing arguments to a function

print_something () {

echo Hello $1

}

print_something Mars

print_something Jupiter

~~~~

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-functions.php>

  

~~~~

#!/bin/bash

# Setting a return status for a function

print_something () {

echo Hello $1

return 5

}

print_something Mars

print_something Jupiter

echo The previous function has a return value of $?

~~~~

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-functions.php>

  
  

~~~~

#!/bin/bash

# Experimenting with variable scope

var_change () {

local var1='local 1'

echo Inside function: var1 is $var1 : var2 is $var2

var1='changed again'

var2='2 changed again'

}

  

var1='global 1'

var2='global 2'

  

echo Before function call: var1 is $var1 : var2 is $var2

  

var_change

  

echo After function call: var1 is $var1 : var2 is $var2

~~~~

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-functions.php>  

  
  
  

# If Statement

  

~~~~

if condition

then

  statements

[elif condition

  then statements...]

[else

  statements]

fi

~~~~

~~~~

#!/bin/bash

# else example

if [ $# -eq 1 ]

then

nl $1

else

nl /dev/stdin

fi

~~~~

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>

  

# or example

~~~~

#!/bin/bash

if [ $USER == 'bob' ] || [ $USER == 'andy' ]

then

ls -alh

else

ls

fi

~~~~

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>

  

# Basic if statement

~~~~

#!/bin/bash

if [ $1 -gt 100 ]

then

echo Hey that\'s a large number.

pwd

fi

date

~~~~

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>

  

# elif statements

~~~~

#!/bin/bash

if [ $1 -ge 18 ]

then

echo You may go to the party.

elif [ $2 == 'yes' ]

then

echo You may go to the party but be back before midnight.

else

echo You may not go to the party.

fi

~~~~

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>

  

# Nested if statements

~~~~

#!/bin/bash

if [ $1 -gt 100 ]

then

echo Hey that\'s a large number.

if (( $1 % 2 == 0 ))

then

echo And is also an even number.

fi

fi

~~~~

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>

  

# and example

~~~~

#!/bin/bash

if [ -r $1 ] && [ -s $1 ]

then

echo This file is useful.

fi

~~~~

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>  

  
  
  

# INPUT/OUTPUT REDIRECTORS

  
  
  
  ~~~~

  • cmd1|cmd2  # pipe; takes standard output of cmd1 as standard input to cmd2

  • < file     # takes standard input from file

  • > file     # directs standard output to file

  • >> file    # directs standard output to file; append to file if it already exists

  • >|file     # forces standard output to file even if noclobber is set

  • n>|file    # forces output to file from file descriptor n even if noclobber is set

  • <> file    # uses file as both standard input and standard output

  • n<>file    # uses file as both input and output for file descriptor n

  • n>file     # directs file descriptor n to file

  • n<file     # takes file descriptor n from file

  • n>>file    # directs file description n to file; append to file if it already exists

  • n>&        # duplicates standard output to file descriptor n

  • n<&        # duplicates standard input from file descriptor n

  • n>&m       # file descriptor n is made to be a copy of the output file descriptor

  • n<&m       # file descriptor n is made to be a copy of the input file descriptor

  • &>file     # directs standard output and standard error to file

  • <&-        # closes the standard input

  • >&-        # closes the standard output

  • n>&-       # closes the ouput from file descriptor n

  • n<&-       # closes the input from file descripor n

~~~~  
  

# Operators

  
  
  
  ~~~~

  • -lt                       # less than

  • -le                       # less than or equal

  • -eq                       # equal

  • -ge                       # greater than or equal

  • -gt                       # greater than

  • -ne                       # not equal

  

  • statement1 && statement2  # and operator

  • statement1 || statement2  # or operator

  

  • -a               # and operator inside a test conditional expression

        -o                 # or operator inside a test conditional expression

  
~~~~  
  

# Process Handling

  
  

## To suspend a job, type CTRL+Z while it is running. You can also suspend a job with CTRL+Y.

  

##  This is slightly different from CTRL+Z in that the process is only stopped when it attempts to read input from terminal.

## Of course, to interrupt a job, type CTRL+C.

  
~~~~

  • myCommand &  # runs job in the background and prompts back the shell

  

  • jobs         # lists all jobs (use with -l to see associated PID)

  

  • fg           # brings a background job into the foreground

  • fg %+        # brings most recently invoked background job

  • fg %-        # brings second most recently invoked background job

  • fg %N        # brings job number N

  • fg %string   # brings job whose command begins with string

  • fg %?string  # brings job whose command contains string

  

  • kill -l               # returns a list of all signals on the system, by name and number

  • kill PID              # terminates process with specified PID

  • kill -s SIGKILL 4500  # sends a signal to force or terminate the process

  • kill -15 913          # Ending PID 913 process with signal 15 (TERM)

  • kill %1               # Where %1 is the number of job as read from 'jobs' command.

  

ps           # prints a line of information about the current running login shell and any processes running under it

ps -a        # selects all processes with a tty except session leaders

  

trap cmd sig1 sig2  # executes a command when a signal is received by the script

trap "" sig1 sig2   # ignores that signals

trap - sig1 sig2    # resets the action taken when the signal is received to the default

  

disown <PID|JID>    # removes the process from the list of jobs

  

wait                # waits until all background jobs have finished

~~~~
  
  
  

# Strings

~~~~
  
  

  • str1 == str2               # str1 matches str2

  • str1 != str2               # str1 does not match str2

  • str1 < str2                # str1 is less than str2 (alphabetically)

  • str1 > str2                # str1 is greater than str2 (alphabetically)

  • str1 \> str2               # str1 is sorted after str2

  • str1 \< str2               # str1 is sorted before str2

  • -n str1                    # str1 is not null (has length greater than 0)

        * -z str1                    # str1 is null (has length 0)

~~~~ 

# Switchcase

~~~~

  

    case expression in

  pattern1 )

    statements ;;

  pattern2 )

    statements ;;

esac

~~~~

  
  

# case example

~~~~

#!/bin/bash

  

case $1 in

start)

echo starting

;;

stop)

echo stoping

;;

restart)

echo restarting

;;

*)

echo don\'t know

;;

esac

~~~~

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>

  

~~~~

#!/bin/bash

space_free=$( df -h | awk '{ print $5 }' | sort -n | tail -n 1 | sed 's/%//' )

case $space_free in

[1-5]*)

echo Plenty of disk space available

;;

[6-7]*)

echo There could be a problem in the near future

;;

8*)

echo Maybe we should look at clearing out old files

;;

9*)

echo We could have a serious problem on our hands soon

;;

*)

echo Something is not quite right here

;;

esac

~~~~

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>  

  
  
  

# Until

# ----------------------

  

~~~~

until condition; do

  statements

done

~~~~

  

# Basic until loop

  

~~~~

#!/bin/bash

counter=1

until [ $counter -gt 10 ]

do

echo $counter

((counter++))

done

echo All done

~~~~

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-loops.php#while>  

  
  
  

# Variables

  

~~~~

  

  • varname=value                # defines a variable

  

  • varname=value command        # defines a variable to be in the environment of a particular subprocess

  

  • echo $varname                # checks a variable's value

  

  • echo $$                      # prints process ID of the current shell

  

  • echo $!                      # prints process ID of the most recently invoked background job

  

  • echo $?                      # displays the exit status of the last command

  

  • read <varname>               # reads a string from the input and assigns it to a variable  

  

  • let <varname> = <equation>   # performs mathematical calculation using operators like +, -, *, /, %

  

  • export VARNAME=value         # defines an environment variable (will be available in subprocesses)

  
  
  

    • ${varname:-word}             # if varname exists and isn't null, return its value; otherwise return word

  

  • ${varname:=word}             # if varname exists and isn't null, return its value; otherwise set it word and then return its value

  

  • ${varname:?message}          # if varname exists and isn't null, return its value; otherwise print varname, followed by message and abort the current command or script

  

  • ${varname:+word}             # if varname exists and isn't null, return word; otherwise return null

  

  • ${varname:offset:length}     # performs substring expansion. It returns the substring of $varname starting at offset and up to length characters

  

  • ${variable#pattern}          # if the pattern matches the beginning of the variable's value, delete the shortest part that matches and return the rest

  

  • ${variable##pattern}         # if the pattern matches the beginning of the variable's value, delete the longest part that matches and return the rest

  

  • ${variable%pattern}          # if the pattern matches the end of the variable's value, delete the shortest part that matches and return the rest

  

  • ${variable%%pattern}         # if the pattern matches the end of the variable's value, delete the longest part that matches and return the rest

  

  • ${variable/pattern/string}   # the longest match to pattern in variable is replaced by string. Only the first match is replaced

  

  • ${variable//pattern/string}  # the longest match to pattern in variable is replaced by string. All matches are replaced

  

  • ${#varname}                  # returns the length of the value of the variable as a character string

  
  

• *(patternlist)               # matches zero or more occurrences of the given patterns

  

• +(patternlist)               # matches one or more occurrences of the given patterns

  

• ?(patternlist)               # matches zero or one occurrence of the given patterns

  

• @(patternlist)               # matches exactly one of the given patterns

  

• !(patternlist)               # matches anything except one of the given patterns

  
  

  • $(UNIX command)              # command substitution: runs the command and returns standard output

  

~~~~

  

# While

  

~~~~

while condition; do

  

  statements

  

done  

~~~~

  

# Basic while loop

  

~~~~

#!/bin/bash

  

counter=1

  

while [ $counter -le 10 ]

  

do

  

echo $counter

  

((counter++))

  

done

  

echo All done

~~~~

 
  
  
  
## Whoami

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-loops.php#while>    

~~~~

if [[ `whoami` != root && `whoami` != obi ]]

then

   print "Script must be run as root or obi user."

   exit 1

fi

~~~~

[[Index]] 
