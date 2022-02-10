#  Redis

* **概念**：Redis是用C语言开发的一个开源的高性能键值对（key-value）数据库
* **NOSQL**：(NoSQL = Not Only SQL) 意即“不仅仅是SQL”，是一项全新的数据库理念，泛指非关系型的数据库。

* **NOSQL和关系型数据库**：关系型数据库与NoSQL数据库并非对立而是互补的关系，即通常情况下使用关系型数据库，在适合使用NoSQL的时候使用NoSQL数据库，



## 数据类型

			1) 字符串类型 string
			2) 哈希类型 hash
			3) 列表类型 list
			4) 集合类型 set
			5) 有序集合类型 sortedset

* string

| 命令          | 说明           | 例子                  |
| ------------- | -------------- | --------------------- |
| set key value | 设置键值       | set username zhangsan |
| get key       | 获取对应键的值 | get username          |
| del key       | 删除           | del username          |



* hash

| 命令                 | 说明                                             | 例子                      |
| -------------------- | ------------------------------------------------ | ------------------------- |
| hset key field value | key 键（变量名），field字段，value值，可以理解成 | hset myhash username lisi |
| hget key field       | 根据对应键，获取其field字段的值                  | hget myhash username      |
| hgetall key          | 获取所有的field和value                           | hgetall myhash            |
| hdel key field       | 删除key的field字段                               | hdel myhash username      |



* list链表

| 命令                 | 说明                                                       | 例子               |
| -------------------- | ---------------------------------------------------------- | ------------------ |
| lpush key value      | 将元素加入到列表的左侧                                     | ...                |
| rpush key value      | 将元素加入到列表的右侧                                     | ...                |
| lrange key start end | 根据范围获取，end -1代表最后一个元素，-2代表倒数第二个元素 | lrange myList 0 -1 |
| lpop key             | 删除最左边的元素并返回                                     |                    |
| rpop key             | 删除最右边元素并返回                                       |                    |



* set集合类型：不允许重复

| 命令           | 说明                    | 例子 |
| -------------- | ----------------------- | ---- |
| sadd key value | 插入                    |      |
| smembers myset | 获取set集合里的所有元素 |      |
| srem key value | 删除set集合中的某个元素 |      |



* sortedset：不允许重复元素，且有顺序

| 命令                              | 说明                                  | 例子               |
| --------------------------------- | ------------------------------------- | ------------------ |
| zadd key score value              | 增加，并且根据浮点数score进行排序     |                    |
| zrange key start end [withscores] | 获取所有的set,withscores==》 附带分数 | zrange mysort 0 -1 |
| zrem key value                    | 删除                                  |                    |



* 通用命令

		1. keys * : 查询所有的键
		2. type key ： 获取键对应的value的类型
		3. del key：删除指定的key value



## 持久化

1. redis是一个内存数据库，服务器重启，数据丢失，所以需要写入硬盘之中。
2. redis持久化机制：
	1. RDB：默认方式，不需要进行配置，默认就使用这种机制
		* 在一定的间隔时间中，检测key的变化情况，然后持久化数据
		1. 编辑redis.windwos.conf文件
			
			**after 900 sec (15 min) if at least 1 key changed**
			
			save 900 1
			
			**after 300 sec (5 min) if at least 10 keys changed**
			
			save 300 10
			
			**after 60 sec if at least 10000 keys changed**
			
			save 60 10000
			
		2. 重新启动redis服务器，并指定配置文件名称
			D:\JavaWeb2018\day23_redis\资料\redis\windows-64\redis-2.8.9>redis-server.exe redis.windows.conf	
		
	2. AOF：日志记录的方式，可以记录每一条命令的操作。可以每一次命令操作后，持久化数据
		1. 编辑redis.windwos.conf文件
			appendonly no（关闭aof） --> appendonly yes （开启aof）
			
			三选一：
			
			appendfsync always ： 每一次操作都进行持久化
			
			appendfsync everysec ： 每隔一秒进行一次持久化
			
			appendfsync no	 ： 不进行持久化

## jedis

* 基本操作：

```
			//1. 获取连接
    		Jedis jedis = new Jedis("localhost",6379);
   			//2. 操作
   			jedis.set("username","zhangsan");
    		//3. 关闭连接
    		jedis.close();
```





### jedis操作各种redis的数据结构



1.  字符串类型 string(**s**)，就是set，get，set的时候可以指定时间

```
	        jedis.setex("activecode",20,"hehe");//将activecode：hehe键值对存入redis，并且20秒后自动删除该键值对
```

2. 哈希类型 hash(**h**) ： map格式  

		        // 存储hash
		        jedis.hset("user","name","lisi");
		        // 获取hash
		        String name = jedis.hget("user", "name");
	           // 获取hash的所有map中的数据
		        Map<String, String> user = jedis.hgetAll("user");

3. list（**l**）

```
			 // list 存储
	        jedis.lpush("mylist","a","b","c");//从左边存
	        jedis.rpush("mylist","a","b","c");//从右边存
	        
	          // list 范围获取
	        List<String> mylist = jedis.lrange("mylist", 0, -1);
	        System.out.println(mylist);
	    
	        // list 弹出
	        String element1 = jedis.lpop("mylist");//c
	        String element2 = jedis.rpop("mylist");//c
```

4. set（**s**）

```
			// set 存储
	        jedis.sadd("myset","java","php","c++");
	      
	        // set 获取
	        Set<String> myset = jedis.smembers("myset");
	        
```

5. sortedset（**z**）：不允许重复元素，且元素有顺序

```
 			// sortedset 存储
	        jedis.zadd("mysortedset",3,"亚瑟");
	        jedis.zadd("mysortedset",30,"后裔");
	        
	        // sortedset 获取
	        Set<String> mysortedset = jedis.zrange("mysortedset", 0, -1);
```



## jedis连接池

* 基本使用

```
//创建连接池对象
JedisPool jedisPool=new JedisPool();

//获取链接
Jedis jedis=jedisPool.getResource();

//使用
jedis.set("123","666");

//关闭(归还到连接池中
jedis.close();
```

jedisPool的构造函数可以传JedisPoolConfig进去



* 配置**JedisPoolConfig**



JedisPoolConfig配置推荐

```
#最大活动对象数     
redis.pool.maxTotal=1000    
#最大能够保持idel状态的对象数      
redis.pool.maxIdle=100  
#最小能够保持idel状态的对象数   
redis.pool.minIdle=50    
#当池内没有返回对象时，最大等待时间    
redis.pool.maxWaitMillis=10000    
#当调用borrow Object方法时，是否进行有效性检查    
redis.pool.testOnBorrow=true    
#当调用return Object方法时，是否进行有效性检查    
redis.pool.testOnReturn=true  
#“空闲链接”检测线程，检测的周期，毫秒数。如果为负值，表示不运行“检测线程”。默认为-1.  
redis.pool.timeBetweenEvictionRunsMillis=30000  
#向调用者输出“链接”对象时，是否检测它的空闲超时；  
redis.pool.testWhileIdle=true  
# 对于“空闲链接”检测线程而言，每次检测的链接资源的个数。默认为3.  
redis.pool.numTestsPerEvictionRun=50  
#redis服务器的IP    
redis.ip=xxxxxx  
#redis服务器的Port    
redis1.port=6379   
```

* 利用配置文件，加载连接池，创建工具类。



