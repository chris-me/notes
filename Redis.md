## Commands

### Delete all Keys beginning with 'foobar:'

```
EVAL "return redis.call('del', 'defaultKey', unpack(redis.call('keys', ARGV[1])))" 0 foobar:*
```

### Delete all the keys of all the existing databases (async)

```
FLUSHALL
```
