### 连接
> 连接命令主要是用于连接 redis 服务

**auth 命令用于检测给定的密码和配置文件中的密码是否相符**
`AUTH PASSWORD`
密码匹配时返回 OK ，否则返回一个错误。

**echo 打印信息**
`echo message`
```redis
echo zhangsan
zhangsan

```

**PING 查看和服务器端连接是否正常**
`ping`
连接正常: 返回`PONG` 如果不能则返回
`Could not connect to Redis at 127.0.0.1:6379: Connection refused`

```redis
ping
pong

```

**quit 关闭当前连接**
`QUIT`
一旦所有等待中的回复(如果有的话)顺利写入到客户端，连接就会被关闭。
```redis
QUIT
ok
```

**SELECT 命令用于切换到指定的数据库，数据库索引号 index 用数字值指定，以 0 作为起始索引值**
`SELECT index `
总是返回 OK 。

```redis
SET db_number 0 
ok

SELECT 1
ok
```