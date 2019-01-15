### Sort_Set
有序set,成员不能重复 每一个元素都有一个double类型的score,用此score进行排序
**zadd 添加一个或多个sort_set成员**
`zadd key_name score value1 value2`

```redis
zadd user 1 zhangsan 2 lisi
2
```

**zrange 获取sort_set的成员**
成员会按照score的大小进行排序
可选项`WITHSCORES` 如果有该选项,则会返回value的score到列表中
`zrange key_name start_number stop_number [WITHSCORES]`

```redis
zadd user 1 zhangsan 2 lisi
2

zadd user_2 2 lisi 1 zhangsan
2


zrange user 0 -1
1) "zhangsan"
2) "lisi"

zrange user_2 0 -1
1) "zhangsan"
2) "lisi"

zrange user 0 -1 withscores
1) "zhangsan"
2) 1.0
3) "lisi"
4) 2.0

```

**zcard 获取sort_set中的成员数量**
`zcard key_name`
```redis
zadd user 1 zhangsan 2 lisi
2

zcard user
2

```

**zcount获取sort_set中指定区间score内的成员数量**
`zcount key_name start_score stop_score`
```redis
zadd user 1 zhangsan 2 lisi
2

zcount user 0 100
2

```
**zincrby为sort_set 的value 的score添加一定的值**
`zincrby key_name incy_score value`
其中的`incy_score`可以是负数
如果可以不存在,则会把score设置成为0 再添加`incy_score`
如果`key_name` 不是一个`sort_set` 则会返回一个错误

```
zadd user 1 zhangsan 2 lisi
2

zrange user 0 -1 withscores
1) "zhangsan"
2) 1.0
3) "lisi"
4) 2.0

zincrby user 10 zhangsan
11.0

zrange user 0 -1 withscores
1) "lisi"
2) 2.0
3) "zhangsan"
4) 11.0

sadd user_set zhangsan lisi
2

smembers user_set
1) "zhangsan"
2) "lisi"

zincrby user_set 10 zhangsan
(error) WRONGTYPE Operation against a key holding the wrong kind of value

zincrby not_found_key 10 zhangsan
10

zrange not_found_ley 0 -1 withscores
1) "zhangsan"
2) 10.0

```

**zinterstore 获取两个或多个sort_set并将结果存储到一个新的sort_set中**
`zinertstore new_store_key_name numberkeys key_name_1 key_name_2`
score的值是多个key的score相加
```redis
zadd user 1 zhangsan 2 lisi 3 wangwu
3

zadd user_2 10 zhangsan 11 lisi 12 wangermazi
3

zinterstore new_store_key 2 user1 user2
2

```

**zrangebyscore 通过score的值返回数据**
`(` 大于   `)`小于 直接数值则为`>=`或`<=`
`zrangebyscore key_name exp`
```redis
zadd user 1 zhangsan 2 lisi 3 wangwu
3

zrangebyscore user (1 3
1) "lisi"
2) "wangwu"

```

**zrank 获取store_set中的值对应的index**
`zrank key_name value`

```redis
zadd user 1 zhangsan 2 lisi 30 wangwu
3

zrank user wangwu
2

zrank user zhangsan
0

zrank user lisi
1

```



