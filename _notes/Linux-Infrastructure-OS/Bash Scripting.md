
[[Index]] 

### Example

```
#!/usr/bin/env bash

name="John"
echo "Hello $name!"
```

## Running script

```
  • chmod +x _filename 
  
  • ./ -> Runs scripts as an excecutable
```







## DEBUGGING SHELL PROGRAMS


```bash

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

  • ./ -> Runs scripts as an excecutable

  
  

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

```
  
### Shell execution

```bash
echo "I'm in $(pwd)"
echo "I'm in `pwd`"  # obsolescent
# Same
```



## For Loops

```bash

for x in {1..10}

do

  statements

done

  

```

  
  

```bash

for name [in list]

do

  statements that can use $name

done

```

  

```bash

for (( initialisation ; ending condition ; update ))

do

  statements...

done

```

### Basic for loop

```bash
for i in /etc/rc.*; do
  echo "$i"
done
```

### C-like for loop

```bash
for ((i = 0 ; i < 100 ; i++)); do
  echo "$i"
done
```

### Ranges

```bash
for i in {1..5}; do
    echo "Welcome $i"
done
```

#### With step size

```bash
for i in {5..50..5}; do
    echo "Welcome $i"
done
```  

## Functions

  

The function refers to passed arguments by position (as if they were positional parameters), that is, $1, $2, and so forth.

$@ is equal to "$1" "$2"... "$N", where N is the number of positional parameters. $# holds the number of positional parameters.

  

```bash

function functname() {

  shell commands

}

```

  

Example:

```bash

!/bin/bash

  

Basic function

print_something () {

  echo Hello I am a function

   }

  print_something

  print_something

  

```

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-functions.php>


  
  
  
  • unset -f functname  # deletes a function definition

  • declare -f          # displays all defined functions in your login session

  
  

```bash

#!/bin/bash

# Passing arguments to a function

print_something () {

echo Hello $1

}

print_something Mars

print_something Jupiter

```

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-functions.php>

  

```bash

#!/bin/bash

# Setting a return status for a function

print_something () {

echo Hello $1

return 5

}

print_something Mars

print_something Jupiter

echo The previous function has a return value of $?

```

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-functions.php>

  
  

```bash

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

```

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-functions.php>  


### Functions Extra Examples

```bash
get_name() {
  echo "John"
}

echo "You are $(get_name)"
```  
### Defining functions

```bash
myfunc() {
    echo "hello $1"
}
```

```bash
# Same as above (alternate syntax)
function myfunc() {
    echo "hello $1"
}
```

```bash
myfunc "John"
```

### Returning values

```bash
myfunc() {
    local myresult='some value'
    echo "$myresult"
}
```

```bash
result=$(myfunc)
```

### Raising errors

```bash
myfunc() {
  return 1
}
```

```bash
if myfunc; then
  echo "success"
else
  echo "failure"
fi
```

  
  

## If Statement

  

```bash

if condition

then

  statements

[elif condition

  then statements...]

[else

  statements]

fi

```

```bash

#!/bin/bash

# else example

if [ $# -eq 1 ]

then

nl $1

else

nl /dev/stdin

fi

```

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>

  

## or example

```bash

#!/bin/bash

if [ $USER == 'bob' ] || [ $USER == 'andy' ]

then

ls -alh

else

ls

fi

```

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>

  

## Basic if statement

```bash

#!/bin/bash

if [ $1 -gt 100 ]

then

echo Hey that\'s a large number.

pwd

fi

date

```

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>

  

## elif statements

```bash

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

```

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>

  

## Nested if statements

```bash

#!/bin/bash

if [ $1 -gt 100 ]

then

echo Hey that\'s a large number.

if (( $1 % 2 == 0 ))

then

echo And is also an even number.

fi

fi

```

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>

  

## and example

```bash

#!/bin/bash

if [ -r $1 ] && [ -s $1 ]

then

echo This file is useful.

fi

```

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>  

  
### Arguments

| Expression | Description |
| --- | --- |
| `$#` | Number of arguments |
| `$*` | All positional arguments (as a single word) |
| `$@` | All positional arguments (as separate strings) |
| `$1` | First argument |
| `$_` | Last argument of the previous command |

**Note**: `$@` and `$*` must be quoted in order to perform as described. Otherwise, they do exactly the same thing (arguments as separate strings).
  
  

## INPUT/OUTPUT REDIRECTORS
  
```bash

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

```  
  





## Operators

    
```bash

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

  
```  

### Conditional execution

```bash
git commit && git push
git commit || echo "Commit failed"
```

### Conditionals

```bash
if [[ -z "$string" ]]; then
  echo "String is empty"
elif [[ -n "$string" ]]; then
  echo "String is not empty"
fi
```

### Conditions

Note that `[[` is actually a command/program that returns either `0` (true) or `1` (false). Any program that obeys the same logic (like all base utils, such as `grep(1)` or `ping(1)`) can be used as condition, see examples.

| Condition | Description |
| --- | --- |
| `[[ -z STRING ]]` | Empty string |
| `[[ -n STRING ]]` | Not empty string |
| `[[ STRING == STRING ]]` | Equal |
| `[[ STRING != STRING ]]` | Not Equal |
| `[[ NUM -eq NUM ]]` | Equal |
| `[[ NUM -ne NUM ]]` | Not equal |
| `[[ NUM -lt NUM ]]` | Less than |
| `[[ NUM -le NUM ]]` | Less than or equal |
| `[[ NUM -gt NUM ]]` | Greater than |
| `[[ NUM -ge NUM ]]` | Greater than or equal |
| `[[ STRING =~ STRING ]]` | Regexp |
| `(( NUM < NUM ))` | Numeric conditions |

#### More conditions

| Condition | Description |
| --- | --- |
| `[[ -o noclobber ]]` | If OPTIONNAME is enabled |
| `[[ ! EXPR ]]` | Not |
| `[[ X && Y ]]` | And |
| `[[ X || Y ]]` | Or |

### File conditions

| Condition | Description |
| --- | --- |
| `[[ -e FILE ]]` | Exists |
| `[[ -r FILE ]]` | Readable |
| `[[ -h FILE ]]` | Symlink |
| `[[ -d FILE ]]` | Directory |
| `[[ -w FILE ]]` | Writable |
| `[[ -s FILE ]]` | Size is > 0 bytes |
| `[[ -f FILE ]]` | File |
| `[[ -x FILE ]]` | Executable |
| `[[ FILE1 -nt FILE2 ]]` | 1 is more recent than 2 |
| `[[ FILE1 -ot FILE2 ]]` | 2 is more recent than 1 |
| `[[ FILE1 -ef FILE2 ]]` | Same files |

### Example

```bash
# String
if [[ -z "$string" ]]; then
  echo "String is empty"
elif [[ -n "$string" ]]; then
  echo "String is not empty"
else
  echo "This never happens"
fi
```

```bash
# Combinations
if [[ X && Y ]]; then
  ...
fi
```

```bash
# Equal
if [[ "$A" == "$B" ]]
```

```bash
# Regex
if [[ "A" =~ . ]]
```

```bash
if (( $a < $b )); then
   echo "$a is smaller than $b"
fi
```

```bash
if [[ -e "file.txt" ]]; then
  echo "file exists"
fi
```

### Brace expansion

```bash
echo {A,B}.js
```

| Expression | Description |
| --- | --- |
| `{A,B}` | Same as `A B` |
| `{A,B}.js` | Same as `A.js B.js` |
| `{1..5}` | Same as `1 2 3 4 5` |

See: [Brace expansion](http://wiki.bash-hackers.org/syntax/expansion/brace)

### Basics

```bash
name="John"
echo "${name}"
echo "${name/J/j}"    #=> "john" (substitution)
echo "${name:0:2}"    #=> "Jo" (slicing)
echo "${name::2}"     #=> "Jo" (slicing)
echo "${name::-1}"    #=> "Joh" (slicing)
echo "${name:(-1)}"   #=> "n" (slicing from right)
echo "${name:(-2):1}" #=> "h" (slicing from right)
echo "${food:-Cake}"  #=> $food or "Cake"
```

```bash
length=2
echo "${name:0:length}"  #=> "Jo"
```

See: [Parameter expansion](http://wiki.bash-hackers.org/syntax/pe)

```bash
str="/path/to/foo.cpp"
echo "${str%.cpp}"    # /path/to/foo
echo "${str%.cpp}.o"  # /path/to/foo.o
echo "${str%/*}"      # /path/to

echo "${str##*.}"     # cpp (extension)
echo "${str##*/}"     # foo.cpp (basepath)

echo "${str#*/}"      # path/to/foo.cpp
echo "${str##*/}"     # foo.cpp

echo "${str/foo/bar}" # /path/to/bar.cpp
```

```bash
str="Hello world"
echo "${str:6:5}"   # "world"
echo "${str: -5:5}"  # "world"
```

```bash
src="/path/to/foo.cpp"
base=${src##*/}   #=> "foo.cpp" (basepath)
dir=${src%$base}  #=> "/path/to/" (dirpath)
```

### Substitution

| Code | Description |
| --- | --- |
| `${foo%suffix}` | Remove suffix |
| `${foo#prefix}` | Remove prefix |
| `${foo%%suffix}` | Remove long suffix |
| `${foo##prefix}` | Remove long prefix |
| `${foo/from/to}` | Replace first match |
| `${foo//from/to}` | Replace all |
| `${foo/%from/to}` | Replace suffix |
| `${foo/#from/to}` | Replace prefix |

```

## Single line comment
```

```bash
: '
This is a
multi line
comment
'
```

### Substrings

| Expression | Description |
| --- | --- |
| `${foo:0:3}` | Substring _(position, length)_ |
| `${foo:(-3):3}` | Substring from the right |

### Length

| Expression | Description |
| --- | --- |
| `${#foo}` | Length of `$foo` |


### Manipulation

```
str="HELLO WORLD!"
echo "${str,}"   #=> "hELLO WORLD!" (lowercase 1st letter)
echo "${str,,}"  #=> "hello world!" (all lowercase)

str="hello world!"
echo "${str^}"   #=> "Hello world!" (uppercase 1st letter)
echo "${str^^}"  #=> "HELLO WORLD!" (all uppercase)
```

### Default values

| Expression | Description |
| --- | --- |
| `${foo:-val}` | `$foo`, or `val` if unset (or null) |
| `${foo:=val}` | Set `$foo` to `val` if unset (or null) |
| `${foo:+val}` | `val` if `$foo` is set (and not null) |
| `${foo:?message}` | Show error message and exit if `$foo` is unset (or null) |

Omitting the `:` removes the (non)nullity checks, e.g. `${foo-val}` expands to `val` if unset otherwise `$foo`.

## [#](https://devhints.io/bash#loops)Loops



## Process Handling

  
  

## To suspend a job, type CTRL+Z while it is running. You can also suspend a job with CTRL+Y.

  

##  This is slightly different from CTRL+Z in that the process is only stopped when it attempts to read input from terminal.

## Of course, to interrupt a job, type CTRL+C.

  
```bash

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

```
  
  
  

## Strings

```bash
  
  

  • str1 == str2               # str1 matches str2

  • str1 != str2               # str1 does not match str2

  • str1 < str2                # str1 is less than str2 (alphabetically)

  • str1 > str2                # str1 is greater than str2 (alphabetically)

  • str1 \> str2               # str1 is sorted after str2

  • str1 \< str2               # str1 is sorted before str2

  • -n str1                    # str1 is not null (has length greater than 0)

        * -z str1                    # str1 is null (has length 0)

``` 

### Defining arrays

```bash
Fruits=('Apple' 'Banana' 'Orange')
```

```bash
Fruits[0]="Apple"
Fruits[1]="Banana"
Fruits[2]="Orange"
```

### Working with arrays

```bash
echo "${Fruits[0]}"           # Element #0
echo "${Fruits[-1]}"          # Last element
echo "${Fruits[@]}"           # All elements, space-separated
echo "${#Fruits[@]}"          # Number of elements
echo "${#Fruits}"             # String length of the 1st element
echo "${#Fruits[3]}"          # String length of the Nth element
echo "${Fruits[@]:3:2}"       # Range (from position 3, length 2)
echo "${!Fruits[@]}"          # Keys of all elements, space-separated
```

### Operations

```bash
Fruits=("${Fruits[@]}" "Watermelon")    # Push
Fruits+=('Watermelon')                  # Also Push
Fruits=( "${Fruits[@]/Ap*/}" )          # Remove by regex match
unset Fruits[2]                         # Remove one item
Fruits=("${Fruits[@]}")                 # Duplicate
Fruits=("${Fruits[@]}" "${Veggies[@]}") # Concatenate
lines=(`cat "logfile"`)                 # Read from file
```

### Iteration

```bash
for i in "${arrayName[@]}"; do
  echo "$i"
done
```

## [#](https://devhints.io/bash#dictionaries)Dictionaries

### Defining

```bash
declare -A sounds
```

```bash
sounds[dog]="bark"
sounds[cow]="moo"
sounds[bird]="tweet"
sounds[wolf]="howl"
```

Declares `sound` as a Dictionary object (aka associative array).

### Slices

| Code | Description |
| --- | --- |
| `!!:n` | Expand only `n`th token from most recent command (command is `0`; first argument is `1`) |
| `!^` | Expand first argument from most recent command |
| `!$` | Expand last token from most recent command |
| `!!:n-m` | Expand range of tokens from most recent command |
| `!!:n-$` | Expand `n`th token to last from most recent command |

`!!` can be replaced with any valid expansion i.e. `!cat`, `!-2`, `!42`, etc.

### String quotes

```bash
name="John"
echo "Hi $name"  #=> Hi John
echo 'Hi $name'  #=> Hi $name
```
### Numeric calculations

```bash
$((a + 200))      # Add 200 to $a
```

```bash
$(($RANDOM%200))  # Random number 0..199
```

```bash
declare -i count  # Declare as type integer 
count+=1          # Increment
```

### Subshells

```bash
(cd somedir; echo "I'm now in $PWD")
pwd # still in first directory
```

### Redirection

```bash
python hello.py > output.txt            # stdout to (file)
python hello.py >> output.txt           # stdout to (file), append
python hello.py 2> error.log            # stderr to (file)
python hello.py 2>&1                    # stderr to stdout
python hello.py 2>/dev/null             # stderr to (null)
python hello.py >output.txt 2>&1        # stdout and stderr to (file), equivalent to &>
python hello.py &>/dev/null             # stdout and stderr to (null)
echo "$0: warning: too many users" >&2  # print diagnostic message to stderr
```

```bash
python hello.py < foo.txt      # feed foo.txt to stdin for python
diff <(ls -r) <(ls)            # Compare two stdout without files
```

### Inspecting commands

```bash
command -V cd
#=> "cd is a function/alias/whatever"
```

### Trap errors

```bash
trap 'echo Error at about $LINENO' ERR
```

or

```bash
traperr() {
  echo "ERROR: ${BASH_SOURCE[1]} at about ${BASH_LINENO[0]}"
}

set -o errtrace
trap traperr ERR
```

## Switchcase

```bash

  

  case expression in

  pattern1 )

    statements ;;

  pattern2 )

    statements ;;

esac

```bash

  
  

## case example

```bash

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

```

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>

  

```bash

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

```

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php>  

### Case/switch

```bash
case "$1" in
  start | up)
    vagrant up
    ;;

  *)
    echo "Usage: $0 {start|stop|ssh}"
    ;;
esac
``` 
  
  

## Until
## ----------------------

  

```bash

until condition; do

  statements

done

```

  

## Basic until loop

  

```bash

#!/bin/bash

counter=1

until [ $counter -gt 10 ]

do

echo $counter

((counter++))

done

echo All done

```

  

From <https://ryanstutorials.net/bash-scripting-tutorial/bash-loops.php#while>  

  
  
  

## Variables

  

```bash

  

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

```

```bash
name="John"
echo $name  # see below
echo "$name"
echo "${name}!"
```

Generally quote your variables unless they contain wildcards to expand or command fragments.

```bash
wildcard="*.txt"
option="iv"
cp -$options $wildcard /tmp
```

  

## While

  

```bash

while condition; do

  

  statements

  

done  

```

  

## Basic while loop

  

```bash

#!/bin/bash

  

counter=1

  

while [ $counter -le 10 ]

  

do

  

echo $counter

  

((counter++))

  

done

  

echo All done

```

### Reading lines

```bash
while read -r line; do
  echo "$line"
done <file.txt
``` 
  
### Working with dictionaries

```bash
echo "${sounds[dog]}" # Dog's sound
echo "${sounds[@]}"   # All values
echo "${!sounds[@]}"  # All keys
echo "${#sounds[@]}"  # Number of elements
unset sounds[dog]     # Delete dog
```

### Iteration

#### Iterate over values

```bash
for val in "${sounds[@]}"; do
  echo "$val"
done
```

#### Iterate over keys

```bash
for key in "${!sounds[@]}"; do
  echo "$key"
done
```

## [#](https://devhints.io/bash#options)Options

### Options

```bash
set -o noclobber  # Avoid overlay files (echo "hi" > foo)
set -o errexit    # Used to exit upon error, avoiding cascading errors
set -o pipefail   # Unveils hidden failures
set -o nounset    # Exposes unset variables
```

### Glob options

```bash
shopt -s nullglob    # Non-matching globs are removed  ('*.foo' => '')
shopt -s failglob    # Non-matching globs throw errors
shopt -s nocaseglob  # Case insensitive globs
shopt -s dotglob     # Wildcards match dotfiles ("*.sh" => ".foo.sh")
shopt -s globstar    # Allow ** for recursive matches ('lib/**/*.rb' => 'lib/a/b/c.rb')
```

Set `GLOBIGNORE` as a colon-separated list of patterns to be removed from glob matches.

## [#](https://devhints.io/bash#history)History

### Commands

| Command | Description |
| --- | --- |
| `history` | Show history |
| `shopt -s histverify` | Don’t execute expanded result immediately |


  
## Whoami

```bash

if [[ `whoami` != root && `whoami` != obi ]]

then

   print "Script must be run as root or obi user."

   exit 1

fi

```bash

### Source relative

```bash
source "${0%/*}/../share/foo.sh"
```

### printf

```bash
printf "Hello %s, I'm %s" Sven Olga
#=> "Hello Sven, I'm Olga

printf "1 + 1 = %d" 2
#=> "1 + 1 = 2"

printf "This is how you print a float: %f" 2
#=> "This is how you print a float: 2.000000"

printf '%s\n' '#!/bin/bash' 'echo hello' >file
# format string is applied to each group of arguments
printf '%i+%i=%i\n' 1 2 3  4 5 9
```

### Transform strings

| Command option | Description |
| --- | --- |
| `-c` | Operations apply to characters not in the given set |
| `-d` | Delete characters |
| `-s` | Replaces repeated characters with single occurrence |
| `-t` | Truncates |
| `[:upper:]` | All upper case letters |
| `[:lower:]` | All lower case letters |
| `[:digit:]` | All digits |
| `[:space:]` | All whitespace |
| `[:alpha:]` | All letters |
| `[:alnum:]` | All letters and digits |

#### Example

```bash
echo "Welcome To Devhints" | tr '[:lower:]' '[:upper:]'
WELCOME TO DEVHINTS
```

### Directory of script

### Getting options

```bash
while [[ "$1" =~ ^- && ! "$1" == "--" ]]; do case $1 in
  -V | --version )
    echo "$version"
    exit
    ;;
  -s | --string )
    shift; string=$1
    ;;
  -f | --flag )
    flag=1
    ;;
esac; shift; done
if [[ "$1" == '--' ]]; then shift; fi
```

### Heredoc

```bash
cat <<END
hello world
END
```

### Reading input

```bash
echo -n "Proceed? [y/n]: "
read -r ans
echo "$ans"
```

The `-r` option disables a peculiar legacy behavior with backslashes.

```bash
read -n 1 ans    # Just one character
```

### Special variables

| Expression | Description |
| --- | --- |
| `$?` | Exit status of last task |
| `$!` | PID of last background task |
| `$$` | PID of shell |
| `$0` | Filename of the shell script |
| `$_` | Last argument of the previous command |
| `${PIPESTATUS[n]}` | return value of piped commands (array) |

See [Special parameters](http://wiki.bash-hackers.org/syntax/shellvars#special_parameters_and_shell_variables).

### Go to previous directory

```bash
pwd # /home/user/foo
cd bar/
pwd # /home/user/foo/bar
cd -
pwd # /home/user/foo
```

### Check for command’s result

```bash
if ping -c 1 google.com; then
  echo "It appears you have a working internet connection"
fi
```

### Grep check

```bash
if grep -q 'foo' ~/.bash_history; then
  echo "You appear to have typed 'foo' in the past"
fi
```

## [#](https://devhints.io/bash#also-see)Also see

-   [Bash-hackers wiki](http://wiki.bash-hackers.org/) _(bash-hackers.org)_
-   [Shell vars](http://wiki.bash-hackers.org/syntax/shellvars) _(bash-hackers.org)_
-   [Learn bash in y minutes](https://learnxinyminutes.com/docs/bash/) _(learnxinyminutes.com)_
-   [Bash Guide](http://mywiki.wooledge.org/BashGuide) _(mywiki.wooledge.org)_
-   [ShellCheck](https://www.shellcheck.net/) _(shellcheck.net)_


[[Index]] 
