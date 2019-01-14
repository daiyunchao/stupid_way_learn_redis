### set
> Set 是 String 类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据

**sadd添加一个或多个值到set中**
如果key不存在,则创建一个
如果key不存在而不是set类型,则返回一个错误
`sadd key_name value1 value2`
```redis
sadd user zhangsan lisi
2

smembers user
1) "zhangsan"
2) "lisi"

set name zhangsan
ok

sadd name lisi
(error) WRONGTYPE Operation against a key holding the wrong kind of value

```

**smembers 获取成员列表**
`smembers key_name`
```redis
sadd user zhangsan lisi
2

smembers user
1) "zhangsan"
2) "lisi"

```

**sismember 判断是否是一个set的成员**
`sismember key_name value`
```redis
sadd user zhangsan lisi
2

sismember user zhangsan
1

sismember user not_found_value
0

```

**scard获取成员数量**
当key不存在时,返回0
`scard key_name`
```redis
sadd user zhangsan lisi
2

scard user
2

scard not_found_key
0

```

**sdiff比较两个set中的差别**
理解: 查询第一个set中出现而第二个set中没出现的值
如果key不存在,则认为该set是空
`sdiff key_name_1 key_name_2`
```redis
sadd user_1 zhangsan lisi wangwu
3

sadd user_2 lisi zhaoliu
2

sdiff user_1 user_2
1) "zhangsan"
2) "wangwu"

```

**sinter获取两个(多个)set的交集**
如果key不存在,则认为该set是空
`sinter key_name_1 key_name_2`

```redis
sadd user_1 zhangsan lisi wangwu
3

sadd user_2 lisi zhaoliu
2

sdiff user_1 user_2
1) "lisi"

```

**smove将成员从一个set移动到另外一个set去**
`smove key_name key_name2 filed`
```redis
sadd user zhangsan lisi
2

sadd user_2 wangwu zhaoliu
2

smove user user_2 lisi
1

smembers user
1) "zhangsan"

smembers user_2
1) "lisi"
2) "zhaoliu"
3) "wangwu"

```

**spop 删除set中的一个或多个元素并返回**
`spop key_name [count]`
```redis
sadd user_2 wangwu zhaoliu lisi
3

spop user_2
wangwu

smembers user_2
1) "lisi"
2) "zhaoliu"

```

**srandmember 随机返回一个或多个set中的元素**
`srandmember key_name [count]`
从 Redis 2.6 版本开始， Srandmember 命令接受可选的 count 参数：
如果 count 为正数，且小于集合基数，那么命令返回一个包含 count 个元素的数组，数组中的元素各不相同。如果 count 大于等于集合基数，那么返回整个集合。
如果 count 为负数，那么命令返回一个数组，数组中的元素可能会重复出现多次，而数组的长度为 count 的绝对值。
该操作和 SPOP 相似，但 SPOP 将随机元素从集合中移除并返回，而 Srandmember 则仅仅返回随机元素，而不对集合进行任何改动。

```redis
sadd user zhangsan lisi wangwu zhaoliu
4

srandmember user
zhangsan

srandmember user 2
lisi
zhaoliu

srandmember user 100
1) "zhangsan"
2) "lisi"
3) "zhaoliu"
4) "wangwu"

srandmember user -2
zhangsan
lisi

srandmember user -2
zhangsan
zhangsan

```

**srem 移除set中的一个或多个元素**
`srem key_name member1 member2`
Redis Srem 命令用于移除集合中的一个或多个成员元素，不存在的成员元素会被忽略。
当 key 不是集合类型，返回一个错误。
```redis
sadd user zhangsan lisi wangwu zhaoliu
4

srem user zhangsan lisi
2

smembers user
1) "zhaoliu"
2) "wangwu"

```
**sunion合区两个或多个set的并集**
`sunion key_name key_name_2`
```redis
sadd user zhangsan lisi 
2

sadd user_2 wangwu zhaoliu
2

sunion user user_2
1) "zhangsan"
2) "lisi"
3) "zhaoliu"
4) "wangwu"

```

**sunionstore 找到并集并存储到另外的set中**
`sunionstore key_name key_name2 key_name3`
```redis
sadd user zhangsan lisi 
2

sadd user_2 wangwu zhaoliu
2

sunionstore user user_2 my_user
2
```

**sscan迭代set中的元素**
`不太理解cursor参数`
`SSCAN key cursor [MATCH pattern] [COUNT count]`
```redis
sadd user zhangsan lisi wangwu zhaoliu
4

sscan user 0 z*
1) "0"
2) 1) "zhangsan"
2) "zhaoliu"

```