# Command Line Introduction

---------------
> [Back to Main](../../../)
---------------
## Contents

0. [Useful Commands](#--useful-commands)
1. [Info](#--info)
2. [Sudo](#--sudo)
3. [Shutdown](#--shutdown)
4. [Alias](#--alias)
5. [VIM](#--vim)
---------------
## - Useful Commands

- `history` - To view the list of commands executed in a terminal
  - CTRL-R activates the reverse-i-search to search for a command
- `script` - Capture a terminal work session in a file
  - To exit type `exit` o `CTRL-D`
- `which pname` - Lets you know which version of pname you are using
- 
---------------
## - Info

  * `whoami` - Indicate your username
  * `id` - Information about the identity and the group to which it belongs
  * `who` - Indicates who is currently connected to the machine

---------------
## - Sudo

Config file: **/etc/sudoers**

Configure sudo (add user username to sudoers) two ways:
 * A:
   * Become root via `su -`
   * Launch the command `visudo`
   * Add the line `username <tab> ALL=(ALL:ALL) <space> ALL`
   * Type `adduser username sudo`
 * B:
   * `adduser username sudo`
 * To save changes logout and re-login

`su - root`  
`sudo -i` - Open terminal session

---------------
## - Shutdown

`shutdown [-h|-r] now` 
* -h shutdown  
* -r restart  
* -now when to perform the action

---------------
## - Alias

Useful for storing complex command lines into commands that are easier to invoke.

`alias aliasname='commands'`

---------------
## - VIM

![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) `TODO`
