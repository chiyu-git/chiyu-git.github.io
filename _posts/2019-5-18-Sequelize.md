---
layout: post
#标题配置
title:  Sequelize
#时间配置
date:   2019-5-18 21:00:00 +0800
#大类配置
categories: document
#小类配置
tag: note back-end
---

* content
{:toc}


# Sequelize的使用

+ 中文文档：https://itbilu.com/nodejs/npm/V1PExztfb.html

+ 中文文档2：https://github.com/demopark/sequelize-docs-Zh-CN

## 链接数据库

### 引入Sequelize

+ `const Sequelize = require('sequelize');`

### 实例化数据库

+ `require`引用后，会指向`Sequelize`的主类的构造函数，引用后就可以通过`new`关键字进行实例化

+ 实例化后就会以连接池的形式连接到所使用的数据库。语法结构如下：

  ```js
  const Sequelize = require('sequelize');
  const sequelize = new Sequelize(database, [username=null], [password=null], [options={}])
  // 下面的形式更好理解
  const db = new Sequelize(database, [username=null], [password=null], [options={}])
  ```

+ 实例化`Sequelize`时，需要传入数据库名、用户名和密码，还可以传入一个可选的`options`参数对象。

  > 注意： 所连接的数据库必须存在，若不存在先创建数据库，其中 dbName 请替换成相应的数据库名称
  >
  > 创建数据库命令： CREATE DATABASE **dbName** DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;）

### options参数对象

**[options.host='localhost']**	

+ String	
+ 连接数据库的主机

**[options.dialect='mysql']**	

+ String
+ 要连接的数据库类型。可选值有：mysql、postgres、sqlite、mariadb、mssql

**[options.dialectOptions]**

+ Object

+ ```js
  dialectOptions: {
    // 字符集
    charset: "utf8mb4",
    collate: "utf8mb4_unicode_ci",
    supportBigNumbers: true,
    bigNumberStrings: true
  }
  ```

  ```js
  // 运算符现在默认启用。
  // 你仍然可以通过在 operatorsAliases 中传递一个运算符映射来使用字符串运算符，但这会产生弃用警告。
  operatorsAliases: false,
  ```

**[options.pool={}]**

+ 使用连接池连接，默认为`true`

+ [options.pool.maxConnections]

+ [options.pool.minConnections]

+ [options.pool.maxIdleTime]：连接最大空置时间（毫秒），超时后将释放连接

  ```js
  // 已弃用
  acquire: 30000,
  idle: 10000
  ```

**[options.timezone='+00:00']**

+ String

+ 时间转换时从数据库得到的JavaScript时间。这个时区将应用于连接服务器的 NOW、CURRENT_TIMESTAMP或其它日期函数

  ```js
  timezone: '+08:00', //东八时区
  ```

**[options.define={}]**

+ Object

+ 定义模型的选项，默认为['sequelize.define'选项](https://itbilu.com/nodejs/npm/VkYIaRPz-.html#api-instance-define)

  ```js
  define:{
    timestamps: false // 取消 Sequelzie 自动给数据表加入时间戳（createdAt 以及 updatedAt）
  }
  ```

### 示例配置

+ ```js
  const Sequelize = require('sequelize');
  
  const demo = new Sequelize('demo', 'root', '', {
      host: 'localhost',
      dialect: 'mysql',
      dialectOptions: {
          // 字符集
          charset: "utf8mb4",
          collate: "utf8mb4_unicode_ci",
          supportBigNumbers: true,
          bigNumberStrings: true
      },
      pool: {
          max: 5,
          min: 0,
      },
      timezone: '+08:00', //东八时区
      define:{
        timestamps: false // 取消 Sequelzie 自动给数据表加入时间戳（createdAt 以及 updatedAt）
      }
  });
  
  module.exports = {
      demo
  }
  ```

## 定义Schema对象

+ Schema简单来理解的化就是表的结构

+ 定义模型`model`和表之间的映射关系使用`define`方法。

  > 定义时Sequelize会自动为其添加`createdAt`和`updatedAt`两个属性（属性相当于表中的字段），这样你就可以知道数据什么时候插入了数据库和什么时候进行了更新。

+ `Model`相当于数据库中的表，该对象不能通过构造函数实例化，而只能通过`sequelize.define()`或`sequelize.import()`方法创建。

  > 关于Schema 和 model 的区别 日后再探讨

### sequelize.define()

+ ```js
  var Project = sequelize.define('project', {
    title: Sequelize.STRING,
    description: Sequelize.TEXT
  })
  
  var Task = sequelize.define('task', {
    title: Sequelize.STRING,
    description: Sequelize.TEXT,
    deadline: Sequelize.DATE
  })
  ```

+ 使用`sequelize.define()`方法定义的 Model，已经实例化，直接引入即可调用 Model的方法

### sequelize.import()

+ ```js
  module.exports = function(sequelize, DataTypes) {
    return sequelize.define(
      'user',
      {
        id: { 
          type: DataTypes.INTEGER, 
          autoIncrement: true,
          primaryKey:true,
        },
        username: {
          type: DataTypes.STRING(45),
          allowNull: false
        },
        password: {
          type: DataTypes.STRING(128),
          allowNull: false
        },
      },
      { 
        freezeTableName: true,
        timestamps: false,
      }
    )
  }
  ```

+ 通过`sequelize.import()`导入的模型会被缓存，所以多次导入并不会重复加载

  ```js
  const demo = db.demo // 引入数据库
  const UserModel = demo.import('../schemas/user.js') // 用sequelize实例的import方法引入表结构，实例化了basicTable。
  ```

### 列配置

**defaultValue**

+ 设置属性的默认值

  ```js
  flag: { 
    type: Sequelize.BOOLEAN, 
    defaultValue: true
  },
  ```

**allowNull**

+ ```js
   // 将allowNull设置为false会将NOT NULL添加到列中，
   // 这意味着当列为空时执行查询时将从DB抛出错误。 
   // 如果要在查询DB之前检查值不为空，请查看下面的验证部分。
   title: { type: Sequelize.STRING, allowNull: false},
  ```

**primaryKey**

+ ```
  identifier: { 
  	type: Sequelize.STRING, 
  	primaryKey: true
  },
  ```

**autoIncrement**

+  autoIncrement 选项用于创建一个自增的整型列

  ```js
  incrementMe: { 
    type: Sequelize.INTEGER, 
    autoIncrement: true 
  },
  ```

**references**

+ 通过references选项可以创建外键

  ```js
   bar_id: {
     type: Sequelize.INTEGER,
     references: {
       // 引用另一个模型
       model: Bar,
       // 连接模型的列表
       key: 'id',
     }
   }
  ```

#### Configuration - 配置

+ 定义模型时，可以通过配置来设置列名等相关信息：

  ```js
  var Bar = sequelize.define('bar', { /* bla */ }, {
    // 不要添加时间戳属性 (updatedAt, createdAt)
    timestamps: false,
  
    // 不从数据库中删除数据，而只是增加一个 deletedAt 标识当前时间
    // paranoid 属性只在启用 timestamps 时适用
    paranoid: true,
  
    // 不使用驼峰式命令规则，这样会在使用下划线分隔
    // 这样 updatedAt 的字段名会是 updated_at
    underscored: true,
  
    // 禁止修改表名. 默认情况下
    // sequelize会自动使用传入的模型名（define的第一个参数）做为表名
    // 如果你不想使用这种方式你需要进行以下设置
    freezeTableName: true,
  
    // 定义表名
    tableName: 'my_very_custom_table_name'
  })
  ```

### dataTypes

+ STRING
+ INTEGER
+ FLOAT
+ DATE
+ 怎么储存嵌套的对象或者数组？                   

## 创建、使用Model对象

+ `sequelize.define()`方式创建的Model，直接引入即可使用

+ `sequelize.import()`方法创建的Model

  ```js
  const demo = db.demo // 引入数据库
  const UserModel = demo.import('../schemas/user.js') 
  ```

## 使用Controller

+ ```js
  const getUserInfo = async function (ctx, next){
    const id = ctx.params.id// 获取url里传过来的参数里的id
    const result = await UserModel.getUserById(id);  // 通过await“同步”地返回查询结果
    ctx.response.body = result // 将请求的结果放到response的body里返回
  }
  ```

+ 其实只是MVC模式的体现，不属于Sequelize定义的部分

# 增删改查（Model方法）

- 有了Model对象，就可以进行增删改查的操作了
- 增删改查的对象都是 **Model 的实例**

## 增

### Model.create()

- 构建一个新的模型实例，并进行保存。与`build()`方法不同的是，此方法除创建新实例外，还会将其保存到对应数据库表中。

  ```js
  create(values, [options]) -> Promise.<Instance>
  ```

  ```js
  /**
   * @param {object} userInfo
   * @return {object}
   */
  const createUser = async function(userInfo){
    let res = await UserModel.create(userInfo)
    return res 
  }
  ```

## 查

### Model.findById() 

- 通过Id查询查询单个实例（单条数据）。

  ```js
  findById(id, [options]) -> Promise.<Instance>
  ```

  ```js
  /**
   * @param {number} id
   * @return {object}
   */
  const getUserById = async function(id) {
    const userInfo = await UserModel.findById(id)
    return userInfo // 返回数据
  }
  ```

- **别名：findByPrimary**

### Model.findOne() 

- 查询单个实例（单条数据）。这将会使用`LIMIT 1`查询条件，所以回调中总是返回单个实例。

  ```js
  findOne(options]) -> Promise.<Instance>
  ```

  ```js
  /**
   * @param {string} username
   * @return {object}
   */
  const getUserByName = async function(username) {
    const userInfo = await User.findOne({
      // 用await控制异步操作，将返回的Promise对象里的数据返回出来。也就实现了“同步”的写法获取异步IO操作的数据
      where: {
        username:username
      }
    })
    return userInfo // 返回数据
  }
  ```

### Model.findAll()

- 查询多个实例（多条数据）

  ```js
  findAll([options]) -> Promise.<Array.<Instance>>
  ```

- **别名：all**

## 改

### Model.update() 

- 更新所匹配的多个实例。promise回调中会返回一个包含一个或两个元素的数组，第一个元素始终表示受影响的行数，第二个元素表示实际影响的行（仅Postgre`options.returning`为true时受支持）

  ```js
  update(values, options) -> Promise.<Array.<affectedCount, affectedRows>>
  ```

  ```js
  /**
   * @param {number} id
   * @param {obeject} newInfo
   * @return {object}
   */
  const updateUserById = async function(id,newInfo){
    const userInfo = await UserModel.update(newInfo,{
      where:{
        id:id
      }
    })
    return userInfo
  }
  ```

## 删

### Model.destroy()

- 删除多个实例，或设置deletedAt的时间戳为当前时间（当启用`paranoid`时）

  ```js
  destroy(options) -> Promise.<Integer>
  ```

- 执行成功后返回被删除的行数

  ```js
  /**
   * @param {number} id
   * @return {object}
   */
  const destroyUserById = async function(id){
    const userInfo = await UserModel.destroy({
      where:{
        id:id
      }
    })
    return userInfo
  }
  ```


## 组合

### Model.findOrCreate() 

- 查找一行记录，如果不存在则创建实例并保存到数据库中

- 在这个方法中，如果`options`对象中没有传入事务，那么会在内部自动创建一个新的事务，以防止在创建完成之前有新匹配查询进入。

  ```js
  findOrCreate(options) -> Promise.<Instance, created>
  ```

# 查询与原始查询

https://itbilu.com/nodejs/npm/VJIR1CjMb.html

### 使用原始查询

- 如，在查询中使用`AND`和`=`：

  ```
  Model.findAll({
    where: {
      attr1: 42,
      attr2: 'cake'
    }
  })
  // WHERE attr1 = 42 AND attr2 = 'cake'
  ```

- 在查询中使用`大于`、`小于`等：

  ```
  Model.findAll({
    where: {
      attr1: {
        $gt: 50
      },
      attr2: {
        $lte: 45
      },
      attr3: {
        $in: [1,2,3]
      },
      attr4: {
        $ne: 5
      }
    }
  })
  // WHERE attr1 > 50 AND attr2 <= 45 AND attr3 IN (1,2,3) AND attr4 != 5
  ```

- 在查询中使用`OR`：

  ```
  Model.findAll({
    where: {
      name: 'a project',
      $or: [
        {id: [1, 2, 3]},
        {
          $and: [
            {id: {gt: 10}},
            {id: {lt: 100}}
          ]
        }
      ]
    }
  });
  
  //WHERE `Model`.`name` = 'a project' AND (`Model`.`id` IN (1, 2, 3) OR (`Model`.`id` > 10 AND `Model`.`id` < 100));
  ```

- 查询成功后会返回包含多个实例（`instance`）的数组。