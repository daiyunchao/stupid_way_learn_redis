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
