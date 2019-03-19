---
layout: post
#标题配置
title:  Mongoose Note
#时间配置
date:   2019-3-17 21:00:00 +0800
#大类配置
categories: document
#小类配置
tag: note back-end
---

* content
{:toc}



# 1. 简介

- 之前我们都是通过shell来完成对数据库的各种操作的，在开发中大部分时候我们都需要通过程序来完成对数据库的操作。

- 而Mongoose就是一个让我们可以通过Node来操作MongoDB的模块。

- MongoDB本身就是Node的一个模块，Mongoose让我们操作MongoDB模块更加简单

- Mongoose是一个对象文档模型（ODM:js对象、数据库文档，以对象的形式操作数据库的文档）库，它对Node原生的MongoDB模块进行了进一步的优化封装，并提供了更多的功能。

- 在大多数情况下，它被用来把结构化的模式应用到一个MongoDB集合，并提供了验证和类型转换等好处

### mongoose的好处

- 可以为文档创建一个模式结构（Schema）

- 可以对字段进行检验，数目，数据类型

- 可以对模型中的对象/文档进行验证

- 数据可以通过类型转换转换为对象模型

- 可以使用中间件来应用业务逻辑挂钩

- 比Node原生的MongoDB驱动更容易

# 2. 基本概念

## 新的对象

### Schema(模式对象)

- Schema对象定义约束了数据库中的文档结构

### Model

- Model对象作为集合中的所有文档的表示，相当于MongoDB数据库中的**集合collection**

### Document

- Document表示集合中的具体文档，相当于集合中的一个具体的文档

- 三者存在先后关系

# 3. Mongoose的使用

## 链接数据库

1. 安装Mongoose
   - npm  i mongoose –save

2. 引入Mongoose
   - var mongoose = require(“mongoose”);

3. 链接MongoDB数据库

   - `mongoose.connect('mongodb://localhost/test'.{useMongoClient:true});`

   - mongodb://地址:端口号/数据库名

   - 如果端口号是默认端口号27017则可以省略

- 断开连接
  - `mongoose.disconnect()`
  - 一般不需要这个方法，一般只会链接一次，除非项目停止服务器关闭，否则链接不会断开

4. 监听MongoDB的链接状态

   - 在mongoose对象中，有一个属性叫做connection，该对象表示的是数据库的链接，通过监视该对象的状态可以监听数据库的断开与链接

   - `mongoose.connection.once(“open”,function(){});`

   - `mongoose.connection.once(“close”,function(){});`

## 创建Shcema对象

- 大写的S，因为是一个构造函数

- `var mongoose = require("mongoose")`
- `var Schema = mongoose.Schema;` 
  - 赋值变量，方便以后使用

```js
var stuSchema = new Schema({
  name:{type:String,required: true}
  age:Number,
  gender:{
    type:String,
    default:"female"
  },
  address:String
})
```

#### 数据类型，必填，默认值

## 创建Model对象

- mongoose.model(ModelName, schema);
  - modelName:要映射的集合名字 
  - mongoose会自动将单数集合名变成复数

- `var StuModel = mongoose.model('Student', stuSchema); `
  - 大写，因为一个集合，一个构造函数，find()相当于找这个构造函数的所有实例，数据库角度，找集合中的所有文档

## 创建Doucument对象

-       ModelName.create({doc,function(err){});
-       ![1547180761337](F:\OneDrive\JS\纲略合集\assets\1547180761337.png) 
-       此时数据库、集合才被创建，和原生MongoDB一样
-       如果不传gender值，则默认是female

# 4. 增删改查（Model方法）

-       有了Model对象，就可以进行增删改查的操作了

## 增

### Model.create(doc(s),[callback])

- 用来创建一个或多个文档并添加到数据库中 //db.collection.insert()

- 参数：

1. doc(s)，可以是一个文档对象，也可以是一个文档对象的数组或多个文档对象

2. callback，可选的，当操作完成之后执行，返回err和插入的文档

- ![1547180894405](F:\OneDrive\JS\纲略合集\assets\1547180894405.png) 

## 查

- 通过find()方法返回的对象，就是Document文档对象 //即是数据库的数据

- Document对象是Model的实例 //instance of
  - ![img](F:\OneDrive\JS\纲略合集\assets\clip_image002-1547180953267.jpg)，像这种带#的方法是实例的方法

  - find()相当于找这个构造函数的所有实例，数据库角度，找集合中的所有文档

### Model.find(conditions, [projection\], [options], [callback])

- 文档的_id属性，是一个对象，但是以字符串形式也是可以find到的，并不影响

- 查询所有符合条件的文档对象**数组**

- conditions:查询的条件，传一个空对象则查询所有

- projection:投影

- options:查询选项（skip limit）

- callback：回调函数，查询结果会通过回调函数返回，以数组形式，数组里面是一个个文档对象

- `StuModel.find({name:”唐僧”},function(err,docs){!err?docs.log→:;})`
  - ![1547186756263](F:\OneDrive\JS\纲略合集\assets\1547186756263.png) 
- `StuModel.find({},{name:1,_id:0},function(err,docs){!err?docs.log→:;})`
  - ![1547186792160](F:\OneDrive\JS\纲略合集\assets\1547186792160.png) 设置投影，与MongoDB一致
- `StuModel.find({},”name age -_id”,function(err,docs){!err?docs.log→:;})`
  - ![1547186824606](F:\OneDrive\JS\纲略合集\assets\1547186824606.png)字符串形式设置投影，写什么有什么，空格连接，取消默认的_id用负号

- **投影用于过滤属性**

- `StuModel.find({},”name age -_id”,{skip:3},function(err,docs){!err?docs.log→:;})`
  - ![1547186871650](F:\OneDrive\JS\纲略合集\assets\1547186871650.png) 跳过前三个

### Model.findOne(conditions, [projection], [options], [callback]）

- 查询符合条件的一个文档**对象**

- callback:err,doc

### Model.findById(id, [projection], [options], [callback]）

- 根据文档的ID查询文档对象

- id以字符串形式传入
  - ![1547186987610](F:\OneDrive\JS\纲略合集\assets\1547186987610.png) 
  - ![1547186992043](F:\OneDrive\JS\纲略合集\assets\1547186992043.png) 
  - 封装之后直接传（）里的字符串即可

## 改

### Model.update(conditions,doc, [options], [callback]）

### Model.updateOne(conditions,doc, [options], [callback]）

### Model.updateMany(conditions,doc, [options], [callback]）

- 用于修改一个或多个文档对象

1. conditions:查询条件

2. doc：传入的是需要修改的属性

   - 有这个属性就修改value，没有这个属性就添加这个属性，**原本对象不会被改写**

   - All top level update keys which are not atomic operation names are treated as set operations

   - 所有不是指定$级别的操作，都会被视为$set，这是为了防止重写文档对象，所以实验的结果是成立的

   - 传入的文档已经是对象形式了，不要多余的加一个括号，会被认为是参数设置

   - `PostModel.update({_id:postInfo._id},postInfo,{new:true},function(err,post){}`

3. options:配置参，

   - multi:true ,update查多个

   - new:true,回调函数的参数是修改后的对象，想要在回调函数内部访问修改后的对象，一个是设置该选项，或者用assign合并，因为常常要对oldDoc进行判断

4. callback:回调函数

- `StuModel.updateOne({name:”唐僧”},{$set{age:20}},function(err){!err?“修改成功”.log→:;})`

- update参数传入$set，可以修改指定属性，防止把整个对象重写{}，新版本做了相应的修改，现在不传入$set也可以

### Model.replaceOne(conditions,doc, [options], [callback]）

- 会覆盖

## 删

### Model.remove(conditions, [callback]）

- 已被废黜

### Model.deleteOne(conditions, [callback]）

### Model.deleteMany(conditions, [callback]）.

- `StuModel.remove({name:”白骨精”},function(err){!err?”ok”.log→:;})`

- delete和remove的不统一命名还是存在和原生一样

- { ok: 1, n: 2 }
- ok 为1 操作成功，n代表匹配到的文档的数量
- 返回的是被删除的文档

## 其他方法

### Model.count(conditions, [callback]）

- 统计文档的数量

- 如果调用find({}).length，性能较差，先获取数组，在输出结果，而count直接获取结果

- `StuModel.count({},function(err,count{!err?count.log→:;});`

# 5. Ducument方法

- Document对象和数据库里的文档一一对应，Document对象是Model的实例，通过Model查询到的结果都是Document

- `var stu = new StuModel({name:,age,address});`

- Model是构造函数，用构造函数直接创建对象，而Model.create()，创建之后还会自动添加到Model

### [Model#save([options\] [options.safe], [options.validateBeforeSave],[fn])

- `stu.save(function(err){!err?”ok”.log→:;});`
  - 自动保存到StuModel的集合中

### update(update,[options],[callback])

- Model.update()要先传条件conditions

- `doc.update({$set:{age:28}},function(err){ })`

- 或者直接：doc.age = 28 ; doc.save();

- 因为就是一个对象，就是对象的属性

### remove([callback])

- 删除自己

- `doc.remove(function(err){});`

### get(path,[type])

- 获取文档中的指定属性值

- doc.get(“name”);

- doc.name;

### set(path,value,[type])

- doc.set(“name”,”猪小小”);

- doc.name = “猪小小”;

- doc._id/doc.id

### equals(doc)

- 是否是同一个文档

### isNew

- 是否是新对象，有没有存进数据库，存了false，没存new

### toJSON()

### toObject()

- 将Document对象转换为普通JS对象

- 转换为普通JS对象之后，所有的Document对象的方法或属性不能再使用

- 作用：数据库保留隐私数据，但是前端显示的时候临时把隐私数据删除![img](F:\OneDrive\JS\纲略合集\assets\clip_image002-1547187742778.jpg)，如果是Document对象无法用delete操作符删除属性，如果用$unset则会删除数据库的数据，后患无穷

- doc.id //undefined doc._id //因为是普通JS对象了，不会自动转化

- ![1547187917067](F:\OneDrive\JS\纲略合集\assets\1547187917067.png) 

# 6. Mongoose模块化

- 引入Mongoose，连接数据库，设定Schema在开发中中需要反复使用，每次不同的js文件都要重新引入，我们实际再操作的主要是Model和Document，所以把这些部分模块化

- 定义一个js文件专门用于连接数据库

### 连接数据库的模块

```js
/*连接数据库*/
// 引入mongoose
const mongoose = require('mongoose')
// 连接指定数据库(URL只有数据库是变化的)
mongoose.connect('mongodb://localhost:27017/calender',{ useNewUrlParser: true })
// 获取连接对象
const conn = mongoose.connection
// 绑定连接完成的监听(用来提示连接成功)
conn.once('connected', () => {
  console.log('db connect success!')
```

### 定义Schema的模块

```js
/* 定义出对应特定集合的Model并向外暴露*/
// 引入mongoose
const mongoose = require('mongoose')

const Schema = mongoose.Schema
// 定义userSchema(描述文档结构)
const userSchema = new Schema({
  events:{type:Array} //储存着用户创建的events的_id
})
// 定义Model(与集合对应, 可以操作集合)
const UserModel = mongoose.model('user', userSchema) // 集合为: users
// 向外暴露Model
exports.UserModel = UserModel
```

- ![1547188225341](F:\OneDrive\JS\纲略合集\assets\1547188225341.png) 
- ![1547188251888](F:\OneDrive\JS\纲略合集\assets\1547188251888.png) 此时返回的，就是StuModel模型对象
- 



### 