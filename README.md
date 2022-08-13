# Table of content
- [Bash Scipting Intro](#Bash-Scipting-Intro)
- [Variables](#Variables)
- [Cat command in Linux with examples](#Cat-command-in-Linux-with-examples)
- [time command](#time-command)
- [df command](#df-command)
- [tee command](#tee-command)

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

# timeout command
- It is a command-line utility that runs a specified command and terminates it if it is still running after a given period of time.
- The command options must be provided before the arguments.
- If no signal is given, ``timeout`` sends the ``SIGTERM`` signal to the managed command when the  
  time limit is reached. You can specify which signal to send using the ``-s`` (``--signal``) option.
- examples
    - Terminate a command after five seconds:
    
               timeout 5 ping 8.8.8.8
               
    - Terminate a command after five minutes:
    
               timeout 5m ping 8.8.8.8
               
    - send ``SIGKILL`` to the ``ping`` command after one minute

               sudo timeout -s SIGKILL ping 8.8.8.8
               
            
# df command
- You can use the ``df`` command to get a detailed report on the system’s disk space usage.
- For example, to show the space available on the file system mounted to the system root directory (``/``),  
  you can use either ``df /dev/nvme0n1p3`` or ``df /``
- To display information about disk drives in human-readable format (kilobytes, megabytes, gigabytes and so on), invoke the ``df`` command with the ``-h`` option:

        df -h
       
- The ``-T`` option tells ``df`` to display file system types:

        df -t
        
- When invoked with the ``-i`` option, the ``df`` command prints information about the filesystem inodes usage.
- The command below will show information about the inodes on the file system mounted to system root directory ``/`` in human-readable format:

        df -ih /
        
# tee command
- The ``tee`` command reads from the standard input and writes to both standard output and one or more files at the same time.
- The most basic usage of the ``tee`` command is to display the standard output (``stdout``) of a program and write it in a file.
- In the following example, we are using the ``df`` command to get information about the amount of available disk space on the file system.  
  The output is piped to the ``tee`` command, which displays the output to the terminal and writes the same information to the file ``disk_usage.txt``
  
        df -h | tee disk_usage.txt
        
- You can view the content of the ``disk_usage.txt`` file using the [cat command](#Cat-command-in-Linux-with-examples).      
- By default, the ``tee`` command will overwrite the specified file. Use the ``-a`` (``--append``) option to append the output to the file :

        command | tee -a file.out
        
- If you don’t want ``tee`` to write to the standard output, you can redirect it to ``/dev/null``:

        command | tee file.out >/dev/null
        
        
