# Redis

### Command
```
===basic===
start server => redis-server.exe redis.windows.conf
start client => redis-cli(or redis-cli.exe -h 127.0.0.1 -p 6379)

===command===
set <key> <val>
get <key>
keys *
flushall
incr <key> <default 1>
incrby <key> <num>
decr <key> <default 1>
decrby <key> <num>

===sort===
LPUSH <key> <num1 num2 … numn>
RPUSH <key> <num1 num2 … numn>
SORT <key>
SORT <key> DESC

LPUSH <key> <str>
SORT <key> ALPHA

SORT <key> LIMIT <offset> <count>

LPUSH uid <n>
SET <key1_n> <val1>
SET <key2_n> <val2>

SORT uid BY <key_*>
SORT uid GET <key_*>
SORT uid GET <key1_*> GET <key2_*>
SORT uid GET #

HMSET <hash_n> <key1> <val1> <key2> <val2> <key3> <val3> …
SORT uid BY <hash_*>-><key>
SORT uid BY <hash_*>-><key> GET <hash_*>-><key>

LRANGE <list> 0 -1
SORT <list> STORE <list2>

===hash===
HSET <hash> <key> <val>
HDEL <hash> <key>
HEXISTS <hash> <key>
HGET <hash> <key>
HGETALL <hash>
HINCRBY <hash> <key> <num>
HINCRBYFLOAT <hash> <key> <num>
HKEYS <hash>
HLEN <hash>
HMGET <hash> <key1> <key2>
HMSET <hash> [<key> <val> …]
HVALS <hash>
```

### Error
```
Q:Creating Server TCP listening socket 127.0.0.1:6379: bind: No error
A:
redis-cli.exe
shutdown
exit
redis-server.exe
```