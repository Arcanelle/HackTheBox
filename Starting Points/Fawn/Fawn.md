
# FAWN

![alt text](image.png)

#ftp, #protocols, #reconnaissance, #anonymous/guest access

## 1. Méthodologie

Ici, on commence de nouveau avec l'énumération: `nmap -sV IP_cible`

```
└──╼ [★]$ nmap -sV 10.129.16.173
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-22 06:32 CST
Nmap scan report for 10.129.16.173
Host is up (0.0098s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
Service Info: OS: Unix
```
On y découvre un service et sa version. Nous pouvons tenter de nous connecter à ce service avec les credentials par défaut et avec cette commande: `ftp IP_cible` : Les credentials par défaut du service ftp sont: ID: anonymous (pas de mot de passe).

```
└──╼ [★]$ ftp 10.129.16.173
Connected to 10.129.16.173.
220 (vsFTPd 3.0.3)
Name (10.129.16.173:root): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 
```

Nous pouvons parcourir les fichiers/dossiers présents sur le serveur avec la commande `ls` : Ici nous constatons un fichier présent appelé "flag.txt".

```
ftp> ls
229 Entering Extended Passive Mode (|||8477|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
```

Nous allons essayer de télécharger le fichier via le protocole ftp que nous utilisons avec la commande `get file_name`. Le fichier ira se stocker sur notre machine dans le répertoire dans lequel nous étions avant de lancer le ftp. Ici mon /home/user.

```
ftp> get flag.txt
local: flag.txt remote: flag.txt
229 Entering Extended Passive Mode (|||48576|)
150 Opening BINARY mode data connection for flag.txt (32 bytes).
100% |****************************************************************************************************************|    32       10.02 KiB/s    00:00 ETA
226 Transfer complete.
32 bytes received in 00:00 (2.74 KiB/s)
```
Puis nous allons lire le fichier. Pour cela nous quittons le ftp avec la commande `exit` et nous allons afficher le fichier avec la commande `cat file_name` : 

```
ftp> exit
221 Goodbye.

└──╼ [★]$ ls
cacert.der  Desktop  Documents  Downloads  flag.txt  Music  my_data  Pictures  Public  Templates  Videos
└──╼ [★]$ cat flag.txt
035db21c881520061c53e0536e44f815
└──╼ [★]$ 
```

## 2. Questions

### Task 1

What does the 3-letter acronym FTP stand for?

```
file transfer protocol
```

### Task 2

Which port does the FTP service listen on usually?

```
21
```

### Task 3

FTP sends data in the clear, without any encryption. What acronym is used for a later protocol designed to provide similar functionality to FTP but securely, as an extension of the SSH protocol?

```
SFTP
```

### Task 4

What is the command we can use to send an ICMP echo request to test our connection to the target?

```
ping
```

### Task 5

From your scans, what version is FTP running on the target?

```
vsftpd 3.0.3
```

### Task 6

From your scans, what OS type is running on the target?

```
unix
```

### Task 7

What is the command we need to run in order to display the 'ftp' client help menu?

```
ftp -?
```

### Task 8

What is username that is used over FTP when you want to log in without having an account?

```
anonymous
```

### Task 9

What is the response code we get for the FTP message 'Login successful'?

```
230
```

### Task 10

There are a couple of commands we can use to list the files and directories available on the FTP server. One is dir. What is the other that is a common way to list files on a Linux system.

```
ls
```

### Task 11

What is the command used to download the file we found on the FTP server?

```
get
```

### Flag

```
035db21c881520061c53e0536e44f815
```