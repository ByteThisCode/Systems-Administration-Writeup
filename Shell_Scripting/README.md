# Shell Scripting

---------------
## Table of Contents

0. [Comandi utili](#comandi-utili)
1. [Ridirezione](#ridirezioni)

---------------
## Comandi utili



## Ridirezione

Bash può disconnettere gli stream predefiniti dal terminale e far trovare gli stessi file descriptor aperti su un file diverso-.

* Ridirezione stdout: `>` e `>>`
  * `ls > filename` scrive lo stdout di ls nel file filename, troncandolo
  * `ls >> filename` scrive lo stdout di ls nel file filename, in append

* Ridirezione stderror: `2>` e `2>>`
  * uguale a sopra

* Confluenza degli stream
  * `ls > filename 2>&1` ridirige lo stderr dentro stdout e poi stdout su file (L'ordine è importante!)

* Ridirezione stdin: `<`
  * `sort < filename` riversa il contenuto di del file su stdin di sort
