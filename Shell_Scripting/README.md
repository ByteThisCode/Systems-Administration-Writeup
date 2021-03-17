# Shell Scripting

---------------
> [Back to Main](../../../)
---------------
## - Contents

0. [Useful Commands](#--useful-commands)
1. [Redirection](#--redirection)

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

**Special Redirection**

* **Definitive redirection**: `exec`
  Utseful for 
  * Ex: `exec 2>/dev/null`
