### yii-queue队列，分析redis

job是怎么样缓存到redis中的

* 生产者：通过serialize将实例化后的对象缓存到redis的hash中
* 消费者：通过unserialize取出对象执行