### 参数说明

#### maxActive

控制一个pool可分配多少个jedis实例,通过pool.getResource()来获取；如果赋值为-1，则表示不限制；如果pool已经分配了maxActive个jedis实例，则此时pool的状态为exhasuted。

#### maxIdle

控制一个pool最多有多少个状态为idle的jedis实例。

#### whenExhaustedAction

表示当pool中的jedis实例都被allocate完时，pool要采取的操作；默认有三种

- WHEN_EXHAUSTED_FAIL 表示无jedis实例时，直接抛出NoSuchElementExecption
- WHEN_EXHAUSTED_BLOCK 表示阻塞住，或者达到maxWait时爆出JedisConnectionException
- WHEN_EXHAUSTED_GROW 表示新建一个jedis实例，也就是说设置的maxActive无用

#### maxWait

表示当borrow一个jedis实例时，最大的等待时间，如果超过等待时间，则直接抛出JedisConnectionException

#### testOnBorrow 

获得一个jedis实例的时候是否检查连接可用性(ping());如果为true，则得到的jedis实例均为可用的。

#### testOnReturn

return 一个jedis实例给pool时，是否检车连接的可用性(ping())；



