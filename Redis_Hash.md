### hash
> hash是 字符串key和value的对应关系,适用于存储 object

**hset 设置hash的值**
`hset key_name field1 value1`
```redis
hset zhangsan name 'zhangsan'
1
hgetall zhangsan
1) name
2) lisi
```

**hsetnx只有字段不存在时才能设置成功**
`hsetnx key_name field value`
```redis
hset lisi name lisi
1

hsetnx lisi name wangwu
0

hsetnx lisi age 20
1

hmget lisi name age
1) "lisi"
2) "20"

```
**hmset 批量设置hash 的值**
如果命令执行成功，返回 OK
`hmset key_name field1 field1_value field2 field2_value`
```redis
hmset lisi name 'lisi' age 20
ok

hmget lisi name age
1) "lisi"
2) "20"

hgetall lisi
1) "name"
2) "lisi"
3) "age"
4) "20"

```
**hget 获取field中的值**
`hget key_name field`

```redis
hset zhangsan name 'zhangsan'
1

hset zhangsan age 18
1

hget zhangsan name
zhangsan

hget zhangsan age
18

```

**hmget 获取多个key对应的值**
`hmget key_name field1 field2`
```redis
hset zhangsan name zhangsan
1

hset zhangsan age 18
1

hmget zhangsan name age not_found
1) "zhangsan"
2) "18"
3) (nil)
```

**hgetall 获取对应key的全部field和field的value**
`hgetall key_name`

```redis
hset zhangsan name 'zhangsan'
1

hset zhangsan age 18
1

hgetall zhangsan
1) "name"
2) "zhangsan"
3) "age"
4) "18"

hgetall lisi
(empty list or set)

```

**hvals获取hash中的全部的值**
`hvals key_name`
```redis
hmset zhangsan name 'zhangsan' age 18
ok

hmget zhangsan name age
1) "zhangsan"
2) "18"

hvals zhangsan
1) "zhangsan"
2) "18"
```

**hincrby为数值类型的值+指定的数值,并返回结果**
如果哈希表的 key 不存在，一个新的哈希表被创建并执行 HINCRBY 命令
如果指定的字段不存在，那么在执行命令前，字段的值被初始化为 0
对一个储存字符串值的字段执行 HINCRBY 命令将造成一个错误

`hincrby key_name field number`
```redis

hset zhangsan name 'zhangsan'
1

hset zhangsan age 18
1

hincrby zhangsan age 1
19

hincrby zhangsan age -1
18

hincrby lisi age 10
10

exists lisi
1

hincrby lisi age_new 12
12

hexists lisi age_new
1

hget lisi age_new
12

hget lisi age
10

```

**hincrbyfloat为数值类型的值+指定的浮点类型的数值,并返回结果**
和`hincrby`类似
如果指定的字段不存在，那么在执行命令前，字段的值被初始化为 0 
`hincrbyfloat key field number`

**hkeys 获取hash中的全部的key**
`hkeys key_name`

```redis
hset zhangsan name 'zhangsan'
1

hset zhangsan age 18
1

hkeys zhangsan
1) "name"
2) "age"
```

**hlen获取hash中的key数量**
`hlen key_name`
```redis
hset zhangsan name zhangsan
1

hset zhangsan age 18
1

hget zhangsan name
zhangsan

hlen zhangsan
2

```

**hexists 检查key是否存在**
`hexists key_name field`
当key不存在时,返回0 当查询的field不存在时 返回0 如果存在返回1
```redis
hset zhangsan name 'zhangsan'
1

hexists zhangsan name
1

hexists zhangsan age
0

hexists lisi name
0

```
**hdel删除hash中的一个key或多个key**
返回值:被成功删除字段的数量，不包括被忽略的字段。
`hdel key_name field1 field2`
```redis
hset zhangsan name "zhangsan"
1

hset zhangsan age 18
1

hdel zhangsan name age
(error) wrong number of arguments (given 4, expected 2)

hdel zhangsan name
1

```