# sworm

A very lightweight ***\*write only\**** Node.js ORM, with support for:

\* Microsoft SQL Server (MSSQL)

\* PostgreSQL

\* MySQL

\* Oracle DB

\* Sqlite 3

\* Browser Web SQL

**本文使用OracleDB，吐槽几点**：**（但是，作者还是牛逼的，辛苦了！！！）**

* 整体使用风格，个人感觉还是更适合MySQL、PostgreSQL
* 文档说明有些地方还是模棱两可、也可能是本人理解能力有限 
* 维护更新基本停滞......

**首先，安装数据库驱动** ` npm install oracledb`

**然后，常规操作** `npm i sworm` **鄙人版本****3.6.0**

### Oracle

* conn

  ```
  var sworm = require('sworm');
  var db = sworm.db({
      driver: 'oracle',
      config: {
          user: 'GZH',
          password: 'GZH',
          connectString: '192.168.1.143/orcl',
          pool: true,
          options: {
              // options to set on `oracledb`
              maxRows: 100
          }
      }
  });
  module.exports = db;//作为中间件
  ```

* 参照README.md文档会发现数据库结构**要求有标识字段：ID**

  ```
  ​```js
  var person = db.model({table: 'people'});
  var address = db.model({table: 'addresses'});
  
  var bob = person({
    name: 'bob',
    address: address({
      address: 'Fremantle'
    })
  });
  
  bob.save()
  ​```
  
  Produces:
  
      -------- people ----------
      | id | name | address_id |
      --------------------------
      | 11 | bob  | 22         |
      --------------------------
  
      ------- addresses ------
      | id | address         |
      ------------------------
      | 22 | Fremantle       |
      ------------------------
  ```

* **model**

  **## Models**

  \```js

  var createEntity = db.model(options);

  \```

  `createEntity` is a function that can be used to create entities from the model.

  `options` can contain the following:

    \* `table` (`undefined`) the name of the table to save entities to

    **\* `id` (`'id'`) the name of the identity column. This can be an array of id columns for compound keys, or `false` if there is no id column.**

    \* `foreignKeyFor` a function that returns a foreign key field name for a member (see [Relationships](#relationships)), defaults to:

  ​    \```js

  ​    function foreignKeyFor(fieldName) {

  ​      return fieldName + '_id';

  ​    }

  ​    \```

    \* for oracle `idType` (`oracledb.NUMBER`) is the type of the identity column, for e.g. `oracledb.STRING`.

    \* for mssql `generatedId` (`scope_identity`) is the method to get the generated id for insert statements:

  ​    \* `scope_identity` uses `scope_identity()` to get the generated id, this is the default.

  ​    \* `output` uses `output inserted.id` to get the generated id. This will work for `uniqueidentifier` column types but is not compatible with tables that have triggers.

  最难理解的就是上述加粗那段话：笔者经过猜测发现，如果数据库设置有ID字段那么就需要把该字段类型修改为自增。（未测试修改原有ID类型，而是粗暴地把ID字段重命名），否则，一直会提示invalid number。。。因为大多是系统使用ID标识基本都是UUID。。。这点报错还是很坑人的。。。

* 当本人看源码

  ```
  exports.db = function(config) {
    var db = {
      log: config && config.log,
      config: config,
  
      model: function(modelConfig) {
        var proto = _.omit(modelConfig, isModelMeta);
        proto._meta = _.extend({
          id: 'id'
        }, _.pick(modelConfig, isModelMeta));
  
        proto._meta.db = this;
        var id = proto._meta.id;
        proto._meta.compoundKey = id == false || id instanceof Array;
  
        var modelPrototype = _.extend(Object.create(rowBase), proto);
  
        function model(obj, options) {
          var saved = typeof options == 'object' && options.hasOwnProperty('saved')? options.saved: false;
          var modified = typeof options == 'object' && options.hasOwnProperty('modified')? options.modified: false;
          var foreignKeyField = typeof options == 'object' && options.hasOwnProperty('foreignKeyField')? options.foreignKeyField: undefined;
          var row = _.extend(Object.create(modelPrototype), obj);
  
          if (saved) {
            row.setSaved();
            if (!modified) {
              row.setNotChanged();
            }
          }
  
          if (foreignKeyField !== undefined) {
            row.setForeignKeyField(foreignKeyField)
          }
  
          return row;
        }
  
        proto._meta.model = model
  
        model.query = function() {
          var self = this;
          return db.query.apply(db, arguments).then(function (entities) {
            return entities.map(function (e) {
              return self(e, {saved: true});
            });
          });
        };
  
        modelPrototype.queryGraph = function() {
          var self = this;
          return db.query.apply(db, arguments).then(function (entities) {
            return graphify(self, entities)
          });
        };
  
        return model;
      },
  ```

  发现db.model({table:'xxx'})还必须添加id属性....所以，使用这个orm最好是先考虑好数据库结构是否可以修改。如果没有对应的表没有ID字段需要将其设置为[false]

  ```
  var user = db.model({
      table: 'user',
      id: [false]
  });
  ```

* 还有一点就是db.query()只能写查询语句，不能写insert语句。

  ```
  throw new Error('oracle: you cannot pass an SQL statement to db.query(), please use db.statement()')
  ```

  需要使用db.statement()，否则会提示上述异常。。。