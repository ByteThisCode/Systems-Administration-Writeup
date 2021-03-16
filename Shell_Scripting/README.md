# Introfuzione linea di comando

---------------
## Table of Contents

0. [Comandi utili](#comandi-utili)
1. [Login e interprete dei comandi](#login-e-interprete-dei-comandi)
2. [Sudo](#sudo)
3. [Shutdown](#shutdown)
4. [Alias](#alias)
5. [VIM](#vim)
---------------
## Comandi utili

- `history` per visualizzare l'elenco dei comandi eseguiti in un terminale
  - CTRL-R attiva la reverse-i-search con la quale ricercare un comando
- `script` cattura in un file una sessione di lavoro del terminale
  - Per uscire digitare `exit` o `CTRL-D`

## Login e interprete dei comandi

  * `whoami` Indica il proprio username
  * `id` Informazioni sull'indentità e sul gruppo di appartenenza
  * `who` indica chi è attualmente collegato alla macchina


## Sudo

Configurare sudo:
 * Diventare root tramite `su -`
 * Lanciare il comando `visudo`
 * Aggiungere la linea `utente <tab> ALL=(ALL:ALL) <space> ALL`
 * Usare `adduser utente sudo`
 * Per aggiornare le modifiche effettuare un logout e rientrare

`su - root`  
`sudo -i` apre sessione terminale


## Shutdown

`shutdown [-h|-r] now` 
* -h arresto  
* -r riavvio  
* -now quando eseguire l'azione

## Alias

Utile per memorizzare linee di comando complesse in comandi più semplici da invocare.

`alias nomealias='comando'`

## VIM

