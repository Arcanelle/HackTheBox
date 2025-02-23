
# SYNCED

![alt text](image.png)

#rsync #protocols #reconnaissance #anonymous/guestaccess

## 1. Méthodologie

On commence par l'énumération avec la commande `nmap -sV -p- IP_cible`:

```
└──╼ [★]$ nmap -sV -p- 10.129.138.181
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-23 06:22 CST
Nmap scan report for 10.129.138.181
Host is up (0.0089s latency).
Not shown: 65534 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
873/tcp open  rsync   (protocol version 31)
```

Un seul port est ouvert, le 873 (rsync). C'est un service utilisé pour le transfert de fichiers et la synchronisation de fichiers. Nous allons donc lister les dossiers/fichiers disponibles sur l'hôte cible avec la commande `rsync --list-only IP_cible::`

```
└──╼ [★]$ rsync --list-only 10.129.138.181::
public         	Anonymous Share
```

Nous allons tenter d'explorer ces dossiers avec la commande `rsync --list-only IP_cible::share_name`:

```
└──╼ [★]$ rsync --list-only 10.129.138.181::public
drwxr-xr-x          4,096 2022/10/24 17:02:23 .
-rw-r--r--             33 2022/10/24 16:32:03 flag.txt
```

Le dossier public contient le fichier flag.txt. Je vais donc le récupérer avec la commande `rsync IP_cible::share_name/file_name /destination/sur_ma/machine/file_name`: Ici je vais stocker le flag.txt dans mon /home/user.

```
└──╼ [★]$ rsync 10.129.138.181::public/flag.txt /home/arcanelle/flag.txt
```

Je vais ensuite afficher le fichier avec la commande `cat`:

```
└──╼ [★]$ cat flag.txt
72eaf5344ebb84908ae543a719830519
```
Pour info, si je dois me connecter en utilisant un user, voici la commande rsync: `rsync --list-only user@IP_cible::[share_name]`

```
└──╼ [★]$ rsync --list-only user@10.129.138.181::
```

## 2. Questions

### Task 1

What is the default port for rsync?

```
873
```

### Task 2

How many TCP ports are open on the remote host?

```
1
```

### Task 3

What is the protocol version used by rsync on the remote machine?

```
31
```

### Task 4

What is the most common command name on Linux to interact with rsync?

```
rsync
```

### Task 5

What credentials do you have to pass to rsync in order to use anonymous authentication? anonymous:anonymous, anonymous, None, rsync:rsync

```
none
```

### Task 6

What is the option to only list shares and files on rsync? (No need to include the leading -- characters)

```
list-only
```

### Flag

```
72eaf5344ebb84908ae543a719830519
```