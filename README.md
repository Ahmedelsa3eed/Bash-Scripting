# Table of content
- [Bash Scipting Intro](#Bash-Scipting-Intro)
- [Variables](#Variables)
- [Commands](#Commands)
    - [Cat](#Cat-command-in-Linux-with-examples)
    - [time](#time)
    - [df](#df)
    - [tee](#tee)
    - [heredoc](#heredoc)
    - [sed](#sed)
    - [env](#env)
    - [SCP](#SCP)
    - [Ip](#Ip)
    - [grep](#grep)
    - [wget](#wget)
    - [cp](#cp)
    - [locate](#locate)
    - [systemctl](#systemctl)
    - [scp](#scp)
    - [du](#du)
    - [cut](#cut)
    - [tar](#tar)
- [Text manipulation tools](#text-commands)
- [mysql](#mysql)
- [ssh](#ssh)
- [notes](#notes)
- [siege - An HTTP/HTTPS stress tester](#siege)
- [debug](#debug)
    - [strace](#strace)
    - [redare2](#redare2)

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
    
    cat filename
    
 ## To view contents of a file preceding with line numbers
 
    cat -n filename
    
 ## Create a file
 
    cat > newfile

 - It will create a file named newfile
 
 ## Copy the contents of one file to another file
 
    cat [source-ffilename] > [destination-filename]

## Append the contents of one file to the end of another file

    cat file1 >> file2
    
## Pass multi-line string to a file in Bash
    
    cat << EOF >> script.py
    from math import factorial
    sum = 0
    for i in range(10000):
        sum += factorial(i)
    print(sum)
    EOF
    
- append this code to a new script named ``script.py``

# time
- The time command is used to determine how long a given command takes to run.

        $time sleep 3
    
 - ouput:
        
        real    0m3.015s
        user    0m0.009s
        sys     0m0.001s 

- **real** or total or elapsed (wall clock time) is the time from start to finish of the call.
- **user** - amount of CPU time spent in user mode.
- **system** or **sys** - amount of CPU time spent in kernel mode.

# timeout
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
               
            
# df
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
        
# tee
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
        
        
# Heredoc
- Here document (Heredoc) is a type of redirection that allows you to pass multiple lines of input to a command.
        
        [COMMAND] <<[-] 'DELIMITER'
            HERE-DOCUMENT
        DELIMITER

- The first line starts with an optional command followed by the special redirection operator ``<<`` and the delimiting identifier.
    - You can use any string as a delimiting identifier, the most commonly used are EOF or END.
    - If the delimiting identifier is unquoted, the shell will substitute all variables, commands and special characters before passing the here-document lines to the       command.
    - Appending a minus sign to the redirection operator ``<<-``, will cause all leading tab characters to be ignored. This allows you to use indentation when writing       here-documents in shell scripts. Leading whitespace characters are not allowed, only tab.
- The here-document block can contain strings, variables, commands and any other type of input.    
- The last line ends with the delimiting identifier. White space in front of the delimiter is not allowed.
- Examples:

        cat << EOF
        The current working directory is: $PWD
        You are logged in as: $(whoami)
        EOF
    - output:

            The current working directory is: /home/linuxize
            You are logged in as: linuxize

Let’s see what will happen if we enclose the delimiter in single or double quotes:

        cat <<- "EOF"
        The current working directory is: $PWD
        You are logged in as: $(whoami)
        EOF

You can notice that when the delimiter is quoted no parameter expansion and command substitution is done by the shell.

        The current working directory is: $PWD
        You are logged in as: $(whoami)

- Instead of displaying the output on the screen you can redirect it to a file using the ``>``, ``>>`` operators.
        
        cat << EOF > file.txt
        The current working directory is: $PWD
        You are logged in as: $(whoami)
        EOF

- When using ``>`` the file will be **overwritten**, while the ``>>`` will **append** the output to the file.
- The heredoc input can also be piped. In the following example the [sed](#sed-command) command will replace all instances of the ``l`` character with ``e``:
    
        cat <<'EOF' |  sed 's/l/e/g'
        Hello
        World
        EOF
    - output

            Heeeo
            Wored

- To write the piped data to a file:

        cat <<'EOF' |  sed 's/l/e/g' > file.txt
        Hello
        World
        EOF


# sed
- ``sed`` is a stream editor.
- It can perform basic text manipulation on files and input streams such as pipelines.
- With ``sed``, you can search, find and replace, insert, and delete words and lines.

        sed -i 's/SEARCH_REGEX/REPLACEMENT/g' INPUTFILE

- ``-i`` - By default, ``sed`` writes its output to the standard output. This option tells ``sed`` to edit files in place. If an extension is supplied (ex -i.bak), a backup of the original file is created.
- ``s`` - The substitute command, probably the most used command in ``sed``.
- ``/ / /`` - Delimiter character. It can be any character but usually the slash (``/``) character is used.
- ``SEARCH_REGEX`` - Normal string or a regular expression to search for.
- ``REPLACEMENT`` - The replacement string.
- ``g`` - Global replacement flag. By default, ``sed`` reads the file line by line and changes only the first occurrence of the ``SEARCH_REGEX`` on a line. When the     replacement flag is provided, all occurrences are replaced.
-  ``INPUTFILE`` - The name of the file on which you want to run the command.

# env
- It can print a list of the current environment variables.
- It can run another program in a custom environment without modifying the current one.
- If ``env`` is run without any options, it prints the variables of the current environment.
  Otherwise, ``env`` sets each ``NAME`` to ``VALUE`` and executes ``COMMAND``.
  
 - Options:
    - ``-i``, ``--ignore-environment``: Start with an empty environment.
    - ``-0``, ``--null``: End each output line with a 0 (``null``) byte rather than a newline.
    - ``-u``, ``--unset=NAME``: Remove variable NAME from the environment.

# SCP
- It allows you to securely copy files and directories between two locations.
- Copy file or directory:
    - From your local system to a remote system.
    - From a remote system to your local system.
    - Between two remote systems from your local system.
- Syntax:
    
    scp [OPTION] [user@]SRC_HOST:]file1 [user@]DEST_HOST:]file2

- Local files should be specified using an absolute or relative path, while remote file names should include a user and host specification.
- you must have at least read permissions on the source file and write permission on the target system.
- Examples:

        scp file.txt remote_username@10.10.0.2:/remote/directory
    
- Pass your identity

        scp -i ~/.ssh/private_key.pem ./file.txt remote_username@10.10.0.2:/home/remote_username
    
 
# Ip
- The ip command is a powerful tool for configuring network interfaces that any Linux system administrator should know.  
- It is used to bring interfaces up or down, assign and remove addresses and routes, manage ARP cache, and much more.

**NOTE**: The configurations set with the ip command are not persistent. After a system restart, all changes are lost

**Syntax**:

        ip [ OPTIONS ] OBJECT { COMMAND | help }
        
OBJECT is the object type that you want to manage:
- ``link`` (``l``) - Display and modify network interfaces.
- ``address`` (``a``) - Display and modify IP Addresses.
- ``route`` (``r``) - Display and alter the routing table.
- ``neigh`` (``n``) - Display and manipulate neighbor objects (ARP table).

## Displaying and Modifying IP Addresses

        ip addr [ COMMAND ] ADDRESS dev IFNAME
        
### Display information about all IP addresses

        ip addr show
or

        ip addr
        
If you want to display only ``IPv4`` or ``IPv6`` ip addresses, use ``ip -4 addr`` or ``ip -6 addr``.

### Display information about a single network interface

        ip addr show dev eth0

# grep
``grep`` is a powerful command-line tool that is used to search one or more input files for lines that match a regular expression and writes each matching line to standard output.

## Exclude Words and Patterns
To display only the lines that do not match a search pattern, use the ``-v`` ( or ``--invert-match``) option.
        
        grep -wv nologin /etc/passwd

**NOTE**: The ``-w`` option tells grep to return only those lines where the specified string is a whole word (enclosed by non-word characters).

To specify two or more search patterns, use the ``-e`` option:

        grep -wv -e nologin -e bash /etc/passwd      
Or      

        grep -wv 'nologin\|bash' /etc/passwd
        
**NOTE**: At the basic regular expression where the meta-characters such as ``|`` lose their special meaning, you must use their backslashed versions.

If you use the extended regular expression option ``-E``, then the operator ``|`` should not be escaped, as shown below:

        grep -Ewv 'nologin|bash' /etc/passwd
        
Example:
- The lines where the string ``games`` occur at the very beginning of a line are excluded:

        grep -v "^games" file.txt
        
- To print out all running processes on your system except those running as user “root” you can filter the output of the ``ps`` command:

        ps -ef | grep -wv root
        
## Exclude Directories and Files
To exclude a directory from the search, use the ``--exclude-dir`` option.

Here is an example showing how to search for the string saeed in all files inside the /etc, excluding the /etc/pki directory:
        
        grep -R --exclude-dir=pki saeed /etc


# wget
``wget`` and the ``-m`` flag will download and mirror an entire web site that is referenced. The syntax would be as follows, replacing the URL as desired:

        wget -m [url]

This will download the entire website on your local drive in a directory named the websites URL.

# cp
``cp`` for copying files and directories
- copy the file under a different name

      cp file.txt new_file.txt

# locate
find the files by name
- Search a file with specific name

        locate sample.txt 

# systemctl
- start and stop any service using systemctl

        sudo systemctl start mysql.service
        sudo systemctl stop mysql.service

- restart of reload any service

        sudo systemctl reload mysql.service
        sudo systemctl restart mysql.service

# scp
SCP (secure copy) is a command-line utility that allows you to securely copy files and directories between two locations.
With scp, you can copy a file or directory:

- From your local system to a remote system.
- From a remote system to your local system.
- Between two remote systems from your local system.

## Copy Files and Directories Between Two Systems

        scp file.txt remote_username@10.10.0.2:/remote/directory
Where ``file.txt`` is the name of the file we want to copy, ``remote_username`` is the user on the remote server, ``10.10.0.2`` is the server IP address. The ``/remote/directory`` is the path to the directory you want to copy the file to. If you don’t specify a remote directory, the file will be copied to the remote user home directory.
You will be prompted to enter the user password, and the transfer process will start.

# du
The ``du`` command, short for “disk usage” reports the estimated amount of disk space used by given files or directories. It is practically useful for finding files and directories taking up large amounts of disk space

- When executed without any option du displays the disk usage of the given file or directory and each of its subdirectories in bytes.

        du ~/Documents

- use arguments like ``-h`` to print sizes in human readable format (e.g., 1K 234M 2G), ``-s``, ``--summarize`` display only a total for each argument

        du -sh learn-bash

# cut
``cut`` is a command-line utility that allows you to cut parts of lines from specified files or piped data and print the result to standard output. It can be used to cut parts of a line by delimiter, byte position, and character.

## How to Cut by Field
To specify the fields that should be cut invoke the command with the -f option. When not specified, the default delimiter is “TAB”.

In the examples below, we will use the following file. The fields are separated by tabs.

    245:789 4567    M:4540  Admin   01:10:1980
    535:763 4987    M:3476  Sales   11:04:1978

For example, to display the 1st and the 3rd field you would use:

    cut test.txt -f 1,3

output

    245:789	M:4540
    535:763	M:3476

You can use any single character as a delimiter. In the following example, we are using the space character as a delimiter and printing the 2nd field:
    
    echo "Lorem ipsum dolor sit amet" | cut -d ' ' -f 2

output

    ipsum

# tar
Extract to current directory
To extract an archive into the current working directory, use the “-x” option as shown below.

    tar -xvf documents.tar.gz
    
In this example, we are unzipping or extracting the “documents.tar.gz” file, which contains three text files.

# text-commands
- Sort: sort a file (assuming the contents are ASCII)

        cat test.txt | sort
  
- uniq: filters out the repeated lines in a file

        cat test.txt | uniq
  
- tr: translating or deleting characters
replace all characters from the standard input (linuxize), by mapping the characters from the first set with the matching ones from the second set.
        
        echo 'linuxize' | tr 'lin' 'red'
echo 'linuxize' | tr 'lin' 'red'

        reduxeze
The ``-d`` (``--delete``) option tells tr to delete characters specified in SET1. When deleting characters without squeezing, specify only one set.
The command below will remove ``l``, ``i`` and ``z`` characters:

        echo 'Linuxize' | tr -d 'liz'

The ``L`` character is not deleted because the input includes an uppercase L while the l character in the SET is lowercase.

        Lnuxe

The ``-s`` (``--squeeze-repeats``) option replaces a sequence of repeated occurrences with the character set in the last SET.

In the following example, ``tr`` removes the repeated space characters:

        echo "GNU     \    Linux" | tr -s ' '
        GNU \ Linux

When SET2 is used the sequence of the character specified in SET1 is replaced with SET2.

        echo "GNU     \    Linux" | tr -s ' ' '_'
        GNU_\_Linux

- Cut:
    - Print selected parts of lines from each FILE to standard output.
    - cut parts of a line by byte position, character, and field
examples:
    - print the first three bytes of each line (``-b``, ``--bytes=LIST`` select only these bytes)

          cut -b 1,2,3 filename

    - print the first word and the third word of each line (``-d``, ``--delimiter=DELIM`` use DELIM instead of TAB for field delimiter)
      ``f 1,3``: Specifies the fields (columns) to be extracted.

          cut -d “ ” -f 1,3 filename
      
- Awk: pattern scanning and processing language
    - Executing awk programs
      An awk program can be run in several ways. If the program is short and simple, it can be passed directly to the ``awk`` interpreter on the command-line:

          awk 'program' input-file...
      
      If the program is large and complex, it is best to put it in a file and use the ``-f`` option to pass the file to the ``awk`` command:

          awk -f program-file input-file...

      In the examples below, we will use a file named “teams.txt” that looks like the one below:


          Bucks Milwaukee    60 22 0.732 
          Raptors Toronto    58 24 0.707 
          76ers Philadelphia 51 31 0.622
          Celtics Boston     49 33 0.598
          Pacers Indiana     48 34 0.585

      When the rule has no pattern, each input record is matched. Here is an example of a rule containing only an action:

          awk '{ print $3 }' teams.txt
      
    - Regular expression patterns
      A regular expression or regex is a pattern that matches a set of strings. Awk regular expression patterns are enclosed in slashes (``//``):

          awk '/0.5/ { print $1 }' teams.txt
          awk '/^[0-9][0-9]/ { print $1 }' teams.txt

# mysql
- start mysql service

        service mysql start

- check the status of your mysql service
  
        service mysql status

- create a new database

        create database <DB Name>;

- show all databases

        show databases;

- create a database user with the name ``user`` and the password ``password`` on our localhost server

        create user 'user'@'127.0.0.1' identified by 'password';

- grant the ``user`` privilege all over the ``dvwa`` database.

        grant all privileges on dvwa.* to 'user'@'127.0.0.1' identified by 'password';

# ssh
The following requirements must be met to be able to login into a remote machine via SSH:
An SSH server must be running on the remote machine.
The SSH port must be open in the remote machine firewall.
You must know the username and the password of the remote account. The account needs to have proper privileges for remote login.

        ssh [OPTIONS] [USER@]:HOST
        ssh username@hostname

## generate an SSH key pair,
- use the following command:

        ssh-keygen
  By default, your private and public keys are saved in your ``~/.ssh/id_rsa`` and ``~/.ssh/id_rsa.pub`` files, respectively.

## Sharing keys
- share your public key with the remote machines
- Use the ``ssh-copy-id`` command to copy your public key over to the destination system.

        ssh-copy-id our_username@other_machine
This will take our public key from its default location and transfer it to the other machine

# notes
---
**Handling Dashed Filenames**

To handle filenames that begin with a dash in Linux, you can use one of the following methods:
- Use the ``—`` option: This will tell the shell that the following arguments are not options. For example,

        $ rm — -samplefile.txt
  
- Utilize the full path to the file. Here is an example:

        $ rm /path/to/-samplefile.txt
  
- Use the ``./`` notation for specifying your current directory as shown in this example:

        $ rm ./-samplefile.txt

---

# siege
An HTTP/HTTPS stress tester


        siege -t 155 -c 15 -i http://homer.whoohoo.com/index.html

options:
- ``-t``: allows you to run the test for a selected period of time.
- ``-c``: allows you to set the concurrent number of simulated users.
- ``-i``: generates user simulation by randomly hitting the URLs read from the urls.txt file.


# debug
## strace
trace system calls and signals
- It intercepts and records the system calls which are called by a process and the signals which are received by a process.  The name of each system call, its arguments and its return value are printed on standard error or to the file specified with the -o option.

        strace ./a.out

## redare2
radare2 is a command-line hexadecimal editor.

        redare2 -d ./a.out
