### 事务
> 不像数据库事务没有数据事务的原子性
> 特点:批量操作在发送 EXEC 命令前被放入队列缓存,收到 EXEC 命令后进入事务执行，事务中任意命令执行失败，其余的命令依然被执行(非原子性).在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中(保证顺序)


**multi 标记一个事务的开始**
`multi`
总是返回 OK 

```redis
multi
ok

set user zhangsan
QUEUED

set user_2 lisi
QUEUED

exec
1) OK
2) OK

get user
zhangsan 

get user_2
lisi

```

**exec执行批量任务(事务)**
`exec`
事务块内所有命令的返回值，按命令执行的先后顺序排列。 当操作被打断时，返回空值 nil 。

```redis
multi
ok

set user zhangsan
QUEUED

set user_2 lisi
QUEUED

exec
1) OK
2) OK

get user
zhangsan 

get user_2
lisi

```

**watch监控一个或多个key,如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断**
`watch key key_2`

```redis
WATCH lock lock_times
(在tryredis中执行报错)
```

**unwatch取消监听**
`unwatch`
```redis
unwatch
OK
(在tryredis中执行报错)
```

**discard取消事务,取消事务中的全部命令**
`discard`

```redis
multi
ok

set user zhangsan 
QUEUED

set user_2 lisi
QUEUED

discard
ok

get user
(nil)

get user_2
(nil)
```
