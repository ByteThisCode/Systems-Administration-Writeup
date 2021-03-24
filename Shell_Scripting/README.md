# Shell Scripting

---------------
> [Back to Main](../../../)
---------------
## - Contents

0. [Useful Commands](#--useful-commands)
1. [Redirection](#--redirection)
2. [Filters](#--filters)
    * [cat](#--cat)  
    * [less](#--less)
    * [rev](#--rev)
    * [head/tail](#--headtail)
    * [cut](#--cut)
    * [sort](#--sort)
    * [uniq](#--uniq)
    * [wc](#--wc)
    * [grep](#--grep)
    * [tr](#--tr)
    * [sed](#--sed)
    * [awk](#--awk)
3. [Regex](#--regex)
4. [xargs](#--xargs)
5. [tee](#--tee)
6. [Process Substitution](#--process-substitution)
7. [Command Substitution](#--command-substitution)
8. [Multiple Files Tools](#--multiple-files-tools)
    * [diff](#--diff)   
    * [paste](#--paste)  
    * [join](#--join)  
9. [Quoting](#--quoting)  
10. [Pathname Expansion](#--pathname-expansion)  
11. [Brace Expansion](#--brace-expansion)  
12. [Parameter Expansion](#--parameter-expansion)
13. [Variables](#--variables)  
    * [Environment Variables](#--environment-variables)   
    * [Notable Variables](#--notable-variables)  
    * [Positional Variables](#--positional-variables) 
    * [Indirect Access](#--indirect-access)  
14. [Array](#--array)  
15. [Builtin Read](#--builtin-read)
16. [Arithmetic Evaluation-Expansion](#--arithmetic-evaluation-expansion)
17. [Flow Control](#--flow-control)
    * [Command Sequences](#--command-sequences)
    * [Functions](#--functions)
    * [Condition Assessment](#--condition-assessment)
    * [test / \[ \] ](#--test---)
    * [Builtin \[\[ \]\]](#--builtin---)
    * [if](#--if)
    * [case](#--case)
    * [for](#--for)
    * [while](#--while)
    * [Cycle Termination](#--cycle-termination)

---------------
## - Other Examples
0. [Quoting](./Examples#--quoting)

---------------
## - Useful Commands

* `2>/dev/null`      - Discard errors 
* `unset VAR`        - To remove an environment variable/array  
* `$'\x0a'`          - HEX value for '\n'
* `&&` `||` `!`      - They can also be used on the command line to logically combine the exit codes of any process
  * Example: `mkdir $MYDIR && cd $MYDIR`                 -  true if I can create $MYDIR and can access it    
  * Example: `cd $MYDIR || echo "$MYDIR inaccessible"`   -  Only if cd fails is it necessary to execute the second command  

---------------
## - Redirection

Bash can disconnect predefined streams from the terminal and have the same file descriptor open on a different file.

* **stdout** redirection: `>` e `>>`
  * `ls > filename`     - Writes the stdout of ls to the file filename, truncating it
  * `ls >> filename`    - Writes the stdout of ls to the file filename, in append
* **stderror** redirection: `2>` e `2>>`
  * same as above

* Confluence of streams
  * `ls > filename 2>&1`   - Redirect stderr into stdout and then stdout to file (Order matters!)

* **stdin** redirection: `<`
  * `sort < filename`      - Dump the contents of the file to sort's stdin

  
**- Special Redirection**

* **Definitive redirection**: `exec`  

  * Ex: `exec 2>/dev/null`  

  Useful because open file descriptors are inherited by child processes:
  * Ex: `exec 3< filein 4> fileout 5<> filerw`  
    -Every reading made from fd 3 with `<&3` will read from filein  
    -Every writing made to fd 4 with `>&4` will write to fileout  
    -Fd 5 can be used for both reading and writing to filerw  
    -To close: `exec 3>&- 4>&- 5>&-`  

* **Here documents**: `commands <<MARKER`  
  Send text directly to a command until the chosen marker is inserted (case sensitive)  
   ``` 
    Lorem ipsum dolor sit amet,  
    consectetur adipiscing elit.
    MARKER
    ```
  * For a single line the marker is not needed: `comand <<< "Single text line..."`  

---------------
## - Filters

### - cat
  * Invoked without parameters copies stdin to stdout  
  * Invoked with one or more files produces their contents in sequence on stdout: `cat file1 file2`
  * `tac`   - Prints the input lines (stdin or file) on stdout in reverse order, from last to first


### - less
  * Displays the contents of a file or a command output, one page at a time. Allows you to navigate both forward and backward through the file.  
  * You can move using up/down arrow or up/down page
  * Main commands: 
      ```
    F / Space bar    - Move Forward one page  
    Ng               - Go to the N-th line in the file  
    G                - Go to the last line in the file  
    /pattern         - Search forward for matching patterns  
    ?pattern         - Search backward for matching patterns  
    n                - Repeat previous search  
    N                - Repeat previous search in reverse direction  
    q                - To quit less  
    ```

### - rev
  * Allows you to reverse the order of the characters of each line of the input stream towards the output stream  
  * Ex: `cat /etc/passwd | rev | cut -f1 -d: -s | rev`

### - head/tail
  * **head** is a filter that allows you to extract the initial part of a file (default first 10 lines)
      ```
    -c NUM        - Produces the first NUM characters  
    -c -NUM       - Produces the entire file except the last NUM characters 
    -n NUM        - Produces the first NUM lines  
    -n -NUM       - Produces the entire file except the last NUM lines  
    ```
   
  * **tail** is a filter that allows you to extract the final part of a file (default last 10 lines)
      ```
    -c NUM              - Produces the last NUM characters  
    -c +NUM             - Produces the entire file starting with the NUM character 
    -n NUM              - Produces the last NUM lines
    -n -NUM             - Produces the entire file starting with the NUM lines
    -f                  - Keeps the file open and any new lines that are appended to it by other processes are displayed in real time
      -f --pid=PID         - At the termination of the PID process also terminates tail 
      -f --retry / -F      - Tail will keep trying until it opens the file (in case you are expecting a file generated by another process)
      ```

### - cut
  * Allows you to cut parts of lines
  * `-c` - Produces for each input line an output line consisting only of the characters listed. Ex:   `cut -cPOSITION [input_file]`
    ```
    cut -c15            - Returns only the 15th character  
    cut -c8-30          - Returns the characters from the 8th to the 30th  
    cut -c-30           - Returns characters up to the 30th  
    cut -c8-            - Returns the characters from the 8th onwards  
    ```
  * Other options:
    * `-d`             - Allows you to define a delimiter  
    * `-f`             - Allows you to select a field  
    * `cut -dDELIMITER -fNUM`  
    * Ex:  `cat /etc/passwd | cut -d: -f1 -s`  
      * `-s` - It prevents lines that do not contain the delimiter from being output    
 

### - sort
  * Allows you to sort the lines of a stream (default lexical order)  
  * Global behavior options:  
      ```
      -u             - Delete multiple entries (equivalent to sort | uniq)  
      -r             - Reverse (descending order)  
      -R             - Random (random permutation of lines)  
      -m             - Merge of the file already sorted  
      -c             - Check if the file is already sorted  
      ```  
  * Other criteria:  
      ```
      -b             - Ignore leading spaces   
      -d             - Only consider alphanumeric characters and spaces   
      -f             - Ignore the difference between lowercase / uppercase  
      -n             - Interpret strings of numbers by their numeric value  
      -h             - Interpret "readable" numbers, such as 2K, 1G...  
      ```
  * It can also search for sort keys in specific positions in the row:   
      ```
      -tSEP          - Set SEP as separator between fields (default spaces)  
      -kKEY          - Sorting key, if used more than once, sorts by the first key, if this equal by the second key, and so on  
      ```  
    KEY is in the form (simplified) `F[.C] [,F[.C]] [OPTS]`  
      * F = Field number  
      * C = Position (characters) in the field  
      * OPTS = One of the sorting options [bdfgiMhnRrV]  
      * Ex: `sort -t. -k 1,1n -k 2,2n -k 3,3n -k 4,4n` (sort ip address list)  

### - uniq
  * Eliminates consecutive duplicates   
      ```
      -c             - It also indicates the number of rows packed into one (ocurrence number)  
      -d             - It shows only non-single entries  
      ```
      
 ### - wc
  * (word count) is a count filter
      ```
      -l, --lines             - Print the number of lines  
      -w, --words             - Print the number of words  
      -m, --chars             - Print the number of characters  
      -c, --bytes             - Print the number of bytes  
      -L, --max-line-length   - Print the length of the longest line  
      ```
      
### - grep
  * Examines incoming lines of text and outputs those containing a pattern corresponding to a **regular expression**  
  * **egrep** modern version  
  * Matching type check:  
      ```
      -E                - Treats pattern as an extended regular expression  
      -F                - Disable the RE and use the parameter as a literal string  
      -w, -x            - Match whole word or whole line  
      -i                - Ignores case for matching  
      ```  
  * Input check:  
      ```
      -r                - Recursively searches all files in a folder  
      -f FILE           - Takes patterns from file, one per line  
      ```
  * Output check:  
      ```
      -o                - Print only the matched parts of a matching line, with each such part on a separate output line  
      -v                - Prints out all the lines that do not matches the pattern  
      -l                - Displays list of a filenames only  
      -n                - Display the matched lines and their line numbers  
      -c                - This prints only a count of the lines that match a pattern  
      --line-buffered   - disable buffering  
      ```
      
### - tr
  * Replaces individual characters more quickly without regex, some examples:  
    * `tr 'A-Z' 'a-z'`     - Changes uppercase to lowercase   
    * `tr ';:.!?' ','`     - Replaces any occurrence of the characters in the first set with ,  
    * `tr ';:.!?' ',-'`    - If the second set is more limited than the first set, its character is repeated enough to generate the 1: 1 match  
    * `tr -d '\r'`         - Eliminates any occurrence of the carriage return  

### - sed
  * Stream EDitor, base format: `sed -e 'command'` or `sed -f 'script'`  
  * Substitution command: `sed 's/OLD_PATTERN/NEW_VALUE/[modifiers]'`  
    * Ex: `cat /etc/passwd | sed 's/^/Line:/'` - Insert string 'line:' at the beginning of each line of passwd  
    * With -E the patterns are about those of egrep  
  * Substitution command modifier:  
      ``` 
      i        - Case sensitive  
      g        - Global, replaces all occurrences in the line  
      NUM      - Replaces only the NUMth occurrence  
      ```
  * Command line options:  
      ```
      -i [suffix]    - Edits the given file [backup with SUFFIX extension if provided]  
      -u             - Unbuffered  
      ```
  
  * Examples:  
      ```
      (file : hi hi hi)  
      bash$ cat file | sed -e 's/hi/hello/'  
         hello hi hi  
      bash$ cat file | sed -e 's/hi/hello/g'  
         hello hello hello  
      ```
      
    
### - awk
  * Works on programs that contain rules comprised of patterns and actions  
  * The action is executed on the text that matches the pattern  
  * Default: prints the number of the chosen field, provided it is separated from the previous one by a number of blanks  
  * Ex: `cat /etc/passwd | awk '{print $2}'` - Print the second field of the file  
  * **-F separator** - To specify a file separator  
  * Identifiers:  
      ```
      $0: Represents the entire line of text  
      $1: Represents the first field  
      $2: Represents the second field  
      $45: Represents the 45th field  
      $NF: Stands for "number of fields" and represents the last field  
      ```

---------------
## - Regex

  * Special Atoms:  
      ```
      .        -   Indicates any character  
      ^        -   Indicates the start of the line  
      $        -   Indicates the end of the line  
      ```
  * Backslash sequence:  
      ```
      \< -\>   -   The empty string at the beginning - at the end of a word  
      \b       -   The empty string at the edge of a word  
      \B       -   The empty string provided it is not at the boundary of a word  
      \w       -   Any letter, number or _  
      \W       -   Any one not included in \w  
      ```
  * Multipliers:  
      ```
      {n,m}    -   From n to m occurrences of the atom that precedes it  
      ?        -   Zero or one occurrence of the atom that precedes it  
      *        -   Zero or more occurrences of the atom that precedes it  
      +        -   One or more occurrences of the atom that precedes it  
      ```
   * Charset:  
      ```
      [abc]    -   Any character between a, b or c  
      [a-z]    -   Any character between a and z inclusive  
      [^dc]    -   Any character that is neither d nor c  
      ```
   * Character class based charsets [:CLASS_NAME:], where CLASS_NAME:  
      ```
      alnum digit punct alpha graph space  
      blank lower upper cntrl print xdigit  
      ```

---------------
## - xargs

  * `xargs <command>` expects a list of strings on standard input, and it then invokes command with those strings as arguments  
  * Peculiarities of behavior:  
      * xargs groups the invocations in order to reduce the load. This can work with commands like ls, but not if the command accepts a single parameter  
      * xargs passes the input lines as they are on the command line built. The presence of spacers will therefore make the command perceive invoked a multiplicity of parameters
  * Options:  
      ```
      -0 (zero)      - Uses null, not space, as the argument terminator  
      -n MAX         - Use at most MAX input lines for each invocation  
      -p             - It asks interactively for confirmation of the launch of each command (useful to keep strings together)  
      ```
  * Ex: `pipeline | which produces | filenames | xargs ls -l` - Will run ls -l for each file received from the pipeline  
  * Ex: `cd /etc && echo passwd group | xargs ls -l` - Will run ls -l for each file received from the pipeline  


---------------
## - tee

  * Is a useful command for duplicating an output stream  
    * Send a copy of stdin to stdout  
    * Sends an identical copy in a file passed as a parameter  
    * `-a`    - The file is opened in append  
  * Ex: `command1 | tee FILE | command2`  

---------------
## - Process Substitution

  *  Pipe the stdout of multiple commands (Feeds the output of a process (or processes) into the stdin of another process)  
  *  Concurrently launches two processes where a pipe is used   
  *  There is no space between the the "<" or ">" and the parentheses  
  *  `cmd_consumer <(cmd_producer_on_stdout)`  
  *  `cmd_producer_on_file >(cmd_consumer_from_stdin)`  
  *  Examples:  
      ```
      bash$ less <(ls -l)   
         /dev/fo/12  - The pipe itself  
         
      bash$ cat <(date)  
         Thu Jul 21 12:40:53 EEST 2011  
      
      bash$ cat <(date) <(date) <(date)  
         Thu Jul 21 12:44:45 EEST 2011  
         Thu Jul 21 12:44:45 EEST 2011  
         Thu Jul 21 12:44:45 EEST 2011  
     
      bash$ echo <(date) <(date) <(date)  
         /proc/self/fd/11 /proc/self/fd/12 /proc/self/fd/13  

      bash$ wc <(cat /usr/share/dict/linux.words)  
       483523  483523 4992010 /dev/fd/63  

      bash$ grep script /usr/share/dict/linux.words | wc  
          262     262    3601  

      bash$ wc <(grep script /usr/share/dict/linux.words)  
          262     262    3601 /dev/fd/63  
      ```

---------------
## - Command Substitution

  * Command substitution allows the output of a command to replace the command itself  
  * Command substitution occurs when a command is enclosed as follows:  
    * `$(command)`  
    * `'command'`  
  * Bash performs the expansion by executing command in a subshell environment and replacing the command substitution with the standard output of the command  
  * Ex: `ls $(cat /etc/passwd | cut -f6 -d:)` - Extracts the home dirs of the users and sets them as parameters to ls  

---------------
## - Multiple Files Tools

### - diff
  * Allows you to show the differences between two files  
  * Special symbols:  
    * a : add  
    * c : change  
    * d : delete  
  * Example:  
      ```
      file1:            file2:  
      row one           row one  
      row two           row three  
      row three         row 4  
      row four          row five  
      row five          row 4bis  
      
      bash$ diff file1 file2  
         2d1  
         < row two  
         4c3  
         < row four  
         ---  
         > row 4  
         5a5  
         > row 5bis  
      ```
      
### - paste
  * Used to join files horizontally (parallel merging) by outputting lines consisting of lines from each file specified, separated by tab as delimiter, to the standard output  
  * When no file is specified, or put dash ("-") instead of file name, paste reads from standard input and gives output until a interrupt command [Ctrl-C] is given  
  * Ex: `paste file1 file2 file3`  

### - join
  * Same principle as paste, but join lines if they start with the same "Key" (needs files sorted identically on the selected key)  
  * Ex: `join file1 file2`  

---------------
## - Quoting

  * The wildcard expansion mechanism and variables is powerful but interferes with literal interpretation of some symbols such as: []!*?${}()"'\`\\|><;  
  * When you have to pass as a parameter to a command of strings containing such symbols, is required protect them from expansion  
  * Methods:  
      ```
      \   - (backslash) inhibits the interpretation of the character only next as special  
      '   - (apex) each character of a string enclosed in a apex pair is protected from expansion and treated literally, without exception  
      "   - (double apex or double quote) any character of a string enclosed in a pair of quotation marks is protected from the expansion,   
             with the exception of the $, of the backtick (`) of \, and other special cases  
      ```
  * Examples:  
      ```  
      \"       The backslash protects the quotes -> on the line remains "  
      '"'      As above, quotes protect quotes  
      \\       The first backslash protects the second -> on the line remains \  
      ```  
  * Interaction examples:  
      ```
      A=hello
      ls *\**           -  List filenames containing the * character in any position
      echo "$A"         -  Print the contents of variable A exactly (hello)
      echo '$A'         -  Print exactly $A
      echo "'$A'"       -  Print 'hello'
      
      bash$ echo $(ls)
         file1 file2
      bash$ echo "$(ls)"
         file1
         file2
      ```
---------------
## - Pathname Expansion  

 * Used interactively with file and directory management commands
 * The patterns are compared to the filesystem:  
   * If there are no files that match the pattern, the pattern remains unchanged on the command line  
   * if there are matching files, instead of the pattern are placed all their names, in alphabetical order  
 * Pattern:  
   * \*        - Represents any string of zero or more characters
   * ?         - Represents any single character
   * [SET]     - Represents any character belonging to SET:
     * A list, ex: `[afhOV]`  
     * An interval, ex: `[a-k]`  -  multiple intervals, ex: `[a-d, 0-5]`  
     * It can be denied with `!` o `^`, ex: `[!a]` `[^A-Z]`  
     * It can be a class like egrep, ex: `[[:alnum:]]`  
     * To include `-` or `]` in the SET, put them as the first character  

---------------
## - Brace Expansion  

 * Unlike pathname expansion, strings are generated independently regardless of whether or not there are files that match the pattern
 * Syntax: `[PRE]{LIST/SEQUENCE}[POST]`
 * List example: `a{d,c,b}e`  ->  expanded from the shell into `ade ace abe○`  
 * Sequence examples:  
     ```  
     file{9..13..2}.c   ->  file9.c file11.c file13.c    (..2 optional increment)    
     doc{009..11}       ->  doc009 doc010 doc011         (009 zero-padding)  
     {a..j..3}          ->  a d g j                      (a,j single alphabetic character only)  
     ```  
     
---------------
## - Parameter Expansion  

 * Useful to be able to easily manage the assignment of default values
 * Syntax: `variable + operator + arguments`  
 * Examples:
     ```
     FILEDIR=${1:-"/tmp"}           -  Use default /tmp if $1 is null or not set - does not alter $1
     cd ${HOME:=/tmp}               -  If $HOME is null or not set, it is initialized to the default /tmp, then expanded in any case
     FILESRC=${2:?"File error!"}    -  Print the error and exit if $2 is null or not set
            Note: omitting the ":", the empty string is considered valid and the default value is used only if the supplied variable is not set
     
     ${1    - Variable
     :-     - Operator
     "/tmp" - Argument
     ```
 * Variable manipulation / search&replace
    
     | name:number:number | Substring staring character, length |
     | ------------------ | ----------------------------------- |
     | #name | Return the length of the string |
     | name#pattern | Remove (shortest) front-anchored pattern |
     | name##pattern | Remove (longest) front-anchored pattern |
     | name%pattern | Remove (shortest) rear-anchored pattern |
     | name%%pattern | Remove (longest) rear-anchored pattern |
     | name/pattern/string | Replace first occurrence |
     | name//pattern/string | Replace all occurrence |
   
   Ex: Changing the .bad extension of a file to .good - `mv "${filename}" "${filename/.bad/.good}`

---------------
## - Variables  

 * A way offered by the shell to store text strings under a given name  
 * The modification/creation of a variable is obtained by indicating on the command line the name of the variable followed by = and the value to be attributed to it  
 * Ex: `NAME=value`  
 * Parameter expansion is used to read the contents of a variable, ex: `$NAME`  
 * If NAME is compound or ambiguous, protect it with {}: `${NAME}`  

### - Environment Variables  

  * Provide a simple way to share configuration settings between multiple applications and processes in Linux: `export varname`  
  * The current environment can be viewed with the `set` command  
  * `env`   -  Print a list of the current environment variables  

### - Notable Variables

 * Set by bash:
   ```
   - BASHPID         -  PID of the current shell  
   - $               -  PID of the "parent" shell  
   - PPID            -  PID of the parent process of the "parent" shell  
   - HOSTNAME        -  Host name  
   - RANDOM          -  Random number from 0 to 32767  
   - UID             -  User id running the shell  
   ```
 * Used by bash:
   ```
   - HOME            -  User home directory
   - LC_vari         -  Choice of various aspects of localization
      - man locale(7), locale(1) and locale(5)
   - PS0..PS4        -  Prompts in different contexts
   ```
 * Others:  `man bash` -> Shell variables

### - Positional Variables

 * Each script can access the arguments indicated on its command line, using variables that have a number as their name:
   ```
   $#          -  Arguments numeber  
   $0          -  Command name  
   $1,$2...    -  Arguments  
   $*, $@      -  Expanded into $1 $2 $3 ...  
   "$*"        -  Expanded into "$1 $2 $3 ..."   
   "$#"        -  Expanded into "$1" "$2" "$3" ...
   ```
 * Positional variables can be set manually:  
     ```  
     bash$ set dog cat house  
     bash$ echo $2  
      cat  
     ```  
 * `shift`  -  Size down the list of positional variables (excluding $0), assigning $N the content of $N+1:  
     ```  
     bash$ echo $# $1 $2 $3 
      3 dog cat house  
     bash$ shift  
     bash$ echo $# $1 $2  
      2 cat house  
     ```    

### - Indirect Access

 * Allows to obtain the name of a variable, which can then be used in any other expression, obtaining an indirect reference to  
 * Example:  
     ```  
   bash$ KEY=RED  
   bash$ RED=VALUE  
   bash$ echo ${!KEY}  
      VALUE  
     ```  
---------------
## - Array

 * Declaration: `delcare -a VECTOR`  
 * Assignment: `VECTOR[0]="first value"`  
 * Access: `echo ${VECTOR[0]}`  
 * Direct initialization: `VECTOR=(value1 value2 value3)`  
 
 * Initialization with alternative separators: 
     ```
     bash$ STRING="Here.there.are"
     bash$ IFS='.' VECTOR=($STRING)
     bash$ echo ${VECTOR[1]}
      there
     ```
 * Indices can be non-consecutive:
     ```
     A[5]="an element"
     A[8]="another element"
     ```
 * To view all the elements of the array you can use the index * or @
     ```
     bash$ echo "${A[*]}"
      an element another element 
     ```
 * To know the set of indexes corresponding to actually assigned cells of the array, use `${!name[@]}` 
     ```
     bash$ echo ${!A[@]}
      5 8 
     ```
 * To know the number of assigned cells, use `${#name[*]}`
     ```
     bash$ echo ${#A[*]}
      2
     ```
 * **Associative Arrays** (bash 4 and next)
   * In associative arrays, the index can be a string, not just a number: they are key-value maps (note `-A` as option):
     ```
     bash$ declare -A ASAR
     bash$ ASAR[fistkey]=firstvalue
     bash$ echo ${ASAR[fistkey]}
      firstvalue
     bash$ KEY=fistkey
     bash$ echo ${ASAR[$KEY]}
      firstvalue
     ```
     
---------------
## - Builtin Read

 * Reads strings from stdin and assigns them to variables  
   *  The input is tokenized using IFS (any spacer by default)  
   *  If there are more tokens than variables, those in excess all end up in the last specified variable, as a single string, including separators  
     ```
     bash$ read A B C  
     today I brought a sandwich for lunch  
     bash$ echo $A / $B / $C  
      today / I / brought a sandwich for lunch  
     ```
 * Read separates using IFS:  
      ```
      bash$ IFS=:  
      bash$ read A B C  
      today:I brought:a sandwich:for lunch  
      bash$ echo $A / $B / $C  
       today / I brought / a sandwich:for lunch  
      ```
  * Options:  
      ```
      -p PROMPT      - Print PROMPT before accepting input  
      -u FD          - Reads from FD instead of stdin  
      -a ARRAY       - Assigns tokens to elements of ARRAY  
      ```
  * Example:  
      ```
      bash$ read -p "Tell me three colors: " -a COL  
      Tell me three colors: red green blue            (Output: "Tell me three colors:", User Input: red green blue)  
      bash$ echo ${COL[1]}  
         green  
      ```
     
   * Processes memento  
       ```
       bash$ echo hello | read A  
       bash$ echo $A  
         (nothing)  
       ```
     * Manipulate variables in child processes without losing the results before they can be used  
       * Solution: Subshell  
           ```
           echo hello | ( read A ; echo $A )
           ```  
     * Need to use to acquire read data interactively by the user in a child process that has stdin supplied by a pipe instead of from the terminal  
       * Solution: create a file descriptor for the terminal:
           ```
           bash$ exec 3<$(tty)
           bash$ echo hello | ( read -u 3 A ; echo $A )
           ```
     
     
---------------
## - Arithmetic Evaluation-Expansion

 * Contexts in which arithmetic evaluation takes place:  
   * To the assignment of values on **declared integer** variables:
       ```
       bash$ declare -i N
       bash$ N="3 * (2 + 5)"
       bash$ echo $N
         21
       ```
   * Using the let builtin or the equivalent compound command (()):
       ```
       bash$ let N++
       bash$ echo $N
         22
       ```

 * (( ))
 
   * Protects the elements of the expression as " "  
   * The $(( )) token is expanded with the result of the expression
   * Example:
       ```
       bash$ counter=0
       bash$ declare -p counter                                               -  -p    Show the type and value of the symbol
         declare -- counter="0"                                               -  "--"  Stands for no type
       bash$ echo $(( counter++ )) $(( counter++ )) $(( counter++ ))
         0 1 2
       bash$ counter=$(( counter * newvar + 100 ))
       bash$ echo $counter
         100
       ```
       
 * Operators:
     ```
     id++ id-- ++id --id                      Post/pre-increase/decrease  
     + - * /                                  Sum subtraction product division  
     ** %                                     Exponentiation, modulo  
     ! ~ & | ^                                NOT NOT AND OR XOR bit-a-bit  
     << >>                                    Left / right binary shift   
     = *= /= %= += -= <<= >>= &= ^= |=        Assignment  
     <= >= < > == !=                          Comparison  
     && ||                                    Logic AND / OR   
     expr1?expr2:expr3                        Returns the result of expr2 or expr3 respectively if expr1 is true or false  
     ```
 * Numbers can be expressed in any base between 2 and 64
   * Default 10, octal 0 prefix, hexadecimal 0x prefix
   * Prefix B# -> base B
     
---------------
## - Flow Control


### - Command Sequences 

 * To keep running in the main shell memory space, use { } 
 * Esample: 
     ```
     bash$ { read A ; read B ; } <<< e$'\x0a'f
     bash$ echo $A . $B
      e . f
     ```
     
### - Functions     

 * Functions are sequences of commands with a name: `function NAME() { SEQUENCE ; }
 * They can receive parameters: `$1 $2` - `$0` remains set to the script name
 * The name of the function is placed in `$FUNCNAME`
 * If invoked "simply", they are performed in the context of the caller:
   * Same memory space
   * "global" variables
   * Possibility to declare local variables with `local`
 * Attention to invocations in the pipeline (the function will be executed by the automatically created child bash)

### - Condition Assessment

 * The bash flow control commands do not process logical expressions, but decide the path based on the exit code of a process 
   (0 == true, other values == false)
 * The exit code of a process is found in the special variable `$?` set by the shell at the end of the process 
 * There are several builtins and commands: `test`, `[ ]`, `[[ ]]`  
     
### - test / [ ]

 * They are the same built-in 
 * Followed by an expression, they evaluate it and return 0 or 1 consistently to the result (true or false)
 * **Main unary tests** -  `test OP ARG`  
     ```
     > For strings:  
         -z             -  true if empty string  
         -n             -  true if not empty string  
     > For files:  
         -e             -  true if file exists  
         -f             -  true if regular file  
         -d             -  true if directory  
         -s             -  true if not empty file  
     
     (Others help test)
     ```
 
 * **Main unary tests** -  `test ARG1 OP ARG2`  
     ```
     > Lexical comparison between strings:  
         =, !=, <, >  
     > Numerical comparison between strings:  
         -eq, -ne       -  equal, not equal  
         -lt, -le       -  less than, less or equal  
         -gt, -ge       -  greater than, greater or equal  
     > File comparison:
         -nt            -  newer than  
         -ot            -  odler than
     ```

 ### - Builtin [[ ]]    
 
  * Supports the same test functions / [ ] and also
    * The binary operators `==` and! `!=` Match the left parameter with a pattern expressed by the right parameter with the same syntax of the pathname expansion
        `[[ "hello" == h?l[b-s] ]]` -> true
    * The binary operator `=~` matches the left parameter with a regular expression specified by the right parameter
        `[[ "hello" =~ ^h.{2}p$ ]]` -> true
 
 ### - if
 
   ```shell
   if CMD1
   then
      #commands executed if CMD1 returns true
   fi
   ```
   ```shell
   if CMD1; then
      #commands executed if CMD1 returns true
   fi
   ```
   ```shell
   if CMD1; then
      #commands executed if CMD1 returns true
   elif CMD2
      #commands executed if CMD2 returns true
   else
      CMD3
   fi
   
   ```

### - case
   ```shell
   case "$variable" in
      name1) echo is name1 ;;
      name?) echo is name2, namea, namez ;;
      name*) echo is name11, name, namehello ;;
      [1-9]name) echo is 1name, 2name, …, 9name ;;
      *) echo not any of the precedents ;;
   esac
   ```
   
### - for
   ```shell
   for VARIABLE in 1 2 3 4 5 .. N
   do
      command1
      command2
      commandN
   done
   ```
   ```shell
   for VARIABLE in file1 file2 file3
   do
      command1 on $VARIABLE
      command2
      commandN
   done
   ```
   ```shell
   for OUTPUT in $(Linux-Or-Unix-Command-Here)
   do
      command1 on $OUTPUT
      command2 on $OUTPUT
      commandN
   done
   ```
 * `for NAME [in WORDS ...]; do COMMANDS; done`
 * WORDS = pathname expansion pattern:  
   `. for F in /tmp/*.bak ; do rm -f "$F" ; done`
 * WORDS = command line parameters:  
   `. for PAR in “$@” ; do echo "$PAR" ; done` 
 * WORDS = command substitution:  
   `for USER in $(cat /etc/passwd | cut -f1 -d:) …`
 * WORDS = brace expansion:  
    `for ITEM in item_{a..z}`
    
 * `for BACKWARDSTENTHS in $(seq 1 -0.1 0)`
 * `for (( i=0, j=0 ; i+j < 10 ; i++, j+=2 ))`

### - while

  ```shell
  while CMD                - or until CMD
  do
   command1
   command2
   commandN
  done
  ```
  * `while` - Iterates if CMD returns true
  * `until` - Iterates if CMD returns false

### - Cycle Termination

 * `break [N]`  
   * Exits a for, while, or until loop  
   * If specified N, exits N nested loops
 * `continue [N]`  
   * Jumps to the next (possible) iteration of a for, while, or until loop
   * If specified N, it restarts going up by N nested cycles
