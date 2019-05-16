# 有关Oracle驱动问题

> 参考地址：<http://www.itpub.net/thread-1898386-1-1.html>

.net访问oracle数据库，过去一直在使用微软提供的OracleClient,而微软宣称，从.NET4.0开始放弃对OracleClient的支持，但不会删除，标记为不建议使用。究其原因：ODP.NET全名是Oracle Data Provider，是Oracle发布的供.NET程序访问Oracle数据库的ADO.NET组件，比微软自带的Oracle组件性能好，更可以访问UDT(User Defined Type)类型，Procedure，REF等等高级Oracle特性。

