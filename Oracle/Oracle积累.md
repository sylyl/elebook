# OracleAQ

> OracleAQ意味着Oracle Advanced Queuing.作为Oracle数据库产品组件，我们可以不需要安装第三方中间件来达到系统交互的目的。

**点对点模型**

两个系统一起使用一个或者多个queue，这种模式称为点对点模型。

把消息输入到queue称为入列（Enqueue），相反，称之为出列（Dequeue）。一条消息只能被一个使用这个queue的系统Dequeue。

**Java中使用Oracle native AQ**

使用Oracle JPublisher创建一个Oracle数据类型对应的Java类。

<<<<<<< HEAD
# Java thin和oci区别

**参考地址**：<https://blog.csdn.net/a327736051/article/details/48766171>

**语法格式**

```
jdbc:oracle:thin:@server ip: service 
jdbc:oracle:oci:@service 
```

**结论**：

> 1）从使用上来说，oci必须在客户机上安装oracle客户端或才能连接，而thin就不需要，因此从使用上来讲thin还是更加方便，这也是thin比较常见的原因。 
> 2）原理上来看，thin是纯java实现tcp/ip的c/s通讯；而oci方式,客户端通过native java method调用c library访问服务端，而这个c library就是oci(oracle called interface)，因此这个oci总是需要随着oracle客户端安装（从oracle10.1.0开始，单独提供OCI Instant Client，不用再完整的安装client） 
> 3）它们分别是不同的驱动类别，oci是二类驱动， thin是四类驱动，但它们在功能上并无差异。 
> 4）虽然很多人说oci的速度快于thin，但找了半天没有找到相关的测试报告。

# Oracle日志截断

**Oracle日志所在目录**：

```
For oracle 9i/10g $ORACLE_HOME/network/log/listener_$ORACLE_SID.log
For oracle 11g/12c $ORACLE_BASE/diag/tnslsnr/主机名称/listener/trace/listener.log
```

**首先停止监听服务进程（tnslsnr）记录日志。**

```
oracle@entel2:[/oracle]$lsnrctl  set log_status off
```

**将监听日志文件（listener.log）复制一份，以listener.log.yyyymmdd格式命名**

```
oracle@entel2:[/oracle]$cp listener.log listener.log.20161201
```

**将监听日志文件（listener.log）清空。清空文件的方法有很多**

```
oracle@entel2:[/oracle]$echo “” > listener.log
或者
oracle@entel2:[/oracle]$cp /dev/null listener.log
或者
oracle@entel2:[/oracle]$echo /dev/null > listener.log
或者
oracle@entel2:[/oracle]$>listener.log
```

**开启监听服务进程（tnslsnr）记录日志**

```
oracle@entel2:[/oracle]$lsnrctl set log_status on
```

**日志迁移**

```
oracle@entel2:[/oracle]$ lsnrctl set log_status off

oracle@entel2:[/oracle]$mv listener.log listener.yyyymmdd

oracle@entel2:[/oracle]$lsnrctl set log_status on
```

=======
>>>>>>> e116b72388ea139a6b76bc4c1765d878367f5543
