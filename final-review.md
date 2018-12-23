# Operating system

- Piece of software that allows us to interact with a computer without needing to know the inner workings
- Manages resources
- Launches every other program

# Unix OS Components

**Kernel**
- Login
- Knows how to run programs
- Basic common interface

**File system**
- Defines the way the disk drive is formatted
- The file allocation table (FAT)
- The data structure on disk that makes files real
- Reading and writing to disk and peripherals
- User commands to interact with files

**Shell**
- A user interface
- Has a global memory
- Has commands to interact with OS

**Utilities**
- Additional OS commands and programs
- Third party commands and programs
- Drivers and managers

# File System

- `\` = root file system
- `~` = current user's home directory
- `.` = current directory
- `..` = parent directory
- using `cd` command by itself from any location will move you to the home directory `~`

### Commands

- `ls` = list files
  - `ls -l` = list files in long form
  - prints a .txt file of the list. if there is no other command, .txt file gets pushed to standard out
- `cd` = change directory
- `pwd` = present working directory (where am I now?)
- `mv` = move files or directories
- `find` = search for files with given properties
- `chmod` = change permissions
- `cp` = copy files or directories
- `cat` = concatenate input files
- `echo` = copy input to output
- `grep` = filter input based on a pattern
- `tr` = translate inputs to outputs
  - ie. `ls | tr .txt .doc` lists file in .txt and converts file to .doc
  - ie. `ls | tr [a-z] [A-Z]` converts every lowercase in the file to uppercase 
- `sort` = sort inputs, then output
  -  usually use sort with a pipe command
  -  default is numerically ascending, alphabetically ascending lowercase then uppercase
- `ps` = display running processes (once)
- `top` = display the running processes (continuous) and resources usage
- `uname` = print system information (which Linux version)
- `ssh` = remoately connect to another computer

### Shell variables at startup

- `pwd` = current working directory
- `path` = list of places to look for commands
- `home` = home directory of user
- `mail` = where your email is stored
- `term` = what kind of terminal you have
- `histfile` = where your command history is saved
- `PS1` = the string to be used as the command prompt

# Redirection

- Changing the standard I/O (keyboard/screen)
  
### Input can be redirected

- From a file, `<`
  - ie. `grep pattern < search_file.txt`
- From the output of another program, `|`
  - ie. `cat test.txt sample.txt | more`

### Output can be redirected

- To a file, `>`
  - ie. `ls > file_info.txt`
- As input to another file, `|`
  - ie. `cat test.txt sample.txt | more`

### Changing both I/O

- ie. `sort < nums > sortednums`
- ie. `tr [a-z] [A-Z] < letter > rudeletter`

### Appending

- `>>` appends to a file or creates the file if it doesn't yet exist
  - ie. `ls /etc >> foo.txt` appends or creates a file "foo.txt" with the directory listing in "/etc"
  - ie.  `echo "the end" >> file.txt` adds "the end" to the end of the file "file.txt"

# Bash scripting

### Shebang

    #!/bin/bash

### Running a bash script

    $ bash filename.bash
    $ . filename.bash
    $ source filename.bash

### Important basics

`$`
- Access content of variables
  - ie. `ls -al $dir`, with `$dir` as a variable

`echo`
- Displays/prints variable
  - ie. `echo $var` prints contents of `$var`

`math`
- $((computation))
  - ie. `a=$((3+5))` would make contents of variable `$a` be the computation of 4+5

## If statements

| Command              | Description                                   |
| -------------------- | --------------------------------------------- |
| `n1 -eq n2`          | true if n1 and n2 are **equal**               |
| `n1 -ne n2 `         | true if n1 and n2 are **not equal**           |
| `n1 -gt n2`          | true if n1 is **greater than** n2             |
| `n1 -ge n2`          | true if n1 is **greater than or equal to** n2 |
| `n1 -lt n2`          | true if n1 is **less than** n2                |
| `n1 -le n2`          | true if n1 is **less than or equal to** n2    |
| `-z string`          | true if string length = **zero**              |
| `-n string`          | true if string length = **non-zero**          |
| `string1 = string2`  | if the strings are **identical**              |
| `string1 != string2` | if the strings are **not identical**          |
| `string`             | true if string is **not NULL**                |
| `-r file`            | true if it exists and is **readable**         |
| `-w file`            | true if it exists and is **writeable**        |
| `-x file`            | true if it exists and is **executable**       |
| `-f file`            | true if it exists and is a **regular file**   |
| `-d file`            | true if it exists and is a **directory**      |

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

## Job control

**Definition**

A shell has the capacity to manage **jobs**, known as processes you run simultaneously. 

- If you finish a command with the `&` shell and while it's not finished, it'll run it concurrently with other processes you have running.
- The shell will, appropriately, assign an ID number to each job it runs in the background.
- Suspend or kill any running jobs with `CTRL+Z` and `CTRL+C`

**Commands**

- `jobs` lets you view running jobs
- `fg` switches a job running in the background to the foreground
- `bg` restarts a suspended job and runs it in the background

**Example**

    $ sleep 100 &
    [1] 1384

    $ jobs
    [1]+  Running                 sleep 100 &

- `[1]` is the job number
- `1384` is the PID (process ID number)
- To kill this job process, use `kill %1` or `kill 1384`

## Wildcards

`*`
- Any pattern
  - ie. `ls *.doc`
  - List all documents with **.doc** extension

`?`
- Any character
  - ie. `*.d?c`
  - List all documents with **.d[any character]c** (.dac, .dzc, etc.)

`[]`
- Or
  - ie. `ls *.d?[acb]`
  - List all documents with **.d[any character][a or b or c]** (.dza, .dmb, etc.)

# C Basics

## Datatypes

**Built-in types**
- `int` = 16/32 bit integer
- `float` = 32 bit floating point number
- `double` = 64 bit floating point number
- `char` = 8 bit character

**Modifiers**
- `short/long` = 16 vs 32 bit int
- `signed/unsigned` = positive vs negative

**Pointers**
- `int *`
- `char *`

## Accessing arguments

`argc`
- Argument count
- Good for condition statements

`argv[i]`
- Access ith argument
- `argv[0]` is always the program name
  - ie. `./a.out`

## Control flow

### If statements

    if (COND) statement;

OR

    if (COND) {
      statements;
    }

OR

    if (COND1) {
      CODE
    }
    else if (COND2) {
      CODE2
    }
    else {
      CODE3
    }

### Switch statements

A **switch statement** allows a variable to be tested for equality against a list of values.

    switch (VAR) {
      case VAL1:
        CODE
        break;      // not optional
      
      case VAL2:
        CODE
        break;      // not optional

      ...

      default:
        code
    }

### For & while loops

    while (COND) statement;

OR

    while (COND) (
      statements;
    )

OR

    do statement; while (COND);

OR

    do {
      statements;
    }
    while (COND);

OR

    for (START; COND; EXPRESSION) statement;

OR

    for (START; COND; EXPRESSION) {
      statements;
    }

## Pointers

**Definition**

**Pointers** are variables which allow you to access (typed) areas of memory.

Referencing
- `&p`
- Return the address of the structure
  
Dereferencing
- `*p`
- Get the at location of p

**Example**

    int x = 5;
    int *p;
    p = &x;
    
    printf("%d", x);      // prints 5
    printf("%d", &p);     // prints 5

## Pass by value vs. Pass by reference

BOTH PRINT 6!

**Pass by value**

    int increment(int n) {
      n++;
      return n;
    }

    int main() {
      int a = 5;
      a = increment(a);
      printf("The value of a is now %d.\n", a);
    }

**Pass by reference**

    int increment(int *m) {
      (*m)++;
    }

    int main() {
      int a = 5;
      increment(&a);
      printf("The value of a is now %d.\n", a);
    }

## Arrays

**Definition**

In C, **arrays** are a series of contiuguous variables of the array type with the array variable being a pointer to the first variable.

- `array[0] == *array`
- `array[i] == *(array+i)`
- `&(array[j]) == (array+j)`

### Types

`type name[size]`
  - ie. `int data[100]`
  - ie. `char name[30]`

`type name [cols][rows]`
  - ie. `int picture[100][200]`

`type name [cols][rows][layers]`
  - ie. `char world[100][100][50]`
  
## Strings

**Definition**

**Strings** are simple char arrays with a 'terminating' null character: `\0`

### Logic

- String logic is based on **pointer position**
- Need to iterate through both or use <string.h>

**Example**

    char *a = "bob";
    char *b = "bob";

    if (a == b)          // false

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

## <stdio.h>

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

## fopen()

- `FILE *fopen(const char *filename, const char *mode)`
- `FILE` is a built-in pointer type
- Always check if NULL pointer after opening
- Modes:
  - `r` = read
  - `w` = write
  - `a` = append

**Example**

    void main() {
      FILE *s = fopen("letter.txt", "r"), *d = fopen("copy.txt, "w");

      if (s == NULL || d == NULL) exit(1);

      copyFile(s, d);
      fclose(s);
      fclose(d);
    }

## fseek()

- `fseek(FILE *stream, long int offset, int whence)`
- Sets the file position of the stream to the given offset

**Parameters**
- stream
  - This is the pointer to a FILE object that identifies the stream
- offset
  - This is the number of bytes to offset from whence
- whence
  - This is the position from where offset is added

**Constants**

`SEEK_SET`
- Beginning of file

`SEEK_CUR`
- Current position of the file pointer

`SEEK_END`
- End of file

**Example**

    #include <stdio.h>
 
    int main () {
      FILE *fp;
      
      fp = fopen("file.txt","w+");
      fputs("This is tutorialspoint.com", fp);
  
      fseek(fp, 7, SEEK_SET);
      fputs(" C Programming Language", fp);
      fclose(fp);
   
      return(0);
    }

Original output:
- `This is tutorialspoint.com`

New output
- `This is C Programming Language`

## Heap Memory

- Use **dynamically allocated memory** when you aren't sure how much memory you'll need ahead of time
- **Heap memory** can allocate and deallocate memory dynamically many times

Request for `N` bytes of heap memory (not initialized):

    void *malloc(int N);

Request for an array of `N` elements each with `size` bytes, and initializes the values all to 0:

    void *calloc(int N, int size);

Ask for additional memory (`size` is the new total size, not added to the old request):

    void *realloc(void *ptr, size_t size);

After using heap memory:

    free(void *ptr);
    ptr = NULL;

**Example**

    int main(void) {
      int *array;
      int n;

      scanf("%d", &n);
      array = (int *) calloc(n, sizeof(int));
      if (array == NULL) exit(1);

      *(array+2) = 5;
      prinf("%d", *(array+2));
      free(array);

      return 0;
    }

## Stack vs. Heap memory

**Stack:**
- Static memory allocation
- Good for when you know exactly how much memory you need to allocate before compile time
- 
**Heap:**
- Dynamic memory allocation
- Used when you don’t know how much memory you will need before compile time

## Endianess
- order that bytes within an integer are stored in memory
- Little
  - least significant comes first
  - most systems use this
- Big
  - the most significant comes first
  - humans use this
- 00000001 00000000: in big endian, this is 256, in little it is 1

## Debugging
- GDB debugger
  - GNU debugger, free and available as part of gcc
  - Has key commands that allow more precision and control than other methods
  - Starting gdb:
    - `gcc -g -o helloworld helloworld.c`
    - `gdb helloworld`
  - Common commands:
    - run <args>, break <line# or funct>, list, backtrace, print <var>, disp <var>, next, nexti, finish
    - help command for more

## Multiple-file projects and libraries
- managing a program composed of multiple files

### Header files
- include the things you want to share among files
- `.h` file usually has the same name as corresponding `.c` file
- has names nad types but no implementation
- `#include` it in any files that want to use that info
- only list `.c` files when compiling with gcc and there will no longer be a warning

### Compiling and linking
- waste of time to compile libraries repeatedly
- use linker
  - `gcc -c swap.c -> swap.o`
  - `gcc main.c swap.o`

## Libraries
- Import using `#include`
- Commmon libraries:
  - Char `ctype.h`
  - File I/O `stdio.h`
  - Math `math.h`
  - Dynamic memory `stdlib.h`
  - String `string.h`
  - Time `time.h`
- Two types to focus on
  - static `.a` (copied by linker into executable)
  - shared `.so` (loaded dynamically as needed, remain in separate files)

### Create/use libraries
- Static
  - gcc -c swap.c
  - ar rcs libswap.a swap.o
  - gcc main.c libswap.a
- Shared
  - gcc -shared -fpic -o libswap.so swap.c
  - gcc main.c -lswap -L
- Set library path:
  - Export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}
  - Environment variable

## Struct
- Allows you to compose primitive elements into a single complex structure
  
      struct OPTIONAL_NAME {
        FIELDS;
      } OPTIONAL_VAR_NAME;

      struct PERSON a;
      scanf("%s", a.name);
      scanf("%d", &(a.age));
      a.salary = 50.25;
      printf("%s %d %f", a.name, a.age, a.salary);

### Struct array

      struct PERSON {char name[30]};
      int main() {
        struct PERSON people[100];
        for (int x=0;x<100;x++) {
          scanf("%s", people[x].name);
        }
      }

### Struct: malloc
- to access contents: `(*x).age=5;` or `x->age=5`

      struct STUDENT {int age; float GPA;};
      struct STUDENT *x;
      x = (struct STUDENT *)malloc(sizeof(struct STUDENT));
      if (x==NULL) exit(1);

### Struct: linked lists

      void printNodes(aNode* my_node) {
        printf("%i", my_node->value);
        if (my_node->next != NULL) {
          printNodes(my_node->next); // recursive
        }
      }

## Pointers to functions

- int *fn(); // function returns int pointer
- int (*fn)(); // pointer to a function that returns an integer

## Make

## Macros
- Allows you to save time and not have to write everything out
  - `$(name)`

## Socket
- connects 2 computers over a network
- composed of: data structure, network, coms alg

### Sockets in C
- socket() -> create a socket, specify its protocol type
- bind() -> specify coordinates (address, port)
- listen() -> wait for someone to respond, OS does all the checking
- accept() -> if someone's there, acknowledge and start coms
- read() -> just like a file, put data into socket
- write() -> just like a file, get data out of socket

## TCP
- transmission control protocol
- Provides a reliable, ordered and error-checker delivery of a stream of bytes
between applications communicating through an IP network
Important tools: Sequenced numbers, Acknowledgements, Re-transmission, Congestion control

## High level layers
- HTTP → HyperText Transfer Protocol (Port 80)
- SSH → Secure shell
- FTP → File Transfer Protocol
- SMTP → Email
- DHCP → Obtaining address on the network
- DNS → Looking up computers with specific names

## HTTP
- request: method (GPPD), URI, ptl version, header
- response: response code, response text, header, data
- response codes:
  - 200: OK
  - 401: unauthorized
  - 403: forbidden
  - 404: not found
  - 500: internal server error
