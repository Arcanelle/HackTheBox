
# REDEEMER

![alt text](image.png)

#redis, #vulnerability assessment, #databases, #reconnaissance, #anonymous/guest access

## 1. Méthodologie

On commence par l'énumération complète avec la commande `nmap -sV IP_cible -p-`

```
└──╼ [★]$ nmap -sV -p- 10.129.169.171
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-22 12:42 CST
Nmap scan report for 10.129.169.171
Host is up (0.0085s latency).
Not shown: 65534 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
6379/tcp open  redis   Redis key-value store 5.0.7
```

Redis est une database qui possède son langage, je vais donc essayer de communiquer avec elle, d'abord avec la commande `redis-cli -h IP_cible` puis `info` afin d'obtenir des informations sur ce serveur:

```
└──╼ [★]$ redis-cli -h 10.129.169.171
10.129.169.171:6379> info
# Server
redis_version:5.0.7
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:66bd629f924ac924
redis_mode:standalone
os:Linux 5.4.0-77-generic x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:9.3.0
process_id:751
run_id:8cf679e871b12b7ba0ffef53b965f33e014c7b56
tcp_port:6379
uptime_in_seconds:213
uptime_in_days:0
hz:10
configured_hz:10
lru_clock:12196678
executable:/usr/bin/redis-server
config_file:/etc/redis/redis.conf

# Clients
connected_clients:1
client_recent_max_input_buffer:4
client_recent_max_output_buffer:0
blocked_clients:0

# Memory
used_memory:859624
used_memory_human:839.48K
used_memory_rss:6012928
used_memory_rss_human:5.73M
used_memory_peak:859624
used_memory_peak_human:839.48K
used_memory_peak_perc:100.00%
used_memory_overhead:846142
used_memory_startup:796224
used_memory_dataset:13482
used_memory_dataset_perc:21.26%
allocator_allocated:1614488
allocator_active:1949696
allocator_resident:9158656
total_system_memory:2084024320
total_system_memory_human:1.94G
used_memory_lua:41984
used_memory_lua_human:41.00K
used_memory_scripts:0
used_memory_scripts_human:0B
number_of_cached_scripts:0
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:1.21
allocator_frag_bytes:335208
allocator_rss_ratio:4.70
allocator_rss_bytes:7208960
rss_overhead_ratio:0.66
rss_overhead_bytes:-3145728
mem_fragmentation_ratio:7.35
mem_fragmentation_bytes:5195312
mem_not_counted_for_evict:0
mem_replication_backlog:0
mem_clients_slaves:0
mem_clients_normal:49694
mem_aof_buffer:0
mem_allocator:jemalloc-5.2.1
active_defrag_running:0
lazyfree_pending_objects:0

# Persistence
loading:0
rdb_changes_since_last_save:4
rdb_bgsave_in_progress:0
rdb_last_save_time:1740249713
rdb_last_bgsave_status:ok
rdb_last_bgsave_time_sec:-1
rdb_current_bgsave_time_sec:-1
rdb_last_cow_size:0
aof_enabled:0
aof_rewrite_in_progress:0
aof_rewrite_scheduled:0
aof_last_rewrite_time_sec:-1
aof_current_rewrite_time_sec:-1
aof_last_bgrewrite_status:ok
aof_last_write_status:ok
aof_last_cow_size:0

# Stats
total_connections_received:6
total_commands_processed:7
instantaneous_ops_per_sec:0
total_net_input_bytes:320
total_net_output_bytes:14861
instantaneous_input_kbps:0.00
instantaneous_output_kbps:0.00
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:0
expired_stale_perc:0.00
expired_time_cap_reached_count:0
evicted_keys:0
keyspace_hits:0
keyspace_misses:0
pubsub_channels:0
pubsub_patterns:0
latest_fork_usec:0
migrate_cached_sockets:0
slave_expires_tracked_keys:0
active_defrag_hits:0
active_defrag_misses:0
active_defrag_key_hits:0
active_defrag_key_misses:0

# Replication
role:master
connected_slaves:0
master_replid:14215e216d1001bc3db8f31c154d039927a9a783
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

# CPU
used_cpu_sys:0.187386
used_cpu_user:0.079309
used_cpu_sys_children:0.000000
used_cpu_user_children:0.000000

# Cluster
cluster_enabled:0

# Keyspace
db0:keys=4,expires=0,avg_ttl=0
```

J'ai une info intéressante ici, tout à la fin de la commande, j'apprends qu'il y a 4 clés dans la database à l'index 0. Je vais sélectionner la database par son index 0 `select 0` afin de voir les clés qu'elle contient `keys *` :

```
10.129.169.171:6379> select 0
OK
10.129.169.171:6379> keys *
1) "flag"
2) "temp"
3) "numb"
4) "stor"
```
Je trouve ainsi la clé "flag" qui contiendra notre flag. Je n'ai plus qu'à afficher la clé avec la commande `get key_name` :

```
10.129.169.171:6379> get flag
"03e1d2b376c37ab3f5319922053953eb"
```

## 2. Questions

### Task 1

Which TCP port is open on the machine?

```
6379
```

### Task 2

Which service is running on the port that is open on the machine?

```
redis
```

### Task 3

What type of database is Redis? Choose from the following options: (i) In-memory Database, (ii) Traditional Database

```
in-memory database
```

### Task 4

Which command-line utility is used to interact with the Redis server? Enter the program name you would enter into the terminal without any arguments.

```
redis-cli
```

### Task 5

Which flag is used with the Redis command-line utility to specify the hostname?

```
-h
```

### Task 6

Once connected to a Redis server, which command is used to obtain the information and statistics about the Redis server?

```
info
```

### Task 7

What is the version of the Redis server being used on the target machine?

```
5.0.7
```

### Task 8

Which command is used to select the desired database in Redis?

```
select
```

### Task 9

How many keys are present inside the database with index 0?

```
4
```

### Task 10

Which command is used to obtain all the keys in a database?

```
keys *
```

### Flag

```
03e1d2b376c37ab3f5319922053953eb
```