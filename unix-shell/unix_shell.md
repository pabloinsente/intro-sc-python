# The UNIX Shell

**TL;DR** sections:

[What is the UNIX shel](#what-is-the-unix-shell)

[Why to use the UNIX shell](#why-to-use-the-unix-shell)

[A curated list of basic shell commands](#a-curated-list-of-basic-shell-commands)

The rest of the content is really useful, but not strictly required to use the shell.

**NOTE 1**: Apologies for typos and misspelled words in advance. This document has not been proofread (yet).

**NOTE 2**: There is a GitHub repository ([here](https://github.com/pabloinsente/intro-sc-python)) associated with this tutorial that you can clone to follow along. You should also be able to recreate the contents of this tutorial in your own machine by simply typing everythin in the shell. If you are not comfortable using Git/Github, you still can read this as a conceptual introduction with examples.

**NOTE FOR WINDOWS USERS**: As Windows is not an Unix-like or Linux-based system, most the commands and examples here won't work, as the Windows Command Prompt and the Windows Power Shell are not bash based. You have a couple of options to follow along:

1. downloading and installing terminal emulators like [GitBash](https://gitforwindows.org/) and [Cygwin](https://www.cygwin.com/)
2. to install the [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
3. open the GitHub repository associated with this tutorial in the cloud environment provided by **MyBinder** by clicking the icon below. Once the environment is ready (it may take a couple of minutes to build), open a terminal there. To open a terminal simply go to "File -> New -> Terminal" or click on the "Terminal" icon under the "Other" section in the landing page. The file with this tutorial is in  `unix-shell/` directory named as **unix_shell.md**

**To open MyBinder** -> [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/pabloinsente/intro-sc-python/master/?urlpath=lab)

If you are a beginner, [GitBash](https://gitforwindows.org/) and [Cygwin](https://www.cygwin.com/) should work just fine, and easier to set-up. The MyBinder environment is a good option too, but you won't be able to save your work. I do not advise trying (WSL) unless you feel comfortable with using the terminal already. Yet, WSL is the best long-term solution for Windows users.

## Table of contents

- [What is the UNIX shel](#what-is-the-unix-shell)
- [Why to use the UNIX shell](#why-to-use-the-unix-shell)
- [Shell syntax basics](#shell-syntax-basics)
- [Standard input, output, and error](#standard-input-output-and-error)
- [Shell commands basics](#shell-commands-basics)
  - [Single commands](#single-commands)
  - [Composed commands](#composed-commands)
- [A curated list of basic shell commands](#a-curated-list-of-basic-shell-commands)
  - [Basic commands](#basic-commands)
  - [File commands](#file-commands)
  - [Directory commands](#directory-commands)
  - [System commands](#system-commands)

## What is the UNIX shell

We begin our journey with the UNIX shell, that cryptic program that runs in your terminal enabling you to do all sort of tasks in your computer. The UNIX shell is a program to *interface* with the lowest level of UNIX-based operating systems (i.e., the *kernel*). If you are running any Mac OS or Linux Distribution, you are using a *UNIX-based* or *Unix-like* operating system. UNIX-based operating systems have two main parts: the *kernel* and the *utilities*.  The *kernel* is the program managing and allocating the resources of the computer hardware (i.e., the Central Processing Unit or CPU, the Random Access Memory or RAM, and devices like the mouse, speaker, etc). It is a *software layer* facilitating the control of the computer hardware. The *utilities* are a set of commands to interface with the kernel. For instance, if you type `pwd` in your terminal, the kernel will load a program called `pwd` into the RAM, read the program instructions, and display the output, in this case, the current working directory path.

The so-called *shell*, also happens to be a UNIX utility program. It has a dual identity: as a *user interface* to the UNIX utilities, and as a *programming language* facilitating the usage and combination of the UNIX utilities. When you open the terminal, the shell program is loaded into the computer memory. When you type commands in the terminal, the shell reads the commands and converts them into a format that is readable by the kernel to be executed. It provides an interactive instance to start programs, manage files, and processes running in the computer. Since the shell is just a program, many variations have been created since 1969, when [Ken Thompson](https://en.wikipedia.org/wiki/Ken_Thompson) developed the the first UNIX implementation at [Bell Labs](https://en.wikipedia.org/wiki/Bell_Labs). The original UNIX shell was written by [Steve Bourne](https://en.wikipedia.org/wiki/Stephen_R._Bourne) in 1970, and it's known as *Bourne shell* or *sh*. The Bourne shell was not available for free at the time, which limited its usage by other programmers. To alleviate this problem, in 1988, the Free Software Foundation tasked [Brian Fox](https://en.wikipedia.org/wiki/Brian_Fox_(computer_programmer)) to develop an open-source reimplementation of the Bourne shell, the so-called *Bourne again shell* or *bash*. Today, the *bash* shell is probably the most widely use implementation of the Unix shell, and the one that serves as a base for us.

## Why to use the UNIX shell

If you haven't use the shell before, you're probably accustomed to interact with computer software via *Graphical User Interfaces* or a *GUI*. This is perfectly fine for most day to day task, but in a research context, there are many important capabilities that GUI interfaces do not provide. In particular, there are a few key capabilities that I want to highlight:

- **Repetition**: there are situations when you want to repeat the same action multiple times, sometimes hundreds or thousands of times, actions like changing the extension of a large batch of files or extracting the last line of multiple text files.  Repeating these actions thousands of times with a GUI is beyond unpractical (and probably bad for your health too), and here is where the shell thrives. For example, changing thousands of files with a *.txt* extension to a *.md* extension can be accomplished with a single line like this one:

```bash
➜ rename "s/txt/md/" *.txt
```

***HEADS UP***: In the code blocks, you see will see a  `➜` as a  *prompt* before the actual command. Anything below that indicates the output printed to the terminal. Some commands do not print to the terminal. For instance:

```bash
# With standard output to the terminal
➜ [shell-command] [ARGUMENT(s)]
some standard output

# Without standard output to the terminal
➜ [shell-command] [ARGUMENT(s)]
```

If you copy the instructions to your terminal, remember to delete or ignore the ➜ symbol. Your terminal may have **$** symbol or some other character as a prompt, indicating that the terminal is ready to receive input.  

- **Automation**: sometimes, instead of repeating the same action, you may want to repeat sequences of actions, or maybe just a single long and complicated action. You may also need to trigger an action automatically in response to some process in your computer. In either case, using the GUI makes you more error prone and slower. Writing the instructions in a *shell script*, a text file with sequences of shell commands, can facilitate these tasks.

- **Reproducibility**: the fact that you can type sequences of instructions in shell scripts, makes extremely easy to *exactly* reproduce steps in data processing pipelines. Alternatively, you could take snapshots of your GUI and provide lengthy instructions of what to point and click at every step, but that would take more effort, more time, and increase the probability of error.

- **Remote server connection**: if you ever need to connect to another computer from your computer, this is, a *remote server*, you will have to use the shell. For instance, *High-performance computing* (HPC),  *High-throughput computing* (HTC), and *Amazon Web Services* (AWS), are all forms of remote computing that require the users to connect and interact via shell commands.

## Shell syntax basics

When you pass commands to the terminal, *roughly* speaking, the shell performs the following set of operations:

- If you pass starting with the '#' symbol, the shell will ignore that as a '*comment*' (i.e., it won't do nothing). Comments are usually used in shell scripts rather than in interactive mode, as descriptors of the action to be taken.

- If you pass commands without the '#' symbol, the shell reads the inputs and divides them into '*words*' and '*operators*'. By *words* we mean any commands like `cd` (change directory) or `cat` (concatenate files to standard output), plus other constructs related to the specific command; by *operators* we mean arithmetic operators (like `+` or `-`) , relational operators (like `-eq` or `-ne`), boolean operators (like `-o` or `!`), string operators(like `!=` or `=`), or file operators (like `-b` or `c`).

- Then, the shell parse the words and operators into subtypes, performs a series of intermediate steps like expansions and redirections, to finally execute the commands instructions.

## Standard input, output, and error

Before moving into more applied topics, I want to briefly review the concepts of standard input, standard output, and standard error, as I will use them constantly in this tutorial.

Linux or unix-like systems have what is known as "standard streams of data". Any process run in such systems is initialized with three data streams: *standard input*, *standard ouput*, and *standard error*. By data we mean instructions in plain text formart.

**Standard input** or "stdin", referes the "place" where programs or processes *get* information from. By default, the shell "takes" input from the keyboard. In other words, standard input is the default place and source of information for Linux/Bash programs.

**Standard output** or "stdout", referes the "place" where programs or processes *send* information to. By default, the shell output will be directed to the screen or monitor (i.e., printed in the terminal), but it can also for to a text file or a printer. In other words, standard outut is the default place where information is send after processing.

**Standard error** or "stderr", referes the "place" where programs or processes *send* errors. By default, the shell output will be directed to the screen or the monitor(i.e., printed in the terminal). In other words, standard error is the default place where the shell send messages about processes that went wrong.

Knowing this concepts will make undertanding bash documentation much easier.

## Shell commands basics

Broadly speaking, there two types of shell commands: *single* commands and *composed* commands.

### Single commands

Single commands are a combination of the command itself, a blank space, a the command arguments. For instance:

```bash
➜ ls -a
```

Here we have:

- `ls` : the command to list information about files
- A white space
- `-a` : an argument saying "do not ignore files starting with ." (hidden files).

### Composed commands

Composed commands are created by combining simple commands into *pipelines*, *lists*, *compounds*, and *coproceses*. We will examine the first three, as are the ones more commonly used.

#### Pipeline

A pipeline is a sequence of one or more commands separated by one of the control operators ‘|’ or ‘|&’, where the *output* of the first command becomes the *input* of the next. For instance:

```bash
➜ ls -l | grep ".txt"
```

The `ls` command list the files in the current directory, and the `grep` command will print the lines matching the ".txt" extension to the terminal.

#### Lists

Lists are a sequence of one or more pipelines separated by one of the operators ‘;’, ‘&’, ‘&&’, or ‘||’. For instance:

```bash
➜ ls | grep ".txt" && ls | grep ".csv"
```

The pipeline on the left side of *&&* will print the files matching the ".txt" , and the pipeline on the right side, will print the files matching the ".csv" extension, only if the left side was executed successfully. 

#### Compound commands

Compound commands are shell programming constructs that allow for more complex operations, particularly related to control flow. Compound commands are further divided into: looping constructs, conditional constructs, and grouping constructs. This type of commands begin with a reserved keyword indicating the beginning of the processes, and another keyword to indicate the end. For instance:

```bash
➜ while test-commands; do consequent-commands; done
```

This will tell the shell to repeat certain action (*consequent-commands*), while some other condition is true (*test-commands*). The `while` keyword indicates the beginning, and the `done` keyword indicate the end. 

## A curated list of basic shell commands

Now that we have enough background knowledge about the inner workings of the shell, we will review a list of the, in my opinion, most useful commands in a research context. This is the most important and practical part of this tutorial. I don't pretend to be exhaustive here. More commands will be introduced in later sections.

***HEADS UP***: In case you use the GitHub repository to follow the examples, be aware the exact output showed here may be different, given that this is a project in constant development. However, the instructions and command description hold true.

### Basic commands

#### `echo`

The `echo` command display text in the terminal. Is often used in combination with other operators to pass information to a file.

##### Simple display

```bash
➜ echo this is my first command
this is my first command
```

##### Combining to pass text to a file

This line will pass the text into the file, instead of printing in the terminal

```bash
➜ echo this is my first command >> first-command.txt
```

#### `clear`

The `clear` command clears the terminal. It is often the case the your terminal will get cluttered with information, which can make things confusing to work with, so clearing often it is useful.

```bash
➜ clear
```

#### `man`

The `man` command allows you to access the on-line reference manual in the terminal. Any time that you want to learn anything about any command, `man` will help you.

The basic syntax is `man [NAME OF THE COMMAND]`.

For instance you can learn about the `man` command itself by

```bash
➜ man man
MAN(1)                        Manual pager utils                        MAN(1)

NAME
       man - an interface to the on-line reference manuals

...
```

#### `help`

Technically, `help` is not a command, but an option for most commands. I'm including this here because along with `man` is one of the most handy tools to learn about and use bash. You can think on `help` as quicker way to look at the documentation.

For instance, to learn more about  `man`

```bash
➜ man --help
Usage: man [OPTION...] [SECTION] PAGE...

  -C, --config-file=FILE     use this user configuration file
  -d, --debug                emit debugging messages
  -D, --default              reset all options to their default values
      --warnings[=WARNINGS]  enable warnings from groff
...
```

### File commands

There are some operations that you can perform with files, like text files, comma separated files, and others.

#### `ls`

The `ls` command *list directory contents*. 

The syntax is: `ls [OPTION(s)] [FILE(s)]`

##### List directory contents

The simples action is to list the contents in the current directory by `ls`

```bash
➜ ls
a-folder  characters-folder        got-characters.txt           __init__.py
b-folder  got-characters-copy.txt  harry-potter-characters.txt  unix_shell.md
```

##### List directory contents including files starting with '.'

Hidden files usually begin with a dot in UNIX-bases systems. To list them along with the visible files you need to add the `-a` option. Here I'm adding `./characters-folder` to list the contents in that directory instead of the current one.

````bash
➜ ls -a ./characters-folder
.   got-characters-copy.txt  harry-potter-characters.txt
..  got-characters.txt       .hidden-file.txt
````

##### List directory contents in long format

Listing in long format reveals detailed information about each file. To do this we add the `-l` option

```bash
➜ ls -l
drwxr-xr-x 3 pablo pablo  4096 Feb  8 15:33 b-folder
drwxr-xr-x 2 pablo pablo  4096 Feb  8 15:40 characters-folder
-rw-r--r-- 1 pablo pablo    18 Feb  8 15:23 got-characters-copy.txt
-rw-rw-r-- 1 pablo pablo    18 Feb  8 12:53 got-characters.txt
-rw-r--r-- 1 pablo pablo    19 Feb  8 15:06 harry-potter-characters.txt
-rw-r--r-- 1 pablo pablo     0 Feb  8 12:43 __init__.py
-rw-r--r-- 1 pablo pablo 11515 Feb  8 15:46 unix_shell.md
```

There is a lot of information here. Let's examine this part by part:

- The first character indicates the [file type](https://en.wikipedia.org/wiki/Unix_file_types). In this example is `d` for directory or `-` for regular file. There are more types that you can look up on-line.
- The next nine characters indicate file permissions.
  - *Characters 1-3*, are for the *user*. Here `-rw` means *reading* and *writing* permissions
  - *Characters 4-6* are for the *group*. Here `-r-` means *reading* permission only
  - Characters 7-9 are for *others*. Here `-r-` means *reading* permission only
- The number after the permission string indicates the the number of [hard links](https://en.wikipedia.org/wiki/Hard_link) to the file
- Next, the `pablo pablo` indicates the *file owner* and the *group*, respectively.
- The number after the group name, indicates the file size in bytes.
- Next, the `Feb  8 15:33` indicates the *date* and *time* of the *last modification* to the file.
- Finally, the last column display the file name.

##### List directory contents by custom order

The `ls` command will sort files alphabetically by default.  You can use the `--sort=KEYWORD` to sort files by *size*, *time*, *version*, *extension*.

```bash
# sort by size
➜ ls -l --sort=size
total 40
-rw-r--r-- 1 pablo pablo 13396 Feb  8 16:09 unix_shell.md
drwxr-xr-x 2 pablo pablo  4096 Feb  8 15:25 a-folder
drwxr-xr-x 3 pablo pablo  4096 Feb  8 15:33 b-folder
drwxr-xr-x 2 pablo pablo  4096 Feb  8 15:40 characters-folder
-rw-r--r-- 1 pablo pablo    19 Feb  8 15:06 harry-potter-characters.txt
-rw-r--r-- 1 pablo pablo    18 Feb  8 15:23 got-characters-copy.txt
-rw-rw-r-- 1 pablo pablo    18 Feb  8 12:53 got-characters.txt
-rw-r--r-- 1 pablo pablo     0 Feb  8 12:43 __init__.py
# sor by time
➜ ls -l --sort=time
total 40
-rw-r--r-- 1 pablo pablo 13396 Feb  8 16:09 unix_shell.md
drwxr-xr-x 2 pablo pablo  4096 Feb  8 15:40 characters-folder
drwxr-xr-x 3 pablo pablo  4096 Feb  8 15:33 b-folder
drwxr-xr-x 2 pablo pablo  4096 Feb  8 15:25 a-folder
-rw-r--r-- 1 pablo pablo    18 Feb  8 15:23 got-characters-copy.txt
-rw-r--r-- 1 pablo pablo    19 Feb  8 15:06 harry-potter-characters.txt
-rw-rw-r-- 1 pablo pablo    18 Feb  8 12:53 got-characters.txt
-rw-r--r-- 1 pablo pablo     0 Feb  8 12:43 __init__.py
```

#### `cat`

The `cat` command *concatenates and print contents of a file to standard output* (I know, confusingly, it has nothing to do with cats).

The syntax is: `cat [OPTION(s)] [FILE(s)]`

##### Display file's contents

```bash
➜ cat harry-potter-characters.txt
Harry
Hermione
Ron
```

##### Display multiple file's contents

```bash
➜ cat got-characters.txt
Harry
Hermione
Ron
Jon
Arya
Daenerys
```

##### Display numerated file's contents 

```bash
➜ cat -n got-characters.txt
1	Jon
2	Arya
3	Daenerys
```

##### Create a new file concatenating standard input text

This will prompt you to enter input text. You type some text and press `enter`. Once you are done, press `Control + D` to exit.

```bash
➜ cat >new-characters.txt
Dumbledore
Voldemort
Jaime
Tyrion
```

Now, if you type `cat new-characters.txt` the list of names will be printed in the terminal.

#### `cp`

The `cp` command *copy files and directories*.

The syntax is: `cp [OPTION(s)][SOURCE] [DESTINY]`

##### Copy contents of a file into another

```bash
cp got-characters.txt got-characters-copy.txt
```

If you `cat` the contents of `got-characters-copy.txt` you will see the duplicated contents

##### Copy a file from one directory into another

```bash
cp got-characters.txt ./a-folder
```

##### Copy a directory into another

```bash
cp -r /a-foler ./b-folder
```

The `-r` option stand for *recursive*. This option is required to copy directories.

#### `touch`

The `touch` command changes files timestamps. However, `touch` is commonly used to create new empty files. 

To change timestamps of an *existing* *file* the syntax is `touch [OPTION(s)] [FILE(s)]`

To create an *new empty file* the syntax is `touch [FILE(s)]` 

##### Change the time-stamp of an existing file

First, I'll review the access and modification times using the `stat` command, to then use the `touch` command to change those times, and finally `stat` again to verify the changes.

```bash
# review access time and modification time
➜ stat harry-potter-characters.txt 
  File: harry-potter-characters.txt
  Size: 19        	Blocks: 8          IO Block: 4096   regular file
Device: 10307h/66311d	Inode: 3440036     Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/   pablo)   Gid: ( 1000/   pablo)
Access: 2020-02-08 15:39:41.039475209 -0600
Modify: 2020-02-08 15:06:40.426141133 -0600
Change: 2020-02-08 15:06:40.426141133 -0600
 Birth: -

# change access and modficiation time
touch -am harry-potter-characters.txt 

# review access time and modification time after change
➜ stat harry-potter-characters.txt     
  File: harry-potter-characters.txt
  Size: 19        	Blocks: 8          IO Block: 4096   regular file
Device: 10307h/66311d	Inode: 3440036     Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/   pablo)   Gid: ( 1000/   pablo)
Access: 2020-02-08 17:37:05.054635614 -0600
Modify: 2020-02-08 17:37:05.054635614 -0600
Change: 2020-02-08 17:37:05.054635614 -0600
 Birth: -
```

Here, the `-a` and `-m` options change the *access time* and *modification time*, respectively. If you compare the `Access` and `Modify` entries from the top and the bottom of the code block, you'll see the changes. 

##### Create a new empty file

```bash
➜ touch my-empty-file.txt
```

This is the most common use of the touch command. You can create multiple empty files by:

```bash
➜ touch my-empty-file-1.txt my-empty-file-2.txt my-empty-file-3.txt
```

#### `mv`

The `mv` command moves files around your file system, and it is also used to rename files.

The syntax to move files is: `mv [OPTION(s)]]  [SOURCE] [DESTINY]`

The syntax to rename a file is: `mv [OPTION(s)] [CURRENT NAME] [WANTED NAME]`

##### Moving a file

```bash
➜ mv my-empty-file.txt ./a-folder
```

Notice that here you can use either absolute or relative paths.

##### Renaming a file

```bash
➜ mv my-empty-file-3.txt my-empty-file-4.txt
```

#### `grep`

The `grep` command print lines matching a desired pattern given some standard input. By standard input, we mean content printed to the terminal or a file. This is one of the most handy command line tools in bash, particularly when combined with other tools like `ls` , `find`, and [regular expressions]('https://en.wikipedia.org/wiki/Regular_expression'). The `grep` documentation is very extensive, and it can be treated as a topic in itself one combined with regular expression. Here we will review a few of the main `grep` functions.

The syntax for basic pattern matching is `grep [OPTION(s)] [PATTERN] [FILE(s)]`

The syntax for pattern matching as extended regular expressions is `grep [OPTION(s)] -e [PATTERN] [FILE(s)]`

The syntax for pattern matching using the `[PATTERN]` as a list of fixed string (instead of regular expression) is  `grep [OPTION(s)] -f [STRING] [FILE(s)]`

##### Basic pattern matching

```bash
➜ grep Harry harry-potter-characters.txt 
Harry
➜ grep Jon got-characters.txt 
Jon
```

##### Extended regular expression pattern matching

In Linux, there is no difference between basic and extended pattern matching. In other system, it may be the case that basic pattern matching is not as general and powerful as extended pattern matching. If you are in need of using complex regular expression patterns to search, you may need to add the `e` option for that to work. I'll not cover this option now since regular expressions knowledge is required.

##### String pattern matching

The difference between basic pattern matching and string pattern matching, is that the latter does not use regular expression rules to search. For instance, suppose you want to search something like `.*` in a file:

```bash
# pattern matching with regular expressions
➜ grep "\.\*" filename
# pattern matching with strings
➜ grep -F ".*" filename
```

In the pattern matching case you have to add the backslash [escape character](https://en.wikipedia.org/wiki/Escape_character) to prevent the interpretation of the `*` as a [wildcard character](https://en.wikipedia.org/wiki/Wildcard_character). In the string search case, the `.*` is interpreted literally, so no escape character is needed.

##### Using the output of another command as `grep` input

The `grep` command is often used in combination with others bash tools. For instance, we can list all the files in the current directory with `ls`, then pass the output as input to `grep`, and search for all files containing the string "got".

```bash
➜ ls | grep got                    
got-characters-copy.txt
got-characters.txt
```

`wc`

The `wc` command prints how many new lines, words, and bytes are in  file.

The basic syntax is `wc [OPTION(s)] [FILE]`

##### Basic count

```bash
➜ wc got-characters.txt
3  3 18 got-characters.txt
```

In order, this is 3 words, 3 new lines, and 18 bytes.

##### Count words only

```bash
➜ wc -w got-characters.txt
3 got-characters.txt
```

##### Count new lines only

```bash
➜ wc -w got-characters.txt
3 got-characters.txt
```

##### Count the number of characters

```bash
➜ wc -m got-characters.txt
18 got-characters.txt
```

##### Count the number of bytes

```bash
➜ wc -c got-characters.txt
18 got-characters.txt
```

#### `head`

The `head` command prints the first elements of a file, by default, the first 10 elements.

The basic syntax is `head [OPTION] [FILE]`

##### Printing the first 10 lines of a file

```bash
➜ head fruits.txt
apple
pear
peach
grape
kiwi
melon
fig
cucumber
cherry
banana
```

##### Printing the first n number of lines of a file

You can print an arbitrary number `n` of lines in a file by using th `n` option.

```bash
➜ head -n 5 fruits.txt
apple
pear
peach
grape
kiwi
```

##### Printing the first n number of bytes of a file

You can print an arbitrary number `n` of bytes in a file by using th `-c` option.

```bash
➜ head -c 20 fruits.txt
apple
pear
peach
```

##### `tail`

The `tail` command prints the last elements of a file, by default, the last 10 elements.

The basic syntax is `tail [OPTION] [FILE]`

##### Printing the last 10 lines of a file

```bash
➜ tail fruits.txt
cucumber
cherry
banana
avocado
coconut
orange
papaya
watermelon
mango
```

##### Printing the last n lines of a file

```bash
➜ tail -n 5 fruits.txt
coconut
orange
papaya
watermelon
mango
```

##### Printing the last n number of bytes of a file

You can print an arbitrary number `n` of bytes in a file by using th `-c` option.

```bash
➜ tail -c 20 fruits.txt
ya
watermelon
mango
```

#### `rm`

The `rm` command removes files or directories. This another very important and useful shell tool.

The basic syntax is `rm [OPTION(s)] [FILE]`

##### Removing a single file

```bash
➜ rm my-empty-file-4.txt
```

You won't see any printed output but you can check the file has been removed with `ls`.

##### Removing multiple files

```bash
➜ rm my-empty-file-1.txt my-empty-file-2.txt
```

##### Removing a directory

`rm` alone won't work this time. The `-r` flag ("recursively") must be added.

```bash
# create an empty folder as example
➜ mkdir empty-dir
➜ rm -r empt-dir
```

### Directory commands

Directory commands are actions you can perform with folders or directories.

#### pwd

The `pwd` prints the current workind directory to the terminal.

The basic syntax is `pwd [OPTION(s)]`

```bash
# you will see your own full path printed out to the terminal
➜ pwd
/mnt/c/Users/pablo/Desktop/projects/unix_shell
```

#### cd

The `cd` command changes the current working directory to another.

The basic syntax is `cd [OPTION(s)] [DIRECTORY]`

##### Move to an specific folder "down"

```bash
# move from current directory to characrers-folder/
➜ cd characters-folder/
```

##### Move one folder up

```bash
# move back from characrers-folder/ to unix_shell/
➜ cd ..
```

##### Move two folders up

```bash
# go back to unix_shell/ first
➜ cd unix_shell/
# move back from unix_shell/ to repo-directory
➜ cd ../..
```

You can follow the patten of `../` to move several directories up.

##### Move the home directory

```bash
# this is no mistake, cd alone would change the directory to home
➜ cd
```

#### mkdir

The `mkdir` command creates a new directory.

The basic syntax is `mkdir [OPTION(s)] [DIRECTORY-NAME]`

##### Make a single directory

```bash
➜ mkdir my-folder
```

##### Make multiple directories

```bash
➜ mkdir my-folder-2 my-folder-3
```

You can create as many directories as you need "below" the current working directory by following the same patter

### System commands

#### ps

The `ps` command prints a snapshot of the corrent active processes to the terminal. This command has many options which are useful for system administrators and othes. For our purposes, just knowing `ps` does that is enough.

The basic syntax is `ps [OPTION(s)]`

```bash
# your output will differ depending on your active processes
➜ ps
PID   TTY          TIME CMD
  8   pts/0    00:00:01 zsh
 1042 pts/0    00:00:00 ps
```

#### kill

The `kill` command terminates an active process.

The basic syntax is `kill PID` or `kill -s signalName PID`

The PID is the process identification number. You can find the PID of a process with the `ps` command.

```bash
kill PID
```

Here is an example of when I have used `kill`: I closed a Jupyter Notebook session by typying `Ctrl` + `Z` in the terminal, instead of `Ctrl` + `C`. Sometimes happens that if you try launch Jupyter again won't work because the default port is in use. Then you have to terminate the process Jupyter process, which is still running in the background, to be able to launch Jupyter again.

#### top

#### whoami

The `whoami` command prints the current user id to the terminal.

The basic syntax is `whoami [OPTION's]`

```bash
# here you will see your used id
➜ whoami
pablo
```

### Input/Output redirection commands

As I explained before, the unix shell has three streams of data: input, output, and error messages. Such information can be redirected before execution. An example is the pipeline `|` that we reviewd in the composed commands section.

#### Redirection operators

Redirection operators are special characters used for change the direction in which streams of data flow. They can be located before or in between commands. Here is a list of several common operators.

##### Redirecting input

To redirect input we use the `<` operator as:

```bash
➜ cat < got-characters.txt
Jon
Arya
Daenerys
```

The `cat` command is "fed" with the text file as input.

##### Redirecting output

To redirect output we use the `>` operator as:

```bash
➜ echo "catbug" > catbug.txt
```

This will cause the outut of `echo "catbug"` to be directed towards the catbug.txt. You can check that printing the catbug.txt contents.

##### Appending redirected output

To append output we use the `>>` operator as:

```bash
➜ echo "Plum" >> catbug.txt
```

This will cause to append (i.e., print at the end) the output of `echo "Plum"` , to the  catbug.txt file. If you use `>` instead, the contents will be replaced rather than appended. This is you will get only:

```bash
Plum
```

Instead of:

```bash
catbug
Plum
```

##### Redirecting standard output and error

There are instances where in addition to the output of a process, you may want to print any error messages generated for something that went wrong. An example of this is running long processes, which may take hours, which you leave unattended. if something goes wrong while you are away, having the error messages printed to a text file may be very useful to fix your code later.

This action is done with the `&>` operator as:

```bash
cat nonexistent-file.txt &> empty.txt
```

The empty.txt file now must contain the "cat: nonexistent-file.txt: No such file or directory" message. If you use `>` instead, the error message will be printed to the terminal, and the empty.txt file will be empty.

[back to top](#the-unix-shell)

## Future sections

This tutorial is not complete. I was not planning to release this yet, but it became necessary to help out some students to learn shell basics. These are the topics I hope to cover later.

- Shell functions basics
- Shell variables basics
- Shell flow control basics
- Shell pattern matching basics
- Shell scripting basics
- Alternative shells
- Additional resources to learn

[back to top](#the-unix-shell)