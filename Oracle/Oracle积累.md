# OracleAQ

> OracleAQ意味着Oracle Advanced Queuing.作为Oracle数据库产品组件，我们可以不需要安装第三方中间件来达到系统交互的目的。

**点对点模型**

两个系统一起使用一个或者多个queue，这种模式称为点对点模型。

把消息输入到queue称为入列（Enqueue），相反，称之为出列（Dequeue）。一条消息只能被一个使用这个queue的系统Dequeue。

**Java中使用Oracle native AQ**

使用Oracle JPublisher创建一个Oracle数据类型对应的Java类。

