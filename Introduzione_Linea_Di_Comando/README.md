# Introduzione linea di comando

---------------
## Contenuti

0. [Comandi utili](#comandi-utili)
1. [Info](#info)
2. [Sudo](#sudo)
3. [Shutdown](#shutdown)
4. [Alias](#alias)
5. [VIM](#vim)
---------------
## - Comandi utili

- `history` - Per visualizzare l'elenco dei comandi eseguiti in un terminale
  - CTRL-R attiva la reverse-i-search con la quale ricercare un comando
- `script` - Cattura in un file una sessione di lavoro del terminale
  - Per uscire digitare `exit` o `CTRL-D`

## - Info

  * `whoami` - Indica il proprio username
  * `id` - Informazioni sull'indentità e sul gruppo di appartenenza
  * `who` - Indica chi è attualmente collegato alla macchina


## - Sudo

Configurare sudo:
 * Diventare root tramite `su -`
 * Lanciare il comando `visudo`
 * Aggiungere la linea `utente <tab> ALL=(ALL:ALL) <space> ALL`
 * Usare `adduser utente sudo`
 * Per aggiornare le modifiche effettuare un logout e rientrare

`su - root`  
`sudo -i` - Apre sessione terminale


## - Shutdown

`shutdown [-h|-r] now` 
* -h arresto  
* -r riavvio  
* -now quando eseguire l'azione

## - Alias

Utile per memorizzare linee di comando complesse in comandi più semplici da invocare.

`alias nomealias='comando'`

## - VIM

-TODO