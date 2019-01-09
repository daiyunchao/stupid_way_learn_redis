### list
> 简单字符串列表,按照插入顺序存储

**lpush添加一个或多个到list中的头部**
`lpush key_name value1 value2`
```redis
lpush user zhangsan lisi
2

lrange user 0 -1
1) "lisi"
2) "zhangsan"

```
**lpushx 将一个值插入到列表的头部**
`lpushx key_name value1`
当key不存在时会出现错误
```redis
lpushx not_found_key zhangsan
0

lpush user zhangsan lisi
2

lpushx user wangwu
3

lrange user 0 -1
1) "wangwu"
2) "lisi"
3) "zhangsan"

```
`lpush key_name value1 value2`
如果 key 不存在，一个空列表会被创建并执行 LPUSH 操作。 当 key 存在但不是列表类型时，返回一个错误
```redis

```

**linsert 插入值到列表中**
`linsert key_name before|after old_value new_value`
列表的元素前或者后插入元素。当指定元素不存在于列表中时，不执行任何操作。
当列表不存在时，被视为空列表，不执行任何操作。
如果 key 不是列表类型，返回一个错误。

```redis
lpush user zhangsan lisi
2

lrange user 0 -1
1) "lisi"
2) "zhangsan"

linsert user before lisi 'wangwu'
3

lrange user 0 -1
1) "wangwu"
2) "lisi"
3) "zhangsan"

linsert user after zhangsan 'mazi'
4

lrange user 0 -1
1) "wangwu"
2) "lisi"
3) "zhangsan"
4) "mazi"

linsert not_found before not_found_filed 'new_value'
0

set username zhangsan
ok

get username
zhangsan

linsert username after zhangsan 'lisi'
(error) WRONGTYPE Operation against a key holding the wrong kind of value

```

**llen 获取list的长度**
`llen key_name`
```redis
lpush user zhangsan lisi wangwu
3

llen user
3

```


**lrange 获取list中指定范围内的value**
`lrange key_name start_num end_num`
区间以偏移量 START 和 END 指定。 其中 0 表示列表的第一个元素， 1 表示列表的第二个元素，以此类推。 你也可以使用负数下标，以 -1 表示列表的最后一个元素， -2 表示列表的倒数第二个元素，以此类推
获取全部元素列表的方法是:
`lrange key_name 0 -1`

```redis
lpush user zhangsan lisi
2

lrange user 0 -1
1) "lisi"
2) "zhangsan"
```

**lindex 通过索引获取列表中的元素**
`lindex key_name index_num`
```redis
lpush user zhangsan lisi
2

lindex user 0
lisi

lindex user 1
zhangsan

lindex user -1
zhangsan

```

**lpop 移除并返回第一个元素**
`lpop key_name`
```redis
lpush user zhangsan lisi
2

lrange user 0 -1

1) "lisi"
2) "zhangsan"

lpop user
lisi

```

**blpop 移除list中的第一个元素并返回**
如果没有则会被阻塞列表直到等待超时或发现可弹出元素为止
`blpop key_name timeout_num`
```redis
lpush user zhangsan lisi
2

lrange user 0 -1
1) "lisi"
2) "zhangsan"

blpop user 10
lisi

BLPOP list1 100
(nil)
(100.06s)
```

**brpop 移除list中的最后一个元素并返回**
如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止
和`blpop`类似,但`blpop`是从第一个开始(左边) 该方法是从(最后一个)右开始

