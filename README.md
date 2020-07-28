# Shell

This course will take you through the basics of shell.

## Introduction

We generally interact with different I/O devices through GUI provided by our OS. GUI is a easy way exposed by our operating system to interact with it. Shell is another way of interacting with the kernel. A kernel is the heart of an OS.

**Kernel** :The kernel is a computer program at the core of a computer's operating system with complete control over everything in the system. It is the "portion of the operating system code that is always resident in memory". It is an integral part of any operating system. A kernel is the software responsible for taking to the CPU, memory allocation, file system etc. Basically any and every opertion performed by an OS goes through a kernel. The shell provides us with a mechanism to interact with the kernels direcly instead of using some GUI to do it.

GUI -> OS -> Kernel -> | CPU + RAM + File System etc |

There can be 2 kinds of kernel ie monolith and micro kernel.

1. Monolith Kernel : Monolithic kernel is a single large process running entirely in a single address space. It is a single static binary file. All kernel services exist and execute in the kernel address space. The kernel can invoke functions directly. Examples of monolithic kernel based OSs: Unix, Linux.
2. Micro Kernel : In microkernels, the kernel is broken down into separate processes, known as servers. Some of the servers run in kernel space and some run in user-space. All servers are kept separate and run in different address spaces. Servers invoke "services" from each other by sending messages via IPC (Interprocess Communication). This separation has the advantage that if one server fails, other servers can still work efficiently. Examples of microkernel based OSs: Mac OS X and Windows NT.

In this tutorial we are going to be discussing about **BASH** of the **Bourne again shell**. Bash or the bourne again shell is the re-write of unix bourne shell distrubuted under GPS v3 license. Shell can be considered as a program which takes a stream of inputs or commands and performs some task based on those inputs.

**Terminal** : Shell is a program which helps us to interact with the kernel. Terminal is an emulator of the terminal devices which are a legacy now. It just helps us to execute our shell commands or provide input streams to shell. Shell or Bash can also be considered as a language that runs in the terminal.

> Note : To copy something in a terminal press `alt + C` or `option + C` or `Command + C`.
> Note : To stop something in BASH press `control + C`.

When you start a terminal you generaly have a default language/process which is started. Generally it is bash. Run `echo 0$` to find default language in your terminal. You can start running a new language like node/python by typing node or python and a new child process will be started for node or python.

Generlly we have bash or zsh as the default language to run on a terminal. This is because these language are specificlly writen for a command line interface. Whereas language like python/node are designed to be written in a file and then executed all at once as a script.

When you start a terminal you see something written to the left which has some information about the user etc.. This is generaly refered to as a prompt.
![image](https://user-images.githubusercontent.com/46783722/87859682-999f2100-c954-11ea-95e8-ad2f1f3ed814.png)

## Different types of shell

1. **Current Shell** :
   To find out the current shell we can use the **ps** command. It stands for process show and lists down the processes running. This commands list down all the processes in the current shell including the background process. You can see that by running a process in the background of your shell and then using ps. Try running :

   ```shell
   > ping 8.8.8.8 > /tmp &
   > ps
   ```

   If you want to know the current process `ps -p $$` is a better command.

   ![image](https://user-images.githubusercontent.com/46783722/87861344-04575900-c963-11ea-94b5-b86db7a8f173.png)

2. **Switching Shell** : We will try to swith our shell from bash to zsh. Try running :

   ```shell
   > zsh
   > ps -p $$
   > echo $0
   ```

   We don't have to provide the complete path to zsh file since the location of this file is present in seach path or env variables path property. Now to exit the shell we type `> exit`.

   Whenever we start a new shell there are a bunch of things that happen under the hood.

   1. The current bash process forks itself by running the system call fork() and starts a new child process. This new process is a copy of the current bash.
   2. Then a exec() system executes the process to start the zsh. The bash is the parent shell and the zsh is the sub-shell.
   3. When we exit the zsh using exit command the current zsh process is distroyed the control goes back to the parent forked bash process.


      ![image](https://user-images.githubusercontent.com/46783722/87861859-0ec82180-c968-11ea-869f-7cc0b63b17ea.png)

        Notice below that the parent of zsh has an id 559 ie the id of bash.
      ![image](https://user-images.githubusercontent.com/46783722/87862169-c4946f80-c96a-11ea-9d02-2f5022a91de9.png)

      We could also use the `pstree` command to see the tree of process
      ![image](https://user-images.githubusercontent.com/46783722/87862254-babf3c00-c96b-11ea-9b12-010ffe08f6cd.png)

3. **Login Shell** : This is the shell that we get when we log into a system. The login shells are defined in /etc/passwd file. In the image below you can see that the default shell for this user is /bin/bash.
   ![image](https://user-images.githubusercontent.com/46783722/87862448-c7dd2a80-c96d-11ea-9c47-b0b6ad54d3b1.png)

   How to change a login shell for a user :

   1. `su -` : You need to switch to a user with admin privelages.
      ![image](https://user-images.githubusercontent.com/46783722/87862749-0fb18100-c971-11ea-823e-26b28b474093.png)
   2. `vim /etc/passwd` : Open the etc/passwd file.
   3. Change the login shell of you user to **/bin/nologin/**.
      ![image](https://user-images.githubusercontent.com/46783722/87862760-340d5d80-c971-11ea-832c-52bb5f104650.png)
   4. `su [user]` : Try to switch back to you user.
      ![image](https://user-images.githubusercontent.com/46783722/87862768-54d5b300-c971-11ea-8c6e-6adc1147fb08.png)

4. **Virtual Console** :
   Linux provides a number of virtual console that we can connect to. If we talk about a mac then these are similar to the terminal emulators that you open. You can start a new terminal by `Command + N`. This starts a new terminal process and a new user session.
   Try :

   ```shell
   > who
   > tty
   ```

   Now press `Control + N`. This will open a new terminal tab. Now again try the above commads to see the result.

   ![image](https://user-images.githubusercontent.com/46783722/87863282-29ee5d80-c977-11ea-9bcc-85ffe1ccdb6b.png)

## Streams

1. **What are streams?** :
   Stream is a collection of characters. For example the stream of characters we type as command's on the terminal. These stream of characters are input stream since they are provided as an input to the shell and the response printed on the terminal is the output stream since it is the output from the shell.

   A shell has 3 stream's ;

   1. Standard Input(stdin) : Usually connected to the keyboard. It could also be connected to some speech input.
   2. Standard Output(stdout) : Usually connected to screen(terminal). This could be connected to some file system.
   3. Standard Error(stderr) : Usually connected to screen(terminal)

   Almost everything in unix system is a file. So the stdin,stdout and stderr are also handled as files.

   1. stdin : file descriptor **0**.
   2. stdout : file descriptor **1**.
   3. stderr : file descriptor **2**.

2. **Standard Out** :
   Usually the stdout stream is connected the monitor. We can redirect our output stream to some other device or file system.
   We do that by using the `>` symbol.
   Eg `echo 'WARNING!' > /tmp/alertfile` will pring WARNING! in the /tmp/alertfile file of the root directory.

   This is known as redirection and this works for any and every command with an output stream.
   Eg `ls -lh /etc/ > /tmp/lsout`

   1. **Clobbering** : When we redirect a stdout to an already existing file the content of that file gets over written. This is know as clobbering since we are distroying already exiting the data in a file. We can prevent this by setting **noclobber** shell parameter ie `set -o noclobber`. You can also overwrite this setting by using `>|` instead of `>` to redirect the output stream.
      Eg `echo 'WARNING!' >| /tmp/alertfile`. We can also revert the setting by `set +o noclobber`. We can also prevent the clobbering by appending the stdout to the exiting content of the file by using `>>` like `echo 'WARNING!' >> /tmp/alertfile`

3. **Redirection** :
   Everything in unix/linux is a file. Even the devices connected to the system show up as files in the local file system.
   When we run the command `tty` it gives us a file path which is noting but the current terminal file.

   Let do an activity where we will redirect the stdout of one terminal to another. For this follow the below steps

   1. `Control + N` to open a new terminal process in a new tab.
   2. `tty` in both the terminals will give you the terminal file path. Something like `/dev/ttys001`. This file is of character special file, you can check this out by running `file /dev/ttys001`. Copy the path of this file ie /dev/ttys001.
   3. Now in the first terminal use this path to redirect the stdout of a command like `echo 'WARNING!' > /dev/ttys001`
   4. Notice `WARNING!` will be displayed on terminal 2.

   This table show the file descriptor and operator used for redirection. We can redirect using file descritor followed by operator.
   Eg for stdout redirection we can use `echo 'WARNING!' 1> /tmp/alertfile`. By default `>` is `1>` so we don't need to use 1 file descriptor explicitly with greater than operator.
   ![image](https://user-images.githubusercontent.com/46783722/87883819-a476b600-ca27-11ea-81ef-14a4c0bb939a.png)

   We can redirect stderr by using `2>` like `ech 'WARNING!' 2> /tmp/alertfile`.

   As we said before that everthing in linux is a file. The stdin,stdout and stderr are also files in /dev/. You can check that out by using `file /dev/std*` command.

## Shell Environment

## Shell Commands

- **echo** : This is to output something or in other words its the output stream.

  1. `echo 0$` : To find the language running in a terminal or in other words it echo's the name of the process which started this echo command. Here it echoes or outputs the value of variable 0\$.

- **sleep** : This makes the shell to wait some time before going forward to the next command.

  1. `sleep 5 && echo ashish` : Waits for 5 sec and then echo ashish.

- **whoami** : This tells about the current sheel user.

  1. `whoami`
  2. `who am i`

- **ping** : Used to hit an endpoint.

  1. `ping 8.8.8.8` : Reach out to google public dns 8.8.8.8.

- **kill** : Used to kill a process

  1. `kill 7053` : It kills the process with id 7053

- **ps** : It stands for process show and shows the list of process in current shell.

  1. `ps` : Show list of all process in current shell.
  2. `ps -p $$` : -p tell to show process with process id and $$ gives the current process. So $$ is the variable which stores the PID for current process.
  3. `ps -ef | egrep 'bash|zsh'` : Gives a detailed process info for bash and zsh process.

- **which** : Find the path of a binary command file.

  1. `which bash` : Gives the location of the bash file.

- **pstree** : Show the tree of all the running process.

- **cat** : Concatenate multipe data streams into 1.

  1. `cat /etc/passwd`: Will open the passwd file in etc folder.

- **ls** : This is the list command to list files and folder in the currend directory.

  1. `ls -lh /bin/ | grep 'bash'` : Will list all the files containing bash in their name in bin directory.

- **file** : This gives detailed description of the file

  1. `file /bin/bash` : Will show the detailed description of bash file.

- **more** : Used to view text file in command prompt.

- **tty** : Prints the file path of the current terminal connected to standard input.

- **who** : Display's a list of users curretly logged into the system.

- **touch** : To create a file.

  1. `touch test.txt` : Creates a file test.txt in current directory.
