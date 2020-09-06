# cheatsheet

### Example

```text
#!/bin/bash

NAME="Payam"
echo "Hello $NAME!"

exit 0
```

### Variables:

```text
varname=value                # defines a variable
varname=value command        # defines a variable to be in the environment of a particular subprocess
echo $varname                # checks a variable's value
read <varname>               # reads a string from the input and assigns it to a variable 
let <varname> = <equation>   # performs mathematical calculation using operators like +, -, *, /, %
export VARNAME=value         # defines an environment variable (will be available in subprocesses)
```

```text
#Special shell variables
echo $$                      # prints process ID of the current shell
echo $!                      # prints process ID of the most recently invoked background job
echo $?                      # displays the exit status of the last command
echo $0                      # display Filename of the shell script
```

### Quoting:

```text
\c             #Take character c literally.
`cmd`          #Run cmd and replace it in the line of code with its output.
"whatever"     #Take whatever literally, after first interpreting $, `...`, \
'whatever'     #Take whatever absolutely literally.

#Example:
match=`ls *.bak`        #Puts names of .bak files into shell variable match.
echo \*                 #Echos * to screen, not all filename as in:  echo *
echo '$1$2hello'        #Writes literally $1$2hello on screen.
echo "$1$2hello"        #Writes value of parameters 1 and 2 and string hello.
```

### Redirection:

```text
python hello.py > output.txt   # stdout to (file)
python hello.py >> output.txt  # stdout to (file), append
python hello.py 2> error.log   # stderr to (file)
python hello.py 2>&1           # stderr to stdout
python hello.py 2>/dev/null    # stderr to (null)
python hello.py &>/dev/null    # stdout and stderr to (null)
python hello.py < foo.txt      # feed foo.txt to stdin for python
```

## Conditionals:

### Test Operators 

In Bash, the `test` command takes one of the following syntax forms:

* **test EXPRESSION**
* **\[ EXPRESSION \]**
* **\[\[ EXPRESSION \]\]**

To make the script portable, prefer using the old test `[` command which is available on all POSIX shells. The new upgraded version of the `test` command `[[` \(double brackets\) is supported on most modern systems using Bash, Zsh, and Ksh as a default shell.  To negate the test expression, use the logical `NOT` \(`!`\) operator.

### Checking Numbers

_Note that a shell variable could contain a string that represents a number. If you want to check the numerical value use one of the following:_

```text
[[ NUM -eq NUM ]]	Equal
[[ NUM -ne NUM ]]	Not equal
[[ NUM -lt NUM ]]	Less than
[[ NUM -le NUM ]]	Less than or equal
[[ NUM -gt NUM ]]	Greater than
[[ NUM -ge NUM ]]	Greater than or equal
```

### Checking Strings

```text
[[ -z STRING ]]	Empty string
[[ -n STRING ]]	Not empty string
[[ STRING == STRING ]]	Equal
[[ STRING != STRING ]]	Not Equal

```

### Checking files

```text
[[ -e FILE ]]	Exists
[[ -r FILE ]]	Readable
[[ -h FILE ]]	Symlink
[[ -d FILE ]]	Directory
[[ -w FILE ]]	Writable
[[ -s FILE ]]	Size is > 0 bytes
[[ -f FILE ]]	File
[[ -x FILE ]]	Executable
[[ FILE1 -nt FILE2 ]]	1 is more recent than 2
[[ FILE1 -ot FILE2 ]]	2 is more recent than 1
[[ FILE1 -ef FILE2 ]]	Same files
```

### More conditions:

```text
[[ -o noclobber ]]	If OPTIONNAME is enabled
[[ ! EXPR ]]	Not
[[ X && Y ]]	And
[[ X || Y ]]	Or
```

### if statement:

```text
#if Statement 

echo -n "Enter a number: "
read VAR

if [[ $VAR -gt 10 ]]
then
  echo "The variable is greater than 10."
fi
```

```text
#if..else Statement

echo -n "Enter a number: "
read VAR

if [[ $VAR -gt 10 ]]
then
  echo "The variable is greater than 10."
else
  echo "The variable is equal or less than 10."
fi
```

```text
#if..elif..else Statement

echo -n "Enter a number: "
read VAR

if [[ $VAR -gt 10 ]]
then
  echo "The variable is greater than 10."
elif [[ $VAR -eq 10 ]]
then
  echo "The variable is equal to 10."
else
  echo "The variable is less than 10."
fi
```

```text
# Nested if Statements
echo -n "Enter the first number: "
read VAR1
echo -n "Enter the second number: "
read VAR2
echo -n "Enter the third number: "
read VAR3

if [[ $VAR1 -ge $VAR2 ]]
then
  if [[ $VAR1 -ge $VAR3 ]]
  then
    echo "$VAR1 is the largest number."
  else
    echo "$VAR3 is the largest number."
  fi
else
  if [[ $VAR2 -ge $VAR3 ]]
  then
    echo "$VAR2 is the largest number."
  else
    echo "$VAR3 is the largest number."
  fi
fi
```

## Loops:

### for:

```text
#Basic for loop
for i in /etc/rc.*; do
  echo $i
done
```

```text
#C-Like for loop
for ((i = 0 ; i < 100 ; i++)); do
  echo $i
done
```

```text
#Ranges
for i in {1..5}; do
    echo "Welcome $i"
done
```

```text
#with step size
for i in {5..50..5}; do
    echo "Welcome $i"
done
```

### while:

```text
n=1

while [ $n -le 5 ]
do
	echo "Welcome $n times."
	n=$(( n+1 ))	
done
```

```text
#Using ((expression)) Format With The While Loop
n=1
while (( $n <= 5 ))
do
	echo "Welcome $n times."
	n=$(( n+1 ))	
done
```

```text
#for ever
while true; do
  ···
done
```

```text
# Reading a test file:
###example1/2:
cat /etc/resolv.conf | while read line; do
  echo $line
done

###example2/2:
file=/etc/resolv.conf
while IFS= read -r line
do
	echo $line
done < "$file"

### Reading A Text File With Separate Fields:
file=/etc/resolv.conf
# set field separator to a single white space 
while IFS=' ' read -r f1 f2
do
	echo "field # 1 : $f1 ==> field #2 : $f2"
done < "$file"
```

### Until:

```text
#!/bin/bash

counter=0

until [ $counter -gt 5 ]
do
  echo Counter: $counter
  ((counter++))
done
```

### Case:

```text
Case/switch
case "$1" in
  start | up)
    vagrant up
    ;;

  *)
    echo "Usage: $0 {start|stop|ssh}"
    ;;
esac
```

### Functions:

```text
# Defining functions:
myfunc() {
    echo "hello $1"
}
# Same as above (alternate syntax)
function myfunc() {
    echo "hello $1"
}
myfunc "John"
```

```text
#Returning values:
myfunc() {
    local myresult='some value'
    echo $myresult
}
result="$(myfunc)"
```

```text
#Raising errors:
myfunc() {
  return 1
}
if myfunc; then
  echo "success"
else
  echo "failure"
fi
```

```text
#Arguments:
$#	Number of arguments
$*	All arguments
$@	All arguments, starting from first
$1	First argument
$_	Last argument of the previous command
```

### Arrays:

```text
Defining arrays
Fruits=('Apple' 'Banana' 'Orange')
Fruits[0]="Apple"
Fruits[1]="Banana"
Fruits[2]="Orange"
```

```text
Operations
Fruits=("${Fruits[@]}" "Watermelon")    # Push
Fruits+=('Watermelon')                  # Also Push
Fruits=( ${Fruits[@]/Ap*/} )            # Remove by regex match
unset Fruits[2]                         # Remove one item
Fruits=("${Fruits[@]}")                 # Duplicate
Fruits=("${Fruits[@]}" "${Veggies[@]}") # Concatenate
lines=(`cat "logfile"`)                 # Read from file
```

```text
Working with arrays
echo ${Fruits[0]}           # Element #0
echo ${Fruits[-1]}          # Last element
echo ${Fruits[@]}           # All elements, space-separated
echo ${#Fruits[@]}          # Number of elements
echo ${#Fruits}             # String length of the 1st element
echo ${#Fruits[3]}          # String length of the Nth element
echo ${Fruits[@]:3:2}       # Range (from position 3, length 2)
echo ${!Fruits[@]}          # Keys of all elements, space-separated
```

```text
Iteration
for i in "${arrayName[@]}"; do
  echo $i
done
```

### Dictionaries:

```text
Defining
declare -A sounds
sounds[dog]="bark"
sounds[cow]="moo"
sounds[bird]="tweet"
sounds[wolf]="howl"
```

```text
Working with dictionaries
echo ${sounds[dog]} # Dog's sound
echo ${sounds[@]}   # All values
echo ${!sounds[@]}  # All keys
echo ${#sounds[@]}  # Number of elements
unset sounds[dog]   # Delete dog
```

```text
Iteration
Iterate over values
for val in "${sounds[@]}"; do
  echo $val
done
Iterate over keys
for key in "${!sounds[@]}"; do
  echo $key
done
```

### Debugging

```text
bash -n scriptname  # don't run commands; check for syntax errors only
set -o noexec       # alternative (set option in script)

bash -v scriptname  # echo commands before running them
set -o verbose      # alternative (set option in script)

bash -x scriptname  # echo commands after command-line processing
set -o xtrace       # alternative (set option in script)
```

## Miscellaneous:

```text
#Numeric calculations
$((a + 200))      # Add 200 to $a
$(($RANDOM%200))  # Random number 0..199
```

```text
#Inspecting commands
command -V cd
#=> "cd is a function/alias/whatever"
```

```text
#Heredoc:
cat <<END
hello world
END
```

```text
#printf:
printf "Hello %s, I'm %s" Sven Olga
#=> "Hello Sven, I'm Olga

printf "1 + 1 = %d" 2
#=> "1 + 1 = 2"

printf "This is how you print a float: %f" 2
#=> "This is how you print a float: 2.000000"
```

```text
#Reading input
echo -n "Proceed? [y/n]: "
read ans
echo $ans

#Reading  Just one character:
read -n 1 ans    
```

```text
#Getting options
while [[ "$1" =~ ^- && ! "$1" == "--" ]]; do case $1 in
  -V | --version )
    echo $version
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

```text
#Check for command’s result
if ping -c 1 google.com; then
  echo "It appears you have a working internet connection"
fi
```

