### 订阅发布
> redis中的订阅发布和其他程序中是相同的意思,监听对象 收到推送 取消监听

订阅:
![](http://www.runoob.com/wp-content/uploads/2014/11/pubsub1.png)
`client1` `client2` `client5` 和 `channel1` 建立了关系(`client1` 监听 `channel1`)

发布:
![](http://www.runoob.com/wp-content/uploads/2014/11/pubsub2.png)
当`channel`发布消息后 会推送到 `client1` `client2` `client5`

**subscribe订阅一个或多个channel**
`subscribe channel channel2`
```redis
subscribe user
Reading messages... (press Ctrl-C to quit)
1) "subscribe"                         
2) "user"                                          
3) (integer) 1

```

**unsubscribe 取消订阅channel**
`unsubscribe channel`
```redis
UNSUBSCRIBE mychannel

1) "unsubscribe"
2) "a"
3) (integer) 0

```

**Psubscribe  命令订阅一个或多个符合给定模式的频道**
`PSUBSCRIBE mychannel`
```redis
PSUBSCRIBE mychannel
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "mychannel"
3) (integer) 1
```
**pubsubscribe 退订频道**
`PUNSUBSCRIBE [pattern [pattern ...]]`
```redis
PUNSUBSCRIBE mychannel 
1) "punsubscribe"
2) "a"
3) (integer) 1
```

**publish 向channel发布消息**
`public channel message`
```redis
redis 127.0.0.1:6379> PUBLISH mychannel "hello, i m here"
(integer) 1
```


**pubsub查看订阅和发布系统的状态**
`PUBSUB CHANNELS`

```redis
PUBSUB CHANNELS
(empty list or set)
```

例子(来自[菜鸟教程](http://www.runoob.com/redis/redis-pub-sub.html)):

客户端订阅了一个频道:
```redis
SUBSCRIBE redisChat
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "redisChat"
3) (integer) 1
```
发布消息:

```redis
PUBLISH redisChat "Redis is a great caching technique"

(integer) 1

PUBLISH redisChat "Learn redis by runoob.com"

(integer) 1


# 订阅者的客户端会显示如下消息
1) "message"
2) "redisChat"
3) "Redis is a great caching technique"
1) "message"
2) "redisChat"
3) "Learn redis by runoob.com"

```