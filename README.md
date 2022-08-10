# Table of content
- [Bash Scipting Intro](#Bash-Scipting-Intro)
- [Variables](#Variables)
- [Cat command in Linux with examples](#Cat-command-in-Linux-with-examples)
- [time command](#time-command)

# Bash Scipting Intro

## What bash means?
- bash stands for bourne-again shell

## Where bash is located?
- write in the terminal ``which bash``

## creating .sh file
- create hello world file ``touch hello.sh`` 

## Writing bash script
- ``#! /bin/bash`` 
    - this line helps the interpreter to know that this is a bash shell script

- ``echo "Hello World!"``
    - ``echo`` command is used to print whatever you write in double quotes

- ``./hello.sh`` 
    - to execute the script

- It won't be executed because you have not permissions to execute this file
- ``chmod +x hello.sh``
    - change the permisssions of the file before executing

- ``ls -al`` 
    - You can see now you have execution permission

- ``./hello.sh``

# Variables
## System variables
- Predefined by your OS
- Defined in Capital case
- ``echo $BASH`` 
    - This will give the bash name   
- ``echo $BASH_VERSION`` bash version
- ``echo $HOME`` home directory
- ``echo $pwd`` present working directory

## User deined variables
- to define a new variable ``name=Ahmed``
- to use the variable ``echo my name is $name``
- **Note** The variable name should not start with a number

# Cat command in Linux with examples
- It reads data from the file and gives their content as output.

## To view a single file
    
    $cat filename
    
 ## To view contents of a file preceding with line numbers
 
    $cat -n filename
    
 ## Create a file
 
    $cat > newfile

 - It will create a file named newfile
 
 ## Copy the contents of one file to another file
 
    $cat [source-ffilename] > [destination-filename]

## Append the contents of one file to the end of another file

    $cat file1 >> file2
    
## Pass multi-line string to a file in Bash
    
    cat << EOF >> script.py
    from math import factorial
    sum = 0
    for i in range(10000):
        sum += factorial(i)
    print(sum)
    EOF
    
- append this code to a new script named ``script.py``

# time command
- The time command is used to determine how long a given command takes to run.

        $time sleep 3
    
 - ouput:
        
        real    0m3.015s
        user    0m0.009s
        sys     0m0.001s 

- **real** or total or elapsed (wall clock time) is the time from start to finish of the call.
- **user** - amount of CPU time spent in user mode.
- **system** or **sys** - amount of CPU time spent in kernel mode.
