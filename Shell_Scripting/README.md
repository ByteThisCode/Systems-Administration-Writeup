# Shell Scripting

---------------
> [Back to Main](../../../)
---------------
## - Contents

0. [Useful Commands](#--useful-commands)
1. [Redirection](#--redirection)
2. [Filters](#filters)

---------------
## - Useful Commands

* `2>/dev/null` - Discard errors 


## - Redirection

Bash can disconnect predefined streams from the terminal and have the same file descriptor open on a different file.

* **stdout** redirection: `>` e `>>`
  * `ls > filename` - Writes the stdout of ls to the file filename, truncating it
  * `ls >> filename` - Writes the stdout of ls to the file filename, in append
* **stderror** redirection: `2>` e `2>>`
  * same as above

* Confluence of streams
  * `ls > filename 2>&1` - Redirect stderr into stdout and then stdout to file (Order matters!)

* **stdin** redirection: `<`
  * `sort < filename` - Dump the contents of the file to sort's stdin

  
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


## - Fitlers

  * cat
    * Invoked without parameters copies stdin to stdout  
    * Invoked with one or more files produces their contents in sequence on stdout: `cat file1 file2`
    * `tac` - Print the input lines (stdin or file) on stdout in reverse order, from last to first
  
