
# MONGOD

![alt text](image.png)

#mongodb #databases #reconnaissance #misconfiguration #anonymous/guestaccess

## 1. Méthodologie

Je commence toujours par la phase d'énumération avec la commande `nmap -sV -p- IP_cible`:

```
└──╼ [★]$ nmap -sV -p- 10.129.167.172
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-23 05:45 CST
Nmap scan report for 10.129.167.172
Host is up (0.0096s latency).
Not shown: 65533 closed tcp ports (reset)
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
27017/tcp open  mongodb MongoDB 3.6.8
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Le port SSH est ouvert ainsi que le port 27017, sur lequel nous trouvons une database MongoDB. Nous allons tenter de se connecter au service MongoDB présent: `mongosh IP_cible`:

```
└──╼ [★]$ mongosh 10.129.167.172
Current Mongosh Log ID:	67bb0af542463ded95fa3f9d
Connecting to:		mongodb://10.129.167.172:27017/?directConnection=true&appName=mongosh+1.10.6
Using MongoDB:		3.6.8
Using Mongosh:		1.10.6

For mongosh info see: https://docs.mongodb.com/mongodb-shell/


To help improve our products, anonymous usage data is collected and sent to MongoDB periodically (https://www.mongodb.com/legal/privacy-policy).
You can opt-out by running the disableTelemetry() command.

------
   The server generated these startup warnings when booting
   2025-02-23T11:44:20.394+0000: 
   2025-02-23T11:44:20.394+0000: ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
   2025-02-23T11:44:20.394+0000: **          See http://dochub.mongodb.org/core/prodnotes-filesystem
   2025-02-23T11:44:21.904+0000: 
   2025-02-23T11:44:21.904+0000: ** WARNING: Access control is not enabled for the database.
   2025-02-23T11:44:21.904+0000: **          Read and write access to data and configuration is unrestricted.
   2025-02-23T11:44:21.904+0000:
------

test> 
```
On va maintenant lister les databases présentes avec la commande Mongo `show dbs` qui va lister les databases présentes:

```
test> show dbs
admin                  32.00 KiB
config                 72.00 KiB
local                  72.00 KiB
sensitive_information  32.00 KiB
users                  32.00 KiB
test> 
```

On va maintenant s'attacher à la base de données trouvée avec la commande `use DB_name`:

```
test> use admin
switched to db admin
admin> 
```

Maintenant, nous pouvons tenter de voir les collections associées à cette database avec la commande `show collections` :

```
admin> show collections
system.version
admin> 
```
Nous cherchons un flag donc je vais continuer à explorer les databases jusqu'à trouver une entrée flag.

```
admin> use sensitive_information
switched to db sensitive_information
sensitive_information> show collections
flag
sensitive_information> 
```

Je dois maintenant afficher cette entrée avec la commande: `db.file_name.find().pretty(.)`. Cela permet d'afficher de manière lisible l'information contenue dans l'entrée flag.

```
sensitive_information> db.flag.find().pretty()
[
  {
    _id: ObjectId("630e3dbcb82540ebbd1748c5"),
    flag: '1b6e6fb359e7c40241b6d431427ba6ea'
  }
]
sensitive_information> 
```

## 2. Questions

### Task 1

How many TCP ports are open on the machine?

```
2
```

### Task 2

Which service is running on port 27017 of the remote host?

```
mongodb 3.6.8
```

### Task 3

What type of database is MongoDB? (Choose: SQL or NoSQL)

```
NoSQL
```

### Task 4

What is the command name for the Mongo shell that is installed with the mongodb-clients package?

```
mongosh
```

### Task 5

What is the command used for listing all the databases present on the MongoDB server? (No need to include a trailing ;)

```
show dbs
```

### Task 6

What is the command used for listing out the collections in a database? (No need to include a trailing ;)

```
show collections
```

### Task 7

What is the command used for dumping the content of all the documents within the collection named flag in a format that is easy to read?

```
db.flag.find().pretty()
```

### Flag

```
1b6e6fb359e7c40241b6d431427ba6ea
```