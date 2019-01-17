### hyperloglog
> 统计基数

**pfadd 添加元素到hyperloglog**
`pfadd key_name element`
添加成功 返回1
没有任何元素添加上去 返回0

```redis
pfadd user zhangsan lisi wangwu zhangsan
1

pfadd user zhangsan lisi wangwu zhangsan
0

```

**pfcount获取基数**
*可检查多个结构体,返回的数量时多个结构体(基数去重)和*
`pfcount key_name key_name_2`
```redis
pfadd user zhangsan lisi wangwu
3

pfcount user
3

pfadd user_2 zhangsan lisi wangwu

pfcount user user_2
3

```

**pfmerge合并htyperloglog到一个新的结构体中**

`pfmerge sotre_key key_name key_name2 key_name3`
```redis
pfadd user zhangsan lisi wanagwu
3

pfadd user_2 a b c d
4

pfadd user_3 zhangsan a b wangwu
4

pfmerge new_user user user_2 user_3
ok

pfcount new_user
7

```