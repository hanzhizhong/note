### 字母含义

~~~css
t_xxx表示表名
u_xxx开头表示 视图
i_xxx开头表示 索引
~~~



### 提交的数据验证

~~~css
post，put:提交的数据需要验证 koa-parameter 
~~~

### 查询操作

~~~css
select * from [tablename]

~~~

### 更新操作

~~~css
update [tablename] set 字段=val1,字段=val2...where 
~~~

### 删除

~~~css
delete from [tablename] where ...
~~~

### 增加

~~~css
insert into [tablename] values();
insert into [tablename] (字段1，字段2...)  values(),()
~~~

### 查询数据库的连接数

~~~css
mysqladmin -uroot -p[你的密码] processlist
~~~

### mysql 的单例连接和连接池的区别

~~~css
单例只维护一个链接，
连接池维护一些链接。

单例只维护一个链接一个链接就不能在你正在使用这个链接的时候同时请求别的数据啦呀。

我的理解，假设执行一个SQL还没结束，你再用connection调用一个SQL查询
这个时候connection会等待的（或者说数据库会保证他们不能并发的）


往后先使用单例模式，有提出要求的就改成连接池
~~~

#### 连接池

~~~css
const config={
    host:'localhost',
    user:'root',
    port:3306,
    password:'123456',
    database:'test'
}


封装01
class Pool{
    constructor(){
        this.pool=mysql.createPool(config)
    }
    connect(sql,arr,cb){
        this.pool.getConnection((err,connection)=>{
            if(err){
                console.log('连接错误',err)
                return 
            }
            connection.query(sql,arr,(error,result)=>{
                if(error){
                    console.log('获取数据失败',error)
                    return 
                }
                cb(result)
            })
            this.pool.releaseConnection(connection)//释放
        })
    }
}
let pool=new Pool()
module.exports= pool

调用 
const pool=require('./pool.js')
pool.connect(sql,arr,function(ret){
    console.log(ret)
})

方法02 promise
class Pool{
    constructor(){
        this.pool={}
        this.create()
    }
    create(){
        this.pool=mysql.createPool(config)
    }
    async connect(sql,arr){
        let connection=await new Promise((resolve,reject)=>{
            this.pool.getConnection((err,connection)=>{
                if(err){
                    reject(err)
                    return 
                }
                resolve(connection)
            })
        })
        return new Promise((resolve,reject)=>{
            connection.query(sql,arr,(err,result)=>{
                if(err){
                    reject(err)
                    return 
                }
                resolve(result)
            })
            this.pool.releaseConnection(connection)
        })
    }
}
let pool=new Pool
module.exports=pool

~~~

### Sequelize 

~~~Css
ORM 
	对象关系映射模型
	通过对象来映射和操作数据库
模型
	用来表示（描述）数据库字段信息的对象，每个模型对象表示数据库中的一个表，后续对数据库的操作都是通过对对应的模型对象来完成的

~~~

#### 要点

~~~css
1.数据库名一定是已经创建好的，不然会报错 

2.钩子(Hooks)
生命周期事件 执行sequelize中，调用之前和之后调用的函数。
不能讲钩子函数与实例一起使用，而是与模型一起使用 

本地钩子总是在全局钩子之前运行
User.beforeCreate(user=>{
    //user就是需要录入数据的数据，可以根据它判断权限的问题
})

仅查询部分属性（字段）时，可以通过 attributes选项来指定。
	attributes:['foo','id']
	属性可以通过数组嵌套的方式来重命名
	attributes:['foo',['bar','baz']] 将bar重命名为baz，返回的数据字段显示的名称为baz
	[[sequelize.fn('COUNT',sequelize.col('hats')),'no_hats']]
select count(hats) as no_hats ...

const Op=Sequelize.Op

[Op.ne]:null	!=null
[Op.eq]:3		=3
[Op.is]:null	is null
[Op.not]:true	is not true
[Op.between]:[6,10]		between 6 and 10
[Op.in]:[1,3]
[Op.notIn]:[1,2]
[Op.like]:'%hat'
[Op.notLike]:'%hat'
[Op.startsWith]:'hat'	'hat%'
[Op.endsWith]:'hat'		'%hat'
[Op.substring]:'hat'	'%hat%'


let temp=Model.build({....}) 返回未保存的对象
如果需要保存到数据库中使用
temp.save().then().catch(err)

Model.update({....},{fields:['title']}).then()
只更新fields 数组中的字段数据


User.create({...}).then(user=>{
    return user.destroy()//强制删除 user.destroy({force:true}) paranoid=true时
恢复软删除的实例 
}).then(user=>{
    return user.restore()
})

sequelize中四种可用的关联类型
BelongsTo BelongsTo BelongsTo 
HasMany HasMany HasMany 
HasOne HasOne HasOne 
HasOne 
BelongsToMany 
BelongsToMany 
BelongsTo 
HasOne 
HasMany 
BelongsToMany 

这四种关联类型的“使用方法”和你需要查询的表(源键)有关

目标键（target key）
目标模型上的列，是源模型外键列所指向的列
HasOne是目标模型
源键（Source key）
是目标模型上的外键属性，指向源模型上的属性。

在sequelize中关联两个模型是，可以将他们称为 源和目标模型对

Class Player extends Model{}
Player.init({...},{sequelize,modelName:'player'})
class Team extends Model{}
Team.init({...},{sequelize,modelName:'team'})

//player作为源，team作为目标
Player.BelongsTo(Team) 
或者
Player.HasOne(Team) 

hasOne会在目标模型中插入外键
belongsTo会在源模型上插入外键

当源模型上有关联的信息时，就用belongsTo
当目标原型上有关联信息时，就用hanOne 

~~~



#### 操作

~~~css
const sequelize=new Sequelize('数据库名','用户名'，‘密码’，{其他配置
,
    dialect:数据库类型，必填项，‘mysql’
    host:'',
    port:'',
    timezone:'+08:00'
});
sequelize.authenticate() //连接测试

sequelize依赖了mysql2 库，需要自己手动安装mysql2

数据库连接完成后，需要确定操作的表
使用orm，不需要通过sql来操作表，而是通过对象来操作
给每个操作的表定义一个对象，  模型model

const userModel=sequelize.define("User",{
    //描述表中对应的字段信息
    //对象的key默认对应着表的column，字段
    key:{
        type,
        allowNull,
        defaultValue,
        unique,
        primaryKey,
        field,数据库中字段的实际名称
        autoIncrement
    }
},{
    //描述表的其他信息
    get,
    set,
    validate,
    timestamps,是否为每条数据添加createdAt和updatedAt字段，并在添加新数据和更新数据的时候自动设置这两个字段的值，默认为true
    paranoid:设置deletedAt字段，当删除一条记录的时候，并不是真的销毁记录，而是通过该字段来表示，即保留数据，进行假删除，默认为false
    freezeTableName:禁用修改表名，默认情况下，sequelize将自动将所有传递的模型名称(defined的第一个参数)转成复数，User=>Users，默认为false
        indexes:[//添加索引
        	{
                name:'uname',
                field:['username']//对用数据库的column名称
            }
        ]
})

模型类=》表
模型创建出来的对象=》表中某条记录
let Kimoo=new UesrModel()//创建了一个user的记录 一行数据
或者使用一个静态方法
let kimoo=UserModel.build({
    username:'',
    age:30,
    gender：‘男’
})
通过new或者build出来的对象不会立即同步添加到数据库中，需要后续的方法来同步
await kimoo.save()

kimoo.update({
    
})==kimoo.set()+kimoo.save()
kimoo.get()
删除
kimoo.destroy()


~~~

#### 数据库迁移

~~~css 
sequelize-cli 
npm install -D sequelize-cli
npm install -S sequelize 
npm install -S mysql2 

使用sequelize-cli 的迁移操作
	./node_modules/.bin/sequelize
	列出所有的操作命令

node_modules/.bin/sequelize init 
自动创建几个目录
	config/config.json 与数据有关的配置信息
	models 包含项目的所有模型 给程序用的
	migrations 包含所有的迁移文件  变更结构
	seeders 包含所有的种子文件  测试数据

migrations 结构：
	up:
	down

执行迁移文件:
	db:migrate
~~~

#### 各个数据库的不同配置

~~~css
dialects:方言 配置数据库类型

1.mysql
new Sequlize('数据库名'，'用户名','密码',{host:'',dialects:'mysql'})
2.sqlite
new Sequelize({dialects:"sqlite",storage:'path/to/数据库名.sqlite'})


~~~

#### 数据检索/查找器

~~~css
find 
Project.findByPk() //k是小写
Project.findOne({})
Project.findOrCreate({where:{},defaults:{}}) 返回的是数组[{},bool]
	如果是新建的bool为true,否则为false
	已经存在的数据不会被修改

Project.create()//创建

Project.update()
Project.destroy()
Project.findAll()
~~~

#### scopes的作用

~~~css
new Sequelize('',{},{
    scopes:{
        where:{}
    }
})
相当于预先定义好的每条sql语句都会携带的scopes字段的where查询语句
~~~



#### sequelize-cli的命令

~~~css
通过迁移功能，你可以将现有数据库转移到另一个状态，反之亦然。进行迁移时，状态转换会被保存到迁移文件中，这些文件描述了如何进入新状态以及如何恢复更改以恢复到旧状态。
~~~

~~~css
npx sequelize-cli init 生成下面的文件夹
config：包含配置文件，告诉cli怎么连接数据库 
models：包含项目中所有的模型  ***这是给程序员用的 可以在这里修改表结构
migrations:包含所有的迁移文件
seeders:包含所有的种子文件

migrations和seeders是给sequelize-cli用的

models index.js导出的文件格式为：{
    model:model//模块的名称 在 tableName modelName 注意大小写
        sequelize:{}//数据库的基本信息
    Sequelize:{}//实例
}
~~~

~~~css
npx sequelize-cli model:generate 创建一个模型

npx sequelize-cli model:generate --name User --attributes firstname:string,email:string
~~~

~~~css
执行迁移 链接数据库并创建表
npx sequelize-cli db:migrate

撤销迁移
npx sequelize-cli db:migrate:undo


撤销所有的迁移
npx sequelize-cli db:migrate:undo:all

恢复到指定文件的迁移
npx sequelize-cli db:migrate:undo:all --to xxxxxxx-create-posts.js
~~~

~~~css
seeders文件表示数据的一些变化，可用于使用样本数据或测试数据填充数据库

npx sequelize-cli seed:generate --name demo-user 创建一个种子文件，然后编辑这个文件
执行种子
npx sequelize-cli db:seed:all
提交到数据库中

撤销种子
npx sequelize-cli db:seed:undo 

撤销到指定的种子
npx sequelize-cli db:seed:undo --seed name-of-seed-as-in-data 

撤销所有的种子
npx sequelize-cli db:seed:undo:all 

npx sequelize-cli db:migrate:status =》up/down两种状态

sequelizeMeta中记录和保存的是迁移的文件 XXX.js

生成迁移文件的结构
npx sequelize-cli migration:generate --name xxxxx...

~~~

#### 聚合函数

~~~css
User.count('字段名').then()
User.max('').then()
User.min('').then()
~~~



#### 配置环境变量

~~~css
window环境下设置环境变量的方式
set NODE_ENV=home;//当前的cmd窗口有效（局部有效）
查看 环境变量
set 回车
echo %NODE_ENV% 回车

npx sequelize-cli model:generate --name Userinfo --attributes sex:enum,age:integer,qg:string,job:string,birthday:date,path:string
~~~

#### undescored:false

~~~css
1.蛇形法 蛇形法是全由小写字母和下划线组成,在两个单词之间用下滑线连接即可, 例如:first_name、last_name。
2.驼峰法 骆驼式命名法就是当变量名或函式名是由一个或多个单词连结在一起,而构成的唯一识别字时, 第一个单词以小写字母开始;第二个单词的首字母大写或每一个单词的首字母都采用大写字母,

// 自动设置字段为蛇型命名规则
  // 不会覆盖已定义的字段选项属性
  underscored: true,
~~~

#### 返回的数据样式修剪

~~~js
//多对多的关系时，显示的数据连城一块饼的问题是 ：raw:true 的使用了，去掉

//多对多 怎样去掉关系表的字段数据：through:{attributes:[]} 
~~~



#### .sequelizerc配置文件

~~~css
在npx sequelize-cli init 时自动触发
.sequelizerc的作用是配置 {config,models,migrations,seeders}的路径或者文件名
{
    'config':resolve(__dirname,'config/mysql.json'),
   "models-path":
    "seeders-path":
    "migrations-path":
}
~~~

### 数据库的索引

~~~js
select * from users where username="han"
这需要全表扫描
如果使用索引将会提升性能：索引就是缩小一张表中需要查询的记录/行 的数目来加快搜索的速度
索引是一种数据结构
~~~



### RBAC 

~~~css
RBAC:基于角色的访问控制 (roles-based access control) who what how 三元组之间的关系
who 权限的拥有者或主体
what 操作或对象
how 具体的权限 （正向授权和负向授权）
who对what进行how的操作

~~~

#### 鉴权管理 即权限判断逻辑

~~~css
1.最基本的权限管理就是 菜单管理
2.功能权限管理。B/S系统的功能体现为URL访问的管理
	如：访问【添加用户】的功能(url)时，有没有授权的提示
3.行级权限管理：A能管理论坛【新闻模块】,不能管理论坛【技术交流】模块箱
4.列级权限：除销售人员外，其他用户不能看到客户的联系方式
5.组织机构/部门级数据权限管理：销售一部不能看到销售二部的订单信息，但是销售经理可以同时看到
6.范围型业务数据权限管理：大卖场销售人员在下订单时，要选择相应的产品所在仓库信息，不能看到【广州仓库】
~~~

#### 授权管理 即权限分配的过程

~~~css
鉴权管理的所有内容都要通过系统的授权功能来分配具体的用户
1.直接对用户授权
2.对用户所属岗位授权
3.用户所有权限授权
4.角色直接关联具体的功能权限(URL),也可以关联负权限，即此角色关联的权限不能使使用负权限功能
5.分级授权，系统管理员可以将自己的权限信息授权给其他用户。
~~~

#### 权限的类型

~~~css
数据权限：账号能查看多少数据
功能权限：大致菜单权限和按钮权限
~~~

#### RBAC的种类

~~~css
RBAC0 是rbac的核心思想，权限的最低需求
RBAC1 基于rbac0，但是增加了角色分级的概念，一个角色可以从另一个角色继承许可权
RBAC2 基于rbac0,增加了一些限制，强调在rbac的不同组件中在配置方面的一些限制
RBAC3 统一模型，包含了 rbac1和rbac2，
~~~



