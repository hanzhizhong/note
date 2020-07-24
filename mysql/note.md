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
new Sequelize('数据库名','用户名','密码',{host:'',dialects:"sqlite",storage:'path/to/数据库名.sqlite'})


~~~

#### 数据检索/查找器

~~~css
find 
Project.findByPK()
Project.findOne({})
Project.findOrCreate({where:{},defaults:{}}) 返回的是数组[{},bool]
	如果是新建的bool为true,否则为false
	已经存在的数据不会被修改

~~~



#### 配置环境变量

~~~css
set NODE_ENV=home;
~~~

