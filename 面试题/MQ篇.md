mq篇：

    1.延时队列怎么实现？

    1.1. activemq有延时队列，其他mq有没有呢？手动实现一个mq.
    《Kafka 核心技术与实战》

    2. kafka 怎么保证不丢消息的？
    https://blog.csdn.net/u013256816/article/details/80790185
    product端：

        1. 设置ack=-1，min.insync.replicas=2, 表示必须有两个副本已经写成功了，leader分区也算是一个。如果备份数是3.
        2. 如果insync已同步副本数不足2，就会抛异常，消息没写成功。重试。
    broke端：
        
        1. unclean.leader.election.enable=false 表示isr（In-Sync Replicas）集合集体下线了，不足人数进行选举，那就不选举，免得一些更新数据慢的节点成了leader
        同时也会抛异常。宁缺毋滥。priduct端就知道消息发送不成功，等待。

    3.Kafka 如何保证消息不丢失，不重复，不丢不重？
    Exactly Once实现原理：
        1.product端：
            product向指定topic发消息。会带一个seq number,从0开始递增，然后broker会在内存中维护topic的seqnum,每一个来只能大一，不然就是重复或者丢失消息。
        2.事务保证
            用户提供一个全局性事务id，即使product挂了，重启以后也能通过pid映射找到这个全局事务id，保证了跨session的问题，每次都会通过事务id,找到特定topic记录的seq number。进行步骤一的比较。

    4. Kafka 选主怎么做的？
    5. kafka 与 rabbitmq区别?
    6. kafka 分区副本怎么同步的?
    https://blog.csdn.net/lizhitao/article/details/45066437
    复制机制不是完全同步，也不是完全异步，有一个isr集合，也就是超过半数的分区副本，都已经从leader 那边批量复制了完数据，修改了ISR的high watermark,使得等于或者等于logendoffset, 那就说明product提交的一条数据，在多个分区之间同步完成了，返回ack, 保证了数据的强一致性。


    8. kafka 为什么可以扛住这么高的qps?
    https://sjq597.github.io/2018/08/22/Kafka%E6%91%B8%E9%B1%BC%E7%B3%BB%E5%88%9701-%E7%99%BE%E4%B8%87QPS%E5%90%9E%E5%90%90%E9%87%8F/
    1.存储设计，连续存储：
        顺序读写机械磁盘有时候比随机读写内存的吞吐量还要好.当然这个还归功于CPU的工作方式,在加载数据的时候,CPU会预测，
        连带读取一整块数据,下次读取如果命中,就直接从内存中读,也不用再去加载.
    2.product:
        消息的写入主要由producer完成,首先简单说下Kafka的消息结构,每一个主题topic的消息有多个partition组成,每个partition都会有leader,follower.这里强调一下,不管是producer写数据还是consumer读数据,都是跟leader打交到,follower只负责从对应的leader同步数据.follower和leader一起构成了这个partition的ISR(同步复制队列),如果follower复制没有跟上,会被从ISR中剔除.所以只有当leader节点挂掉的时候,ISR中的follower节点才有可能备胎转正,数据的读写有新的leader节点负责.
        所以说到写数据,就必须要说到Kafka的Ack机制.有时候性能和可靠性本身就是矛盾的,Kafka发送数据光快还不行,还得保证可靠性.
    Ack机制:
        0:表示producer无需等待leader的确认，
        1:代表需要leader确认写入它的本地log并立即确认，
        -1:代表所有的备份都完成后确认
