### NodeJs如何使用Redis
> NodeJs如何使用Redis

使用模块: `redis`

#### 安装redis模块
`npm install redis`

#### 创建客户端连接
```javascript
const redis = require('redis')
const client = redis.createClient(6379, 'localhost')
```

#### 使用
``` javascript
client.set('hello', {a:1, b:2}) // 注意，value会被转为字符串,所以存的时候要先把value 转为json字符串
client.get('hello', function(err, value){
    console.log(value)
})

```

其他的使用命令和之前学到的命令相同