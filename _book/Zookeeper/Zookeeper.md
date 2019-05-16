# Zookeeper

**官方文档地址**：<https://zookeeper.apache.org/doc/current/zookeeperOver.html>

**设计目标**

* Zookeeper是可复制的

![pic](D:\电子书\Zookeeper\zkservice.jpg)

> ​	They maintain an in-memory image of state, along with a transaction logs and snapshots in a persistent store. As long as a majority of the servers are available, the ZooKeeper service will be available.

- Zookeeper是有序的

>  ZooKeeper stamps each update with a number that reflects the order of all ZooKeeper transactions. Subsequent operations can use the order to implement higher-level abstractions, such as synchronization primitives.

- Zookeeper以读取为主的方式表现得更为突出

> It is especially fast in "read-dominant" workloads. ZooKeeper applications run on thousands of machines, and it performs best where reads are more common than writes, at ratios of around 10:1.

- Zookeeper每个节点以‘/‘路径命名（好像是B树？有待考证）,节点存在session，session结束时，节点就会被删除，这种临时性的节点方式有助于实现tbd（仿真场景检测前跟踪）

![pic](D:\电子书\Zookeeper\zknamespace.jpg)

- Zookeeper节点可以设置监控
- Zookeeper组件

> With the exception of the request processor, each of the servers that make up the ZooKeeper service replicates its own copy of each of the components.

![pic](D:\电子书\Zookeeper\zkcomponents.jpg)

- Zookeeper配置文件**conf/zoo.cfg**

  - 独立配置

    ```
    tickTime=2000
    dataDir=/var/lib/zookeeper
    clientPort=2181
    ```

    

  - 多配置

    ```
    tickTime=2000
    dataDir=/var/lib/zookeeper
    clientPort=2181
    initLimit=5
    syncLimit=2
    server.1=zoo1:2888:3888
    server.2=zoo2:2888:3888
    server.3=zoo3:2888:3888
    ```

