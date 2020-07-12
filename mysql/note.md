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

