#tuling5

## Redis 数据结构应用场景
### string 应用场景
* 单值缓存
```shell
set key value
get key value
```
* 对象缓存
```shell
set user:1 value（json格式）
mset user:1:name huzc user:1:balance 3000
mget user:1:name user:1:balance
```
* 分布式锁
```shell
setnx product:10001 true # 返回 1 代表获取锁成功，返回 0 代表失败
del product:10001 # 执行完业务释放锁
set product:10001 true ex 10 # 防止程序意外终止导致死锁
```
* 计数器
```shell
incr article:readcount:{文章id}
get article:readcount:{文章id}
```

* web 集群 session 共享: spring session + redis 实现 session 共享

* 分布式系统全局序列号
```shell
incrby orderId 1000 # redis 批量生成序列号提升性能
```

### hash 应用场景
* 对象缓存
``` shell
hmset user 1:name huzc 1:balance 3000
hmget user 1:name 1:balance
```
一般一个hash不会存太多的数据，实际生产中会采用`分段`

* 购物车
```shell
# 添加商品
hset cart:[用户id] 商品id 数量
hset cart:1001 10088 1
# 增加数量
hincrby cart:1001 10088 1
# 商品总数
len cart:1001
# 删除商品
hdel cart:1001 10088
# 获取购物车所有商品 
hgetall cart:1001
```

#### 优点
1. 同类数据归类整合存储，方便数据管理
2. 相比 string 操作消耗内存和CPU更小
3. 相比 string 存储更节省空间

#### 缺点
1. 过期功能不能使用在 field 上，只能用在 key 上
2. Redis 集群架构下不适合大规模使用


### list 应用场景
常用数据结构:
1. Stack(栈) = lpush + lpop = fifo
2. Queue(队列) = lpush + rpop
3. Blocking MQ(阻塞队列) = lpush + brpop


* 微博消息和微信公众号消息(清风徐来关注了刘亦菲、周杰伦等大V)
	* 刘亦菲发微博, 消息 ID 为 10018: `lpush msg:{清风徐来ID} 10018`
	* 查看最新微博消息: `lrange msg:{清风徐来ID} 0 4`

### set 应用场景
* 微信抽奖小程序
```shell
# 点击参与抽奖集合
sadd key {userid}

# 查看参与抽奖所有用户
smembers key

# 抽取count名中奖者
srandmember key [count] # 不会删除set中元素
# 或
spop key [count] # 会删除set中元素
```

* 微博微信点赞
```shell
# 点赞
sadd like:{消息id} {用户id}

# 取消点赞
srem like：{消息id}

# 检查用户是否点过赞
sismember like：{消息id} {用户id}

# 获取用户点赞列表
smembers like:{消息id}

# 获取点赞用户数
scard like:{用户id}
```

* 集合操作

![[Pasted image 20220914094107.png]]
```shell
# 交集
sinter set1 set2 set3 -> {c}

# 并集
sunion set1 set2 set3 -> {a,b,c,d,e}

# 差集
sdiff set1 set2 set3 -> {a} # 元素在set1，并且不再set2和set3中 
```