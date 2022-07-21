# Table of content
- [Bash Scripting Inro](https://github.com/Ahmedelsa3eed/Bash-Scripting#bash-scipting-intro-bash-scripting-inro)


# Bash Scipting Intro #{Bash-Scripting-Inro}

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

