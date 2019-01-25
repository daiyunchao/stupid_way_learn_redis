## 基础部分总结
> 通过这些天的redis基础部分的学习,基本了解了redis的数据结构和一些常用的操作命令,简单点,基础部分就是key-value的设置值和取值

### 数据结构:
字符串:`Sting`
对象:`Hash`
简单有序字符串列表: `List`
无序去重字符串列表: `Set`
有序去重字符串列表: `ZSet`(Set的升级版)

#### 常用操作汇总:

#### 基础部分,和数据类型无关:

删除key
`del key`

判断key是否存在
`exists key`

设置一个key的过期时间(单位是秒)
`expire key` 在字符串中类似于 `setex`

设置一个key的过期时间(单位是毫秒)
`pexpire key` 在字符串中类似于 `psetex`

为key设置一个过期时间
`expireat key`

获取当前的全部key
`keys *`

移除key的过期时间
`persist key`

```redis
set username 'zhangsan'
ok

expire username 10
1

ttl username
7(返回的是还剩下的时间,假如使用了3秒)

persist username
1

ttl username
-1

```

ttl以秒为单位返回key的生命剩余时间
`ttl key_name`

pttl以毫秒为单位返回key的生命剩余时间
`pttl key_name`

rename修改 key 的名称
`rename key newkey`

renamenx仅当 newkey 不存在时，将 key 改名为 newkey
`renamenx key newkey`

type返回key的数据类型
`type key`
none(key不存在) string(字符串) list(列表) set(集合) zset(有序集合) hash(哈希表)

#### `String部分`:

存储一个string
`set key value`

获取一个string
`get key value`

获取字符串的一部分getrange
`getrange username 0 3`

getset key 设置一个新的值,并且返回旧值
`set username zhangsan`
`getset username 'lisi'`
`zhangsan`

setnx 只有当key不存在时才能设置其值
`setnx key_name value`

mset 设置多个值
`mset name1 'zhangsan' name2 'lisi' name3 'wangwu'`

mget 获取多个值
`mget name1 name2 name3`

msetnx设置多个key对应的值,但只有当key不存在时才能设置(原子操作,要成功都成功,要失败都失败)
`msetnx key1 value1 key2 value2`

setex设置一个值并且设置它的过期时间(单位秒)
`setex key_name timeout value`

ttl获取值的剩余时间(单位秒)
`ttl key_name`

psetex设置一个值并且设置它的过期时间(单位毫秒)
`psetex key_name timeout value`

pttl获取值的剩余时间(单位毫秒)
`pttl key_name`

setrange,截取字符串,拼接字符串
`SETRANGE KEY_NAME OFFSET VALUE`
```redis
set username zhangsan
ok

get username
zhangsan

setrange username 0 'he name called'

get username
he name called

setrange username 14 ' zhangsan'
he name called zhangsan

```

append字符串的相加功能
`append key_name value`


获取存储字符串值的长度strlen
`strlen key_name`

incr数值类型的值加一
`incr key_name`

incrby为数值类型的值+指定的数值
`incrby key_name number`

decr数值类型减一
`decr key_name`

decrby为数值类型的值-指定的数值
`decr key_name num`



#### `Hash部分`:

hset 设置hash的值
`hset key_name field1 value1`

hmset 批量设置hash 的值
`hmset key_name field1 field1_value field2 field2_value`

hsetnx只有字段不存在时才能设置成功
`hsetnx key_name field value`

hget 获取field中的值
`hget key_name field`

hmget 获取多个key对应的值
`hmget key_name field1 field2`

hgetall 获取对应key的全部field和field的value
`hgetall key_name`
```redis
hgetall zhangsan
1) "name"
2) "zhangsan"
3) "age"
4) "18"
```

hvals获取hash中的全部的值
`hvals key_name`
```redis
hvals zhangsan
1) "zhangsan"
2) "18"

```

hlen获取hash中的key数量
`hlen key_name`

hexists 检查key是否存在
`hexists key_name field`


hdel删除hash中的一个key或多个key
`hdel key_name field1 field2`


#### `List部分`:

lpush添加一个或多个到list中的头部
`lpush key_name value1 value2`


lpushx 向一个已存在的key中插入值,并放到头部
`lpushx key_name value1`

rpush 添加一个或多个到list的尾部(右边)
`lpush`类似

rpushx将一个值插入到列表的尾部
`lpushx`类似

linsert 插入指定顺序的值到列表中
`linsert key_name before|after old_value new_value`

llen 获取list的长度
`llen key_name`

lrange 获取list中指定范围内的value
`lrange key_name start_num end_num`
`lrange key_name 0 -1` 获取全部元素

lindex 通过索引获取列表中的元素
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

lpop 移除一个元素并返回
`lpop key_name`

ltrim从左边裁剪元素
`ltrim key_name start_index end_index`

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

rpop 删除列表中的最后一个元素
`rpop key_name`


