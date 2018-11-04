#### COMP 206 Fall 2018

# Table of contents
1. [Operating system](#operating-system)
2. [Linux](#linux)
3. [Bash](#bash)
4. [C](#c)
    1. [Pointers](#pointers)
    2. [2D arrays](#2d-arrays)
5. [Memory](#memory)
6. [BMP images](#bmp-images)

# Operating system

#### Definition
- software that supports a computer's basic functions
  - ie. scheduling tasks, executing applications, controlling peripherals
- OS has "control" over application programs by deciding when they start/stop, which users can access, which resources can be accessed (files, network, etc)
- OS must launch every other application

## Computer hardware to OS

### The beginning: hooting up
- first device to get power is a BIOS or firmware interface
- BIOS is a special piece of hardware that manages the lowest level
- BIOS selects which code to start running next, based on a priority over bootable media (hard drives, USB keys, DVD, the network) 

### The middle: a boot loader
- a disk is bootable if it has a particular sequence of data at its very beginning, the Master Boot Record (MBR)
- BIOS reads each device in order until it finds one that's indicated "bootable"
- BIOS can load an OS and begin its execution, but very often there's a small piece of software known as boot loader indicated in MBR
  - a common boot loader for use with Linux is called **GNU GRand Unified Bootloader (GRUB)**
- boot loader can provide some more flexibility and a nice interface to let you choose an OS, which the boot loader then executes

### The end (of the beginning)
- the **Kernel** is the part of the OS that runs first and always
- Kernel is responsible for loading and running all other programs
- Kernel gets to create the spec of what's allowed, enforce security, kill other jobs, provide access to hardware, etc.

# Linux

## What is Linux?
- an operating system based on the Linux kernel written by Linux Torvalds in 1991
  - Linux kernel was a reimplementation of Unix designed from the start to be **free and open source**
- Linux is hard to define because some people have made new versions from this kernel that others don't agree on
- Linux is still developed by Linux Torvalds, along with the worldwide community

## Unix/Linux philosophy
- multiuser/multitasking
- toolbox approach
- flexibility/freedom
- conciseness
- *everything* is a file
- file system has places; processes have life
- designed by programmers, for programmers

## Unix OS components

**Kernel**
- login
- task switching, multi-processing
- basic interface with user and programs
- drivers, run-time stack, heap

**File system**
- defines the way the disk drive is formatted
- file allocation table (FAT)
- data structure on disk that makes files feel *real*
- reading/writing to disk and peripherals
- user commands to interact with files

**Shell**
- a more "advanced" user interface
- has a global memory
- scriptable to produce complex behaviour

**Utilities**
- additional OS commands and programs
- third party commands and programs
- drivers

## Shell as a user interface
- a shell is a command **interpreter** turns text that you type (at the command line) into actions:
  - **runs a program (aka command)**
  - allows you to edit a command line
  - opens the door for complex ways of chaining commands into larger programs, scripting, etc.

## File system: an interface to the disk
- `bin`: `sh`, `date`, `csh`
- `etc`: `passwd`, `group`
- `lib`: `libc.so`
- `usr`: `bin`, `man`, `local`
  - `local`: `bin`, `man`, `src`
- `dev`: `ttya`, `null`
- `tmp`
- `home`: `frank`, `linkdadb`, `rfunk`
  - `lindadb`: `mail`, `bin`, `src`

## Commands to know
- `ls`: list files
- `cd`: change directory
- `pwd`: where am I now? (present working directory)
- `mv`: move files or directories
- `find`: search for files with given properties
- `chmod`: change permissions
- `cp`: copy files or directories
- `cat`: concatenate input files
- `echo`: copy input to output
- `grep`: filter input based on a pattern
- `tr`: translate inputs to outputs
- `sort`: sort inputs, then output
- `ps`: display running processes (once)
- `top`: display the running processes (continuous) and resource usage
- `uname`: print system info (which Linux version)
- `ssh`: remotely connect to another computer

## Running a program
- type the program's name, followed by command line args
- ie.  `$ ls -l /bin`
  - program name: `ls`
  - first arg: `-l`
  - second arg: `/bin`

## Defaults for I/O
- when a shell runs a program for you:
  - standard input is your keyboard
  - standard output is your screen/window
  - standard error is your screen/window

### Terminating standard input
- if standard input is your keyboard, you can type stuff in that goes to a program
- to end the input, type ctrl+D on a line by itself, ending the input *stream*
- shell is a program that reads from stdin

### Input redirection
- shell can attach things other than your keyboard to stdin
- a file, using the "<" operator:
  - ie. `$ grep pattern < search_file.txt`
    - the contents of the file are fed to a program as if you typed it
  - ie. `$ sort < nums.txt`
    - sort the lines in the file nums and send the result to stdout
- the output of another commands, using the "|" operator:
  - ie. `$ ls | grep`
    - the output of another program is fed as input as if you typed it
    - both programs run simultaneously to continue transmitting info over their "pipe"

### Output redirection
- shell can attach things other than your screen to stdout (or stderr)
- a file using the ">" operator:
  - ie. `$ ls > file_info.txt"
    - the output of a program is stored in file
- the input of another program, using a pipe as we have seen on the prev slide

### Output and output append
- the command `$ ls > foo.txt`
  - creates a new file named foo, overrides any existing file named foo
- if you use `>>`, the output will be appended, leaving any contents that were prev in the file and adding the new output at the end
  - ie. `$ ls /etc >> foo.txt`
  - ie. `$ ls /usr >> foo.txt`
- `>>` still creates the file if it wasn't there previously

### Errors
- sometimes we don't want them ending up in our redirect file, so the default behaviour `>` is that errors stay on the terminal and only stdout enters the file
- however, the shell is powerful an we can decide on what goes where:
  - `1>`: send stdout only (same as `>`)
  - `2>`: send stderr only (stdout might stay on terminal)
  - `&>`: send both stdout and stderr
- you can add both `1>` and `2>` giving diff files
  - if you name the same file, you'll only get stdout

## Pipes
- a **pipe** is a holder for a stream of data
- pipe can be used to hold the output of one program and feed it to the input of another

### Asking for a pipe
- separate 2 commands with the `|` character
- let the shell do all the work!
  - ie. `$ ls | sort`
  - ie. `$ ls | sort > sortedls`

## Text editing in Linux
- editors inside the shell's window:
  - vim, emacs, pico, nano
- window-based editors:
  - gedit, sublime, vs code, eclipse

## Important Linux paths
- `/`: the root of the file system
  - every other file falls below `/` in the directory tree
- `~`: current user's home directory
- `.`: right here when it starts a path, and nothing if it occurs within a path (2nd case just a convenience for programming)
- `..`: parent directory

## Job control
- shell allows you to manage *jobs* (aka running processes)
  - place jobs in the background
  - move a job to the foreground
  - suspend a job
  - kill a job

### Background jobs
- if you follow a command line with `&`, the shell will run the job in the background
  - you don't need to wait for the job to complete, you can type in a new command right away
  - you can have a bunch of jobs running at once
  - you can do all this with a single terminal

## Suspend/kill foreground jobs
- suspend with `ctrl+Z`
  - job is stopped, not dead
  - job will show up in the `jobs` output
- kill with `ctrl+C`
  - it's gone...

### Moving a job back to the foreground
- the `fg` command will move a job to the foreground
  - you give `fg` a job number as reported by `jobs` preceded by a `%`
    - ie. `fg %1 `
  
## Wildcards
- when you type in a command line, the shell treats some characters as special
- these special characters make it easy to specify filenames
- the shell processes what you give it, using the special characters to replace your command line with one that includes a bunch of file names

### * character
- `*` matches anything
- if you give the shell `*` by itself as a command line arg, the shell will remove the `*` and replace it with all filenames in the current dir
  - ie. `a*b` matches all files in the current dir that start with `a` and end with `b`

### Other characters
- `?`: matches any single char
  - ie. `ls Test?.doc`
- `[abc...]`: matches any of the enclosed characters
  - ie. `ls T[eE][sS][tT].doc`
- `[!abc...]`: matches any character except those listed
  - ie. `ls [!0-9]*`

## Quoting
- If we don't want characters to mean their special form in the command line, surround a string with double quotes
  - ie. `echo here is a star "*"`

### Quoting exceptions
- some special characters are **not ignored** even if inside double quotes:
  - `$`: prefix for var names
  - `"`: the quote char for itself
  - `\`: for multiple things
  - `$(())`: for math
- command substitutions using `$(...)` or `..` are still evaluated

### Single quotes
- the strongest version of quotes, nothing at all is "escaped"
  - `$var` is not replaced by their value
  - `\` is no longer special
  - math `$(())` does not work
  - command substitution using `$(...)` does not work
- for syntax, you can use single quotes just like double quotes

# Bash

## Creating a shell program
- shell programs are written in text files, named `.bash` by convention
- any series of commands you can type into the terminal one by one can also form a shell program
  - a new line starts whenever you press enter

### How to run a shell program
- run a shell program in one of several ways:
  - `$ bash program.bash`
  - `$ . program.bash`
  - `$ source program.bash`

## First line of a bash program
- `#!/bin/bash`: the standard first line for every bash script
- known as a shebang sequence to indicate which shell we intend if bash is not the shell in terminal being used

## Shell variables
- shell keeps track of a set of parameter names and values
- assign variables with command prompt
  - ie. `# my_var=Hello`
- access variables with dollar sign
  - ie. `# echo $my_var`
- some of these are special parameters that determine behaviour of the shell
- some are used to build program logic
- variable with an assignment command is a shell *builtin* command:
  - ie. `# HOME=/etc`
- variable name syntax
  - no spaces in var name, between var name and equals, between equals and value, within value
  - to use spaces in values, enclose them in quotes
    - ie. `# NEWVAR="blah blah blah"`
  
### set command (shell builtin)
- the **set** command with no parameters prints out a list of all the shell variables

## Some bash variables
- `pwd`: current working directory
- `path`: list of places to look for commands
- `home`: user's home directory
- `mail`: where your email is stored
- `term`: what kind of terminal you have
- `histfile`: where your command history is saved
- `PS1`: the string to be used as the command prompt

### PS1
- the `PS1` shell var is your command line prompt
- is a string
  - ie. `# PS1="# "`

## Fancy bash prompts
- `\t`: is replaced by the current time
- `\w`: is replaced by the current directory
- `\h`: is replaced by the hostname
- `\u`: is replaced by the username
- `\n`: is replaced by a new line

## Capturing command output in a variable
- use the back-tick operator to skip the file system (efficiency of memory vs disk) and reuse the output within our bash program
  - format: `#  var=`command``
  - annything that `# command` alone would put to the terminal is now stored as the value of a var
  - access: `$var`

## Math
- shell can do simple arithmetic
- enclose computation: `$((computation))`
  - ie. `# a=$((3+5))`
  - ie. `# echo There are $((60*60)) seconds in an hour`

## Program exit codes
- every process returns an integer value when it terminates
- part of the Linux process spec

## If statements

Command | Description
--- | ---
`n1 -eq n2` | true if n1 and n2 are **equal**
`n1 -ne n2 ` | true if n1 and n2 are **not equal**
`n1 -gt n2` | true if n1 is **greater than** n2
`n1 -ge n2` | true if n1 is **greater than or equal to** n2
`n1 -lt n2` | true if n1 is **less than** n2
`n1 -le n2` | true if n1 is **less than or equal to** n2
`-z string` | true if string length = **zero**
`-n string` | true if string length = **non-zero**
`string1 = string2` | if the strings are **identical**
`string1 != string2` | if the strings are **not identical**
`string` | true if string is **not NULL**
`-r file` | true if it exists and is **readable**
`-w file` | true if it exists and is **writeable**
`-x file` | true if it exists and is **executable**
`-f file` | true if it exists and is a **regular file**
`-d file` | true if it exists and is a **directory**

### Writing if statements

- `[[ condition ]]` always works
- `[ condition ]` works on some bash shells, not all

## For & While Loops

### For loops

**Format**

    for (var) in (list)
    do
      (actions)
    done

**Example**

    $ cat for2.sh

    i = 1
    weekdays = "Mon Tue Wed Thu Fri"

    for day in "$weekdays"
    do
      echo "Weekday $((i++)) : $day"
    done

    $ ./for2.sh

    Weekday 1 : Mon
    Weekday 2 : Tue
    Weekday 3 : Wed
    Weekday 4 : Thu
    Weekday 5 : Fri

### While loops

**Format**

    while (condition)
    do
      (actions)
      [continue]
      [break]
    done

**Example**

    #!/bin/bash

    i = `wc -c < $1`;

    while test $i -lt $2
    do
      echo -n "0" >> $1;
      i = `wc -c < $1`;
    done

    % ./fill ass1.c 100

## Shell conditional structure
- shell conditionals are slightly different than other programming languages
- they rely on a program to run and give an output code that can be evaluated
- contrasting Bash and C, the if statement looks directly at built-in vars with equality, greater than, less than operators as a core part of the language
- mush used and give us our first building block to make larger programs and deploy our code for software systems

# C

## Program elements
- `#include`: way we ask for language functionality to be "turned on"
  - same as import in Java or Python
- `int main()`: indicates this is the first function to run in a prog
- `printf()`: basic method to write to terminal (stdout)
- `\n`: new line

## Compiling/running C programs
- compiling means to create a prog from source code
  - `$ gcc hello_world.c`
- running means asking the terminal to exec the prog
  - `$ ./a.out`

## History of C
- invented in 1972 by Denis Ritchie
- used to program early UNIX versions
- defined by the standed in K&R text first version
- standardized by ANSI organization as C90

## About C
- general purpose, imperative, typed language designed to support cross-platform code with minimal reqs
- a compiled language
  - source code cannot be run directly
  - needs to be processed by a *compiler* which produces machine code
- sign of a compiled language:
  - after compilation, the programs can run even if the compiler is not present on the system
  - important for creative low-level components like the kernel
- by having a compiler available for each machine, the identical C code is guaranteed to work everywhere, "cross platform"

## C philosophy
- C statements are close to the low level features provided by the computer and OS
- C gives us access to low level without getting in the way too much
- The programmer has the power:
  - access to low level memory
  - allowed to change the types used to interpret data
  - language often provides several ways to accomplish the same operation
- advantage: C code is easily portable across systems, even for low level ops
- risks:
  - programmer is free to make mistakes
  - C code can also sometimes be "ugly"
  - C code can be unsafe to run

## C datatypes
- C provides a number of built-intypes
  - `int`: integers
  - `float`: floating point numbers of single precision
  - `double`: floating point numbers of double precision
  - `char`: characters to represent text
- modifiers to give more flexibility
  - `short`, `long`: change the number of bits used to represent an int
  - `signed`, `unsigned`: determine whether one can represent negative numbers
  - pointers store addresses in the computer's memory

## Function components
- a return type (can be void)
- a name (must be unique in the prog)
- an arg list with types and var names (can be empty)
- a function body enclosed in {}

### C function properties
- name must be unique
- pass arguments by value
- define a local scope for variables

## Arrays 

### Data types
- in order to store more than one value within the same variable, C provides simple arrays:
  - ie. `int hourly_prices[24];`
- provide the size when declaring the variable, but it can be left off when specifying a function argument
  - ie. `int main( int argc, char *argv[] )`
    - the size is decided by the calling function (e.g., here we can type any number of arguments

### Array view in memory
- array variable gives us “space” to store N elements of the declared type
- In C this does not come with much extra help:
  - `array.len()` is not defined
  - `array.sort()` is not defined
  - `sizeof(array)` is a dangerous operation

### Array sizes
- C does not provide any safety mechanisms for programmers regarding array length
- it's possible to accidentally process past the end of the array

### Beyond the end of an array
- you can see any of the follow errors:
  - `*** stack smashing detected ***: ./a.out terminated`: Aborted (core dumped)
    - The program continues to run, but some other variable has been changed and problem occur later
  - `Segmentation fault` (core dumped)
    - There may be no detectable change and everything seems fine (if you are very lucky/unlucky)

### Array best practices
- define a constant for the array size to setup array and limit any time you use the data
      
      const int array_length = 10;
      float account_balances[array_length]; for( pos=0; pos<array_length; pos++ )

- use "pre-processor replacement" to specify size

      #define array_length 10
      float account_balances[array_length]; 
      for( pos=0; pos<array_length; pos++ )

- keep the length of your array in a variable
- more powerful because the array can take different lengths at run-time

      int array_length = 10;
      float account_balances[array_length];
      for( pos=0; pos<array_length; pos++ )

### Array sorting
- `pos`: each position in the array
- `curr`: current value at `pos`
- `min_val`, `min_pos`: position of the minimal element from `pos` to the end

## Binary
- find the decimal value of a binary number:
  - sum of 2<sup>i</sup> for every "1", where `i` is the position in the number, with 0 on the right
    - ie. 1101 = 2<sup>0</sup> + 2<sup>2</sup> + 2<sup>3</sup> = 1 + 4 + 8 = 13
- find the binary value of a decimal number:
  - while `decimal_val` not zero:
    - find the largest power 2 that is less than or equal and add a “1” in the binary number in position i, where i is the position in the number, with 0 on right

## What's in memory
- the Kernel process
- the Shell process
- your program's process
  - the program's machine code
  - program variables
  - required libraries
  - bookkeeping info

### Variables space
description | type | bits | range
--- | --- | --- | ---
integer | short | 16 | +/- 32k
integer | int | 32 | +/- 2.1b
integer | long | 64 | +/- 9.2 x 10<sup>18</sup>
floating point | float | 32 | +/- 10<sup>38</sup>
floating point | double | 64 | +/- 10<sup>308</sup>
floating point | long double | 128 | +/- 10<sup>4932</sup>
character | char | 8 | -127 to 128
character | unsigned char | * | 0 to 255
pointer | char* | 64 | 0 to 1.8 x 10<sup>19</sup>
pointer | int* (etc) | |

### The type matters for real data
- match between the meaning of underlying data and the available tools and data types from C
  - ie. 16-bit colour image made to be small to work on old game consoles:
    - red takes 4 bits
    - green takes 4 bits
    - blue takes 4 bits
    - transparency takes 4 bits
    - choices: 2 chars, 1 short, 3 chars, 1 int, etc.

### Arrays and memory
- arrays take multiple "slots', each of the underlying type
  - ie. `float array[2];`
- guaranteed to be stored in order
- number as you specify on array creation and does not change

## C strings
- arrays of characters
  - ie. `char name[100] = "David";`
- each element is a character stored using the ASCII table
  - a mapping between our printable letters and the 0s and 1s in memory
- must be "null terminated" with the special `\0` NULL value, with integer representation 0
- allows things like `printf()` to output just the data you want

### Strings in memory
- char type is really an 8-bit int

## C code to work with single characters
- single quotes for literals
  - `char char_variable = 'w';`
- math allows moving alphabetically forward and backwards, finding relative positions
  - `char_variable++;` (it's now = 'x')
  - `char_variable - 'a'` (tells you what position in alphabet, 23 here)
- logic works via alphabetical order
  - `char_variable == 'x'` (evals true)
  - `char_variable > 'z'` (evals false)

## C code to work with char arrays
- double quotes for literals
  - `char str_var[100] = "hello";`
- doesn't work with math
  - `str_var++;` (error)
  - `str_var – "jello"` (nothing related to 'h' – 'j')
- logic operators don't work
  - `str_var == "hello"` (incorrect)
  - `str_var > "jello"` (incorrect)

## Address operator &
- variable identifier represents the value of the variable
- variable identifier preceded with `&` represents the address of the variable

## Pointers
- a **pointer** is a variable that stores an address
- must be initialized with a specified address prior to its use
- pointers allow us to move around our strings (think iterators, lists, indices)
      char str_var[100] = “Hello”;
      char *start = str_var;
      char *mid = str_var+3;

### Pointer syntax
- declare a non-pointer variable:
  - `type varname;`
- declare a pointer with:
  - `type *varname;`
  - star can be anywhere between type and var
  - `varname` holds the value of "the address of a variable or array of `type`"

### Dereferencing
- the primary use of a **pointer** is to access and change the value of the variable that pointer points to
- value of the variable is represented by preceding the pointer variable identifier by an asterisk (*), which literally means 'value at address'
- the 'value at address' operator is also called indirection operator or **dereference operator**

      int i = 3;
      int *p = &i;
      printf("The value of i = %d\n", i);
      printf("The value of i = %d\n", *p);
      printf("The address of i = %p\n", &i);
      printf("The address of i = %p\n", p);
      printf("The address of p = %p\n", &p);

- `*p` holds the value of `i`
- `p` holds the address of `i`, aka `&i`

## Pointers vs arrays
- in some ways, they're interchangeable
  - an array in C is implemented as the address of its first entry, so we "point to" the array

        char array[4] = "abc";
        char *ptr = array;

- in some ways, they differ
  - the array variable holds the address to the start of this memory always, while the pointer if flexible

        char array[4] = “abc”;
        char *ptr = array;
        ptr++; ptr++; ptr++; ptr++; // These work

        array++; // This is an error!

  - an array says how much memory it requires at the beginning
  - a ptr declaration alone does not request memory to store data

        char array[4];
        array[0] = ‘a’; // This line is OK

        char *ptr;
        ptr[0] = ‘a’; // This line may seg fault

## C string using pointers
- pointing to start of literal or array works

      char *ptr = "hello";
      char str_array[100] = “hello”;
      char *ptr2 = str_array;

- pointer math moves us around the string and computes distances

      ptr=ptr+3; //Now points to lo
      ptr = ptr – 1; // Back to llo
      char *ptr2 = str_array;
      ptr – ptr2; // Gives position; difference, 2 here

## C Built-in libraries
- it's important we know how to do operations "bit-by-bit"
- standardized implementations of basic operations are provided as language libraries:
  - <stdio.h>
  - <stdlib.h>
  - <string.h>
  - <math.h>

### <string.h>

`size_t strlen(const char *str)`
- Computes the length of the string `str` up to but not including the terminal null character.

`int strcmp(const char *str1, const char *str2)`
- Compares the string point to, by `str1` to the string pointed to by `str2`

`char *strcpy(char *dest, const char *src)`
- Copies the string pointed to, by `src` to `dest`

`void *memset(void *str, int c, size_t n)`
- Copis the character `c` (an unsigned char) to the first `n` charactesr of the string pointed to, by the argument `str`

`char *strcat(char *dest, const char *src)`
- Appends the string pointed to, by `src` to the end of the string pointed to by `dest`

`char *strstr(const char *haystack, const char *needle)`
- Finds the first occurence of the entire string `needle` (not including the terminating null character) which appears in the string `haystack`


### <stdio.h>

**<stdio.h>** provides built-in functions to work with I/O.

**3 types of I/O**

Console
- Keyboard, screen
- stdin, stdout, stderr

Stream
- Constant stream of data from logical/physical device

File
- Reading or writing to file

**Important functions**

`int printf(const char *format, ...)`
- Sends formatted output to stdout

`int fputs(const char *str, FILE *stream)`
- Writes a string to the specified stream up to but not including the null character

`int fputc(int char, FILE *stream)`
- Writes a character (unsigned char) specified by the argument char to the specified stream and advances the position indicator for the stream

`int fprintf(FILE *stream, const char *format, ...)`
- Sends formatted output to a stream

`char *fgets(char *str, int n, FILE *stream)`
- Reads a line from the specified stream and stores it into the string pointed to by str. It stops when either (n-1) characters are read, the newline character is read, or the end-of-file is reached, whichever comes first

`int fgetc(FILE *stream)`
- Gets the next character (an unsigned char) from the specified stream and advances the position indicator for the stream

## fopen(...)
- returns a "file pointer"
  - NULL (=0) if there was any problem; **always check for this!**
  - otherwise, it's safe to use the other file operations
- the mode string indicates what we want to do, so `fopen()` can check that we have the correct permissions
  - `r`: read only. file must exist previously with read permission
  - `w`: write only. create new file or overwrite prev contents
  - `a`: append. create new file or add to end of prev contents
  - `b`: cn be added to any meaning to interpret the file as binary
  - `+`: for read and write together
  
## Text file formats
- can be unstructured, for the purposes of human consumption
  - ie. text version of a novel, your journal, written answers
  - common extension: `.txt`
- more interesting for systems: structured text files
  - ie. C program, BASH program, website, etc.
- thinking text as data for our computation
  - goal: allow programs to easily save and restore content, not necessarily easy to read for humans, but also a "nice to have"
  - systems programmers are responsible for creating good file types: how to split up the data, tell where one item ends and another begins, make processing fast

### Text data file examples
- `.csv`: comma separated values
- `.html`: hyper text markup language
- `.md`: markdown
- `.yaml`: yet another markup language
- `.json`: javascript object notation

## 2D arrays
- declared by repeating the square bracket syntax
  - `char ttt[3][3];`
- creates an array where each entry is itself an array
- the type (ie. char) is the same for every entry of the inner array
- outer array behaves as we expect:
  - fixed size
  - always represents the memory of the first entry and can't be moved (ie. no `++`)
  - elements stored directly after one another

![2D arrays](https://i.imgur.com/SV5xzQl.png)

### Accessing 2D array data
- for `type array_name[N][M]`
  - syntax `array_name[i][j]` is used both to read
and write data at entry `i`, `j`
- first index can be 0 to N-1, second index can be 0 to M-1
- in the image:
  - `ttt[0][0]` evaluates to ‘x’
  - `ttt[2][2] = ‘o’;` sets bottom-right value

### When to use a 2D array
- when each "row" has the same size always

        int days_in_each_month[2][12] = {
          { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 },
          { 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 } 
        };

- often when data looks like a "table"

        float grades[number_of_assignments][class_size] = {
          { 100.0, 99.0, 83.0 },
          { 99.9, 99.9, 99.8 } 
        };

### What 2D arrays look like in memory

- the system memory is addressed in ascending linear order (1D)
  - ie. `char[2][3] greetings = { ”hi”, “yo” };`

![2D arrays in memory](https://i.imgur.com/XBgqCza.png)

### When to not use 2D arrays
- if the data in each entry "row" can vary in length
- if the length of each entry "row" isn't known when coding, for example it will change based on user input
  - ie. `char greetings[4][8] = { “hi”, “yo”, “bonjour”, “hello” };`

![when not to use 2d arrays](https://i.imgur.com/yWtPBaC.png)

### Array of pointers
- the type of each entry is now `char*`, a single character pointer
  - ie. `char *greetings[4] = { “hi”, “yo”, “bonjour”, “hello” };`
- avoids wasting space

![array of pointers](https://i.imgur.com/ZzkDIJn.png)

### How to call functions with 2D arrays
- `void f( char current_board[3][3] ){` 
- `void f( char current_board[][3] ){`
- `void f( char(*current_board)[3] ){`
  - this says “pointer to data of type 3-length char array”
  - we can note the outer array with unknown size is equivalent to pointer
  - the brackets around (char*) are essential to distinguish this from an array of pointers

## argv[]
- type is char* argv[], an array of pointers, each to one of the argument words
- want the user to be able to type any number of arguments, each of any length and have the result end up properly sorted
- we can sort “in place” by working only on the pointer values within argv, no need to create a new variable

![argv](https://i.imgur.com/iaZt3Xr.png)

# Memory

## Key thoughts on memory
- your `a.out` is a file, it takes space on a disk
- each running process requires system resources
- when we create systems programs, we have control over how the memory is used

## Elements of a running process
- text space
- data segment
- BSS segment
- heap
- stack
- Kernel space
- memory mapping

### Statically Sized Parts that come from gcc
- text space
  - our runnable machine code
  - stored in `a.out`
  - the processor loads these instructions and steps through line by line
- data segment
  - elements of fixed size that are known at compile time
  - stored in `a.out`
    - ie. string literals
  - values can't be changed
- BSS (Blocked Started by Symbol) segment
  - space to hold "static" variables without initial values
  - filled with zeroes at run time
  - not actually stored in `a.out`, but only the size needed since there's no initial data
  - changeable

### Elements that change in size
- stack
  - space to store the local variables within each function
  - stack "frames" are created when the function is called, and removed when the function returns
  - stack variables, including arrays, are temporary and we shouldn't keep pointers to them
- heap
  - space for the programmer to control dynamically
  - where we're allowed to request the most available resources
  - grows and shrinks at our request
  - in object oriented languages, used for "new" objects

### Others
- Kernel
  - we get to see a copy of a portion of the kernel in each process
  - this is a "trick" of the OS to give us low level functionality
- memory mapping
  - access to files and libraries the OS connects us to

## Static (global), stack, and heap memory

### Automatically sized arrays
- these arrays live on the **stack**
- the size limit is typically quite small
- good for small stuff, but we need to ask for way more memory

### Stack memory limited
- when creating pointers, we must think about the stack push-pop behaviour
  
### To fix Statically Sized Arrays
- test with `gcc –DSTATIC_ARRAY_SIZE=number`
- you can typically get away with much larger sizes, even to the point where you can't practically use the memory provided
  
## The heap: dynamically allocated memory
- provides flexible persistent memory across function calls
  - ie. `char* createImageData( int width, int height )`
    - should return a pointer to new memory that can hold pixel data
    - incorrect to point to a stack variable
    - if we don't know the size of our bunny ears in advance, we can't use static
- request for N bypes of heap memory (not initialized):
  - `void *malloc(size_t numberOfBytes);`
- request for an array of N elements each with size bytes, and initializes the values all to 0
  - `void *calloc(size_t num, size_t size_of_each);`

### Allocating useful types
- `malloc` and `calloc` return a void pointer: `void *`
- it must be cast before it can be dereferenced
  
      int *a = (int *) malloc( sizeof(int) * 40 ); // OR
      int *a = (int *) calloc(40, sizeof(int));

- the `sizeof()` function simplifies the allocation of memory by calculating the size of the provided data type

### Rules to follow for malloc
- 3 TYPEs to fill in:
  - `TYPE1 variable_name = (TYPE2)malloc( sizeof(TYPE3)*number );`
  - `int *pi = (int*)malloc( sizeof(int) * 1 );`
- RULE1: TYPE1 matches TYPE2, both are pointer types:
  - the point of casting is to tell C how we plan to interpret the heap memory allocated for us by malloc
  - malloc returns `void*` so that it can handle any type
  - since this is initially empty memory, casting is always safe
- RULE2: TYPE3 is the de-referenced version of TYPE2
  - “one less star”
  - when we dereference the variable with `*variable_name`, C will assume the memory is of the pointer’s underlying type

### Using malloc correctly
- allocate a single integer on heap
  - `int *pi = (int*)malloc( sizeof(int) );`
- allocate an array of 10 integers on heap
  - `int *my_numbers = (int*)malloc( 10*sizeof(int) );`
- allocate a single integer pointer on heap
  - `int **ppi = (int**)malloc( sizeof(int*) );`

### Common malloc errors
- mismatch between sizes
  - `int *pi = (int*)malloc(10*sizeof(char));`
- not casting to pointer
  - `int i = (int)malloc(sizeof(int));`
- forgetting `sizeof` data type
  - `int *my_array = (int*)malloc(10);`

### Realloc
- **realloc** changes the size of memory
  - `void *realloc(void *ptr, size_t size);`
- heap might not contain enough space to expand at this address
- data can be copied to a new location, changing the pointer
  
![realloc memory](https://i.imgur.com/9EmVRiz.png)

### Important last step!
- for every dynamically allocated section in memory, ensure to run `free` when finished using
  - `free(void *ptr);`
- the pointed must be to heap memory, allocated by malloc, calloc or realloc
- signals the OS this space can now be used again
- good practice:
  - `ptr=NULL`
  - immediately after `free`
  - ensures you don't forget and mess with memory of another var

# BMP images

## How do cameras form images?
- project light with a lens that has similar behaviour to our eye
- instead of a retina, light sensitive electronics (CCD or CMOS), count arriving photons
  - each is tuned for a colour we call RGB (red, green, blue)
  - RGB values at one spot are called a *pixel*
- each pixel is read off as 3 integer values
  
## How to sore images as a file on disk
- an image is a 2D grid of pixels
  - num_rows = height
  - num_cols = width
  - num_colors: how many per pixel could be 3 for RGB, 1 for b/w, or 4 for RGBA (alpha = transparency)
- 2 additional types of data:
  - a header
    - holds information fields such as the image size, compression, color depth
  - padding
    - almost always present to align the elements into 4 or 8 byte boundaries (details coming)

## Image file formats
- `.bmp`: Windows bitmap
- `.jpg`: Joint Photographic Experts Group
- `.png`: Portable Network Graphics
- `.tiff`: Tagged Image File Format
- `.gif`: Graphics Interchange Format

## How to read BMP file using C
- what works well:
  - check the magic number
    - if it matches, very likely it follows the rules
  - file size field: makes it easy to access all the data
  - width and height, allows finding a specific pixel
  - opening with code like `rb`
- what to avoid:
  - checking for ASCII code values: space, new line, etc.
  - attempting to use `atoi`, `atof`
  - if we open with `r` alone (no b), C will do some of this automatically and cause us problems
  - `fgets`, `fscanf` meant to work with text

## Word endianess
- the order that bytes within an integer are stored in memory is a convention, named Endianess, and there is no right answer
- most systems we deal with will be Little Endian, but there are major execptions (the Sun company)

![big and little endian](https://i.imgur.com/ClJUlYd.png)

### Endianess intuition helper
- little or big?
  - name determined by the "significance" of the byte at the first address:
  - little: the least significant comes first
  - big: the most significant comes first

### Endianess and images
- BMP files are explicitly specified to use Little Endian, which is the format of most machines (all x86 compatible)

## Steganography
- concerned with concealing the fact that a secret msg is being sent as well as concealing the msg contents

![Diagram of a Steganography Software System](https://i.imgur.com/4pLrhGo.png)

### Elements required for in-image steganography
- read and write binary image data
- read and change pixel values
- steganography requires us to work with bits directly
  - view the text string in binary form so we can access one bit at a time
  - ability to modify only a single bit (the LSB) of each pixel
  - ability to extract the LSB again for decoding

## Bit-wise operations
- shifts:
  - `bit_arg<<shift_arg`
    - shifts bits to of bit_arg shift_arg places to the left -- equivalent to multiplication by 2^shift_arg
  - `bit_arg>>shift_arg`
    − shifts bits to of bit_arg shift_arg places to the right -- equivalent to integer division by 2^shift_arg
- bit-wise logic:
  - left_arg & right_arg
    − takes the bitwise AND of left_arg and right_arg
  - left_arg | right_arg
    - takes the bitwise OR of left_arg and right_arg
  - left_arg ^ right_arg
    - takes the bitwise XOR of left_arg and right_arg (one or the other but not both)
  - ~arg
    - takes the bitwise complement of arg

### Bit-shift operators
- moves the existing bits a specified number of positions left or right
  - when a bit hits the edge, it is lost
  - new bits always take 0 (for unsigned – we do not cover signed bit shifting in 206)
- note, shifting is similar to multiplying or dividing by powers of 2

### Multi-byte shifting
- treats the true int value
- we don't have to think about address ordering here
- if a bit hits the boundary of its byte, it seamlessly moves to the next using significance order
- now, only the least and most significant byte boundaries cause "loss"
  
![multi-byte shifting](https://i.imgur.com/vDbHueR.png)

### Bit-wise logical operators
- each applies a *truth table* to the bits in its args, one at a time
- logical AND and logical OR truth tables
- when you apply a & b, these operations are applies to all of the bits in a and b, 1 bit at a time

![bit-wise logical operators](https://i.imgur.com/6tZ8Y8E.png)

### Integrated bit-wise example
- check the value of bit 3

      char c = 85;
      if( (c & (1<<2)) > 0 )
      printf( “bit 3 of c is a 1!\n” );
      else
      printf( “bit 3 of c is a 0!\n” );

![example](https://i.imgur.com/kGDb5UQ.png)

