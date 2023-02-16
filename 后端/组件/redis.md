redis

常用命令



①：连接服务端：
    ./redis-cli -h 127.0.0.1 -p 6379
②：Redis默认是有16个数据库的（0~15）通过select命令来切换数据库
    select 1    -- 连接到第 2 个数据库 0开始计算
③：往数据库设置string类型值
    set name zhangsan
④：查看数据库中key的数量
    dbsize
⑤：查看刚才添加的key的值
    get name
⑥：查看所有key的值
    keys *
⑦：清空全部数据库和清空当前库
    flushall（清空全部库） flushdb（清空当前库）  
⑧：删除添加的name key键
    del name







