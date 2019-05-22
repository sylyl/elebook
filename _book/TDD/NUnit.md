# NUnit

- [TestFixture]表示：类包含了测试代码（这个特性可以被继承）。这个类必须是公有的，这个类还必须有一个默认构造函数

- [Test]表示它是一个测试方法。测试方法的返回值必须为void并且不能带有参数

- [SetUp]属性：用来标识方法，在开始所有测试之前执行，用来在测试前初始化一些资源，比如初始化类

- [TearDown]属性：用来标识方法，在所有测试完成之后执行，用来释放一些资源

  - TestFixtureSetUp/TestFixtureTearDown**和**SetUp和TearDown**相似**

    **不同之处：对于昂贵资源，例如数据库连接，一般都是关键资源。而且多次连接/关闭数据库会造成时间的浪费**

    SetUp和TearDown是每个test方法执行时，都需要执行，而TestFixtureSetUp/TestFixtureTearDown是该类第一个test执行时，执行TestFixtureSetUp，最后一个执行完后，执行TestFixtureTearDown

- [Ignore]属性：用来标识方法，指示这个方法由于某些原因暂时不需要测试（比如没有完成相关代码）

  

### 常用方法

* 测试二个参数是否相等

  ```c#
  Assert.AreEqual(　int　expected,　int　actual　);
  Assert.AreEqual(　decimal　expected,　decimal　actual　);
  ```

* 测试二个参数是否引用同一个对象

  ```c#
  Assert.AreSame(　object　expected,　object　actual　);
  Assert.AreNotSame(　object　expected,　object　actual　);
  ```

* 测试一个对象是否被一个数组或列表所包含

  ```c#
  Assert.Contains(　object　anObject,　IList　collection　);
  ```

* 测试一个对象是否大于另一个对象

  ```
  Assert.Greater(　int　arg1,　int　arg2　);
  ```

* 测试一个对象是否小于另一个对象

  ```
  Assert.Less(　int　arg1,　int　arg2　);
  ```

* 类型断言

  ```
  Assert.IsInstanceOfType(　Type　expected,　object　actual　);
  ```

* 条件测试

  ```
  Assert.IsTrue(　bool　condition　);
  Assert.IsFalse(　bool　condition);
  Assert.IsNull(　object　anObject　);
  Assert.IsNotNull(　object　anObject　);
  Assert.IsNaN(　double　aDouble　);
  Assert.IsEmpty(　string　aString　);
  Assert.IsNotEmpty(　string　aString　);
  Assert.IsEmpty(　ICollection　collection　);
  Assert.IsNotEmpty(　ICollection　collection　);
  ```

*  字符串断言(StringAssert)：提供了许多检验字符串值的有用的方法

  ```
  StringAssert.Contains(　string　expected,　string　actual　);
  StringAssert.StartsWith(　string　expected,　string　actual　);
  StringAssert.EndsWith(　string　expected,　string　actual　);
  StringAssert.AreEqualIgnoringCase(　string　expected,　string　actual　);
  ```

> 参考文档：<http://www.36sign.com/nunit/index.html>

