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

**rpush 添加一个或多个到list的尾部(右边)**
和`lpush`类似

**rpushx将一个值插入到列表的尾部**
和`lpushx`类似

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

lrem移除元素
根据参数 COUNT 的值，移除列表中与参数 VALUE 相等的元素
count > 0 : 从表头开始向表尾搜索，移除与 VALUE 相等的元素，数量为 COUNT 。
count < 0 : 从表尾开始向表头搜索，移除与 VALUE 相等的元素，数量为 COUNT 的绝对值。
count = 0 : 移除表中所有与 VALUE 相等的值。
被移除元素的数量。 列表不存在时返回 0
`lrem key_name count value`
```redis
lpush zhangsan lisi wangwu zhangsan lisi
5

lrange user 0 -1
1) "lisi"
2) "zhangsan"
3) "wangwu"
4) "lisi"
5) "zhangsan"

lrem user 2 zhangsan
2

lrange user 0 -1
1) "lisi"
2) "wangwu"
3) "lisi"

```

**ltrim从左边裁剪元素**
list保留 指定的元素列表
`ltrim key_name start_index end_index`
下标 0 表示列表的第一个元素，以 1 表示列表的第二个元素，以此类推。 你也可以使用负数下标，以 -1 表示列表的最后一个元素， -2 表示列表的倒数第二个元素，以此类推
```redis
lpush user zhangsan lisi wangwu
3

lrange user 0 -1
1) "wangwu"
2) "lisi"
3) "zhangsan"

ltrim user 0 1
ok

lrange user  -1
1) "wangwu"
2) "lisi"

```

**rpop 删除列表中的最后一个元素**
返回被移除的元素
如果列表不存在则返回 nil
`rpop key_name`
```redis
lpush user zhangsan lisi wangwu
3

lrange user 0 -1
1) "wangwu"
2) "lisi"
3) "zhangsan"

rpop user
zhangsan

lrange user 0 -1
1) "wangwu"
2) "lisi"
```

**rpoplpush 移除列表的最后一个元素，并将该元素添加到另一个列表(的开始位置[因为使用的是lpush])并返回**
`rpoplpush source_key_name target_key_name`
```redis
lpush user1 zhangsan lisi wangwu
3

lrange user1 0 -1
1) "wangwu"
2) "lisi"
3) "zhangsan"

lpush user2 wangermazi
1

rpoplpush user1 user2
zhangsan

lrange user1 0 -1
1) "wangwu"
2) "lisi"

lrange user2 0 -1
1) "zhangsan"
2) "wangermazi"

```
