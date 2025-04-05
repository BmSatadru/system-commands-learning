# Shell Variables in Linux: A Comprehensive Guide

Shell variables is fundamental to becoming proficient in Linux. Shell variables store information that can be referenced and manipulated in scripts and on the command line. Let's explore the different types of shell variables with practical examples.

## Frequently Used Shell Variables

These environment variables are available in most Linux shells and provide information about the user's environment.

### $USER

The $USER variable contains the username of the currently logged-in user.

```bash
echo $USER
```

Example output:
```
gaurav
```

### $HOME

The $HOME variable points to the current user's home directory.

```bash
echo $HOME
```

Example output:
```
/home/gaurav
```

This variable is commonly used in scripts to reference files in the user's home directory:

```bash
ls $HOME/Documents
```

### $PATH

The $PATH variable contains a colon-separated list of directories where the shell looks for executable files.

```bash
echo $PATH
```

Example output:
```
/usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin
```

When you type a command, the shell searches through each directory listed in $PATH until it finds an executable with that name. If you want to add a custom directory to your PATH:

```bash
export PATH=$PATH:/home/gaurav/scripts
```

### $PWD

The $PWD variable contains the full path to the current working directory.

```bash
echo $PWD
```

Example output:
```
/home/gaurav/projects
```

This is equivalent to running the `pwd` command.

### $HOSTNAME

The $HOSTNAME variable contains the name of the machine you're working on.

```bash
echo $HOSTNAME
```

Example output:
```
learning-ocean
```

## Special Shell Variables

These variables are used primarily in shell scripts to access parameters, check status, and get information about the current process.

### $0

The $0 variable contains the name of the script being executed.

```bash
#!/bin/bash
echo "Script name: $0"
```

If this script is named `test.sh`, the output would be:
```
Script name: ./test.sh
```

### $1 to $9

These variables contain the positional parameters (arguments) passed to a script.

```bash
#!/bin/bash
echo "First argument: $1"
echo "Second argument: $2"
echo "Third argument: $3"
```

If you run this script as `./script.sh hello world linux`, the output would be:
```
First argument: hello
Second argument: world
Third argument: linux
```

For arguments beyond $9, you need to use ${10}, ${11}, etc.

### $#

The $# variable contains the number of arguments passed to a script.

```bash
#!/bin/bash
echo "Number of arguments: $#"
```

If you run this script with three arguments: `./script.sh arg1 arg2 arg3`, the output would be:
```
Number of arguments: 3
```

### $@

The $@ variable contains all the arguments passed to a script as separate strings.

```bash
#!/bin/bash
echo "All arguments: $@"
```

Example output with `./script.sh arg1 arg2 arg3`:
```
All arguments: arg1 arg2 arg3
```

This is particularly useful in loops:

```bash
#!/bin/bash
for arg in "$@"
do
    echo "Argument: $arg"
done
```

### $?

The $? variable contains the exit status of the last executed command. A value of 0 typically indicates success, while non-zero values indicate failure.

```bash
ls /home
echo "Exit status: $?"
```

Example output (if /home exists):
```
gaurav
Exit status: 0
```

Now let's try with a non-existent directory:

```bash
ls /nonexistent
echo "Exit status: $?"
```

Example output:
```
ls: cannot access '/nonexistent': No such file or directory
Exit status: 2
```

This is extremely useful for error checking in scripts:

```bash
#!/bin/bash
cp file1.txt backup/
if [ $? -eq 0 ]; then
    echo "Backup successful"
else
    echo "Backup failed"
fi
```

### $$

The $$ variable contains the process ID (PID) of the current shell.

```bash
echo "Current shell PID: $$"
```

Example output:
```
Current shell PID: 2433
```

This is useful for creating unique temporary files:

```bash
TEMPFILE="/tmp/myapp_$$"
echo "Creating temporary file: $TEMPFILE"
```

### $-

The $- variable displays the current shell options (flags) that are set.

```bash
echo $-
```

Example output:
```
himBHs
```

Each character represents a different shell option:
- h: Remember the location of commands
- i: Interactive shell
- m: Job control is enabled
- B: Brace expansion enabled
- H: History expansion enabled
- s: Commands are read from standard input

## The Echo Command

The `echo` command is one of the most basic yet versatile commands in Linux, used to display text and variable values.

### Basic Echo Usage

```bash
echo Hello World
```

Output:
```
Hello World
```

### Echo with Multiple Arguments

Echo can take multiple arguments separated by spaces:

```bash
echo First Second Third
```

Output:
```
First Second Third
```

### Echo with Quotes

Using double quotes preserves spaces but allows variable expansion:

```bash
echo "Hello    World"
echo "Your username is $USER"
```

Output:
```
Hello    World
Your username is gaurav
```

Using single quotes preserves everything literally, including variables:

```bash
echo 'Your username is $USER'
```

Output:
```
Your username is $USER
```

### Multi-line Echo and Nesting Quotes

You can create multi-line output with echo:

```bash
echo "This is line 1
This is line 2
This is line 3"
```

To nest quotes, you can alternate between single and double quotes:

```bash
echo 'He said "Hello" to me'
echo "She said 'Goodbye' to me"
```

Output:
```
He said "Hello" to me
She said 'Goodbye' to me
```

### Variables Not Expanded in Single Quotes

As mentioned earlier, variables are not expanded within single quotes:

```bash
echo "Home directory: $HOME"
echo 'Home directory: $HOME'
```

Output:
```
Home directory: /home/gaurav
Home directory: $HOME
```

### Escaping $ Using \$

If you want to print a literal $ character in double quotes, you need to escape it with a backslash:

```bash
echo "The cost is \$5"
```

Output:
```
The cost is $5
```

## Viewing Environment Variables

There are several commands to view environment variables:

### printenv

The `printenv` command displays all environment variables:

```bash
printenv
```

To view a specific variable:

```bash
printenv USER
```

### env

The `env` command also displays all environment variables:

```bash
env
```

### set

The `set` command displays all variables (including shell variables) and functions:

```bash
set
```

## Working with the Date Command

The `date` command displays the current date and time:

```bash
date
```

Example output:
```
Tue Apr 1 01:39:00 IST 2025
```

### date -R

The `-R` option displays the date in RFC 5322 format:

```bash
date -R
```

Example output:
```
Tue, 01 Apr 2025 01:39:00 +0530
```

## Running Unaliased Commands

Sometimes commands are aliased to add options or modify behavior. To run the original command without aliases:

### Using Backslash

```bash
\date
```

### Using Full Path

```bash
/usr/bin/date
```

## Process Status Commands

The `ps` command displays information about active processes:

### Basic ps

```bash
ps
```

Example output:
```
  PID TTY          TIME CMD
 2433 pts/0    00:00:00 bash
 2602 pts/0    00:00:00 ps
```

### ps --forest

The `--forest` option shows the process hierarchy:

```bash
ps --forest
```

Example output:
```
  PID TTY          TIME CMD
 2433 pts/0    00:00:00 bash
 2603 pts/0    00:00:00  \_ ps --forest
```

### ps -ef

The `-ef` options show all processes in full format:

```bash
ps -ef
```

Example output:
```
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 Mar31 ?        00:00:02 /sbin/init
root         2     0  0 Mar31 ?        00:00:00 [kthreadd]
...
gaurav    2433  2432  0 01:30 pts/0    00:00:00 bash
gaurav    2604  2433  0 01:39 pts/0    00:00:00 ps -ef
```

### ps -f

The `-f` option shows processes in full format:

```bash
ps -f
```

Example output:
```
UID        PID  PPID  C STIME TTY          TIME CMD
gaurav    2433  2432  0 01:30 pts/0    00:00:00 bash
gaurav    2605  2433  0 01:39 pts/0    00:00:00 ps -f
```

### ps -e

The `-e` option shows all processes:

```bash
ps -e
```

Example output:
```
  PID TTY          TIME CMD
    1 ?        00:00:02 init
    2 ?        00:00:00 kthreadd
    ...
 2433 pts/0    00:00:00 bash
 2606 pts/0    00:00:00 ps
```