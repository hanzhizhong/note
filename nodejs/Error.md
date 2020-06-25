### formidable

> [参考网址][https://www.cnblogs.com/abab301/p/9489000.html]

~~~javascript
//使用方法
npm install formidable

const formidable=require('formidable')

//使用方法
let form=new formidable.IncomingForm()
//上传文件的限制和路径设置
form.uploadDir='upload' //切记切记 不能使用绝对或者相对路径，不然会报错
form.keepExtensions=true//使用上传文件的拓展名
form.maxFieldsSize=2*1024*1024 //fields最大2M
form.parse(req,(err,fields,files)=>{
    //fields为所有的表单中字段内容
    //files为上传的文件格式内容
})

特点：大而全 不需要body-parser等中间件了
~~~

### multer 文件上传的又一个插件

~~~css
只针对 type="multipart/form-data"
sigle
array
fields
var storage=multer.diskStorage({
    destination(req,file,cb){
        cb(null,'/tmp/my-uploads')
    },
    filename(req,file,cb){
        cb(null,file.fieldname+'-'+Date.now())
    }
})
const upload=multer({storage:storage})
上面的代码可以统一控制上传的文件路径和文件名

limits:{
    fileSize:'5MB' 是字符串，是上传文件大小加起来的大小进行限制
}
~~~



### express 中的路由报错

> ![image-20191204100916427](assets/image-20191204100916427.png)
>
> + 解决的方法是： 
>
>   + ~~~javascript
>        const express=require('express')
>        const router=express.Router(); //注意是Router()
>        ~~~
>  
> 

### art-template 

> **js的模板引擎** 在模板中使用方法的传参方式
>
> + ![image-20191204144210536](assets/image-20191204144210536.png)
>
> + 解决方法
>
>   ![image-20191204144448506](assets/image-20191204144448506.png)

### sqlite3 

#### 新建并打开数据库

+ new sqlite3.database(filename,[mode],[callback]) 返回一个自动打开的数据库对象

  + ~~~javascript
    filename :有效的文件名，“test.db” 如果是:":memory",表示是内存数据库，数据不会持久化保存
    mode: 数据库的模式3种， sqlite3.OPEN_READONLY, sqlite3.OPEN_READWRITE, sqlite3.OPEN_CREATE
    callback :成功或者错误时调用，第一个参数是错误，或者空对象
    ~~~

#### 关闭数据库

+ database.close([callback]) 关闭一个数据库的链接对象

  + ~~~javascript
    callback 关闭成功的回调。第一个参数是一个错误，为null表示成功关闭
    ~~~

#### 执行DDL和DML  

>  数据查询语言DQL，数据操纵语言DML，数据定义语言DDL，数据控制语言DCL。 
>
>  + DQL
>
>  ~~~css
>  select
>  from
>  where
>  ~~~
>
>  + DML
>
>  ~~~css 
>  insert
>  update
>  delete
>  ~~~
>
>  + DDL
>
>  ~~~css 
>  创建数据库中的各种对象---表.视图
>  create table/view/index/syn/cluster 
>  		表  视图 索引 同义词 簇
>  ~~~
>
>  + database.run(sql,[param,...], [callback])
>
>  ~~~javascript
>  sql:要运行的sql字符串。sql类型是 DDL和DML, DQL不能使用这个命令。执行后返回值不包含任何结果，必须通过回调函数获取执行结果
>  ~~~
>
>  param,...: 当sql语句中包含（?）时，这里可以传入对应的参数
>
>  ~~~javascript
>  // 直接通过参数传值.
>  db.run("UPDATE tbl SET name = ? WHERE id = ?", "bar", 2);
>   
>  // 将值封装为一个数组传值.
>  db.run("UPDATE tbl SET name = ? WHERE id = ?", [ "bar", 2 ]);
>  
>  // 使用一个json传值.参数的前缀可以是“:name”，“@name”和“$name”。推荐用“$name”形式
>  db.run("UPDATE tbl SET name = $name WHERE id = $id", {
>  $id: 2,
>  $name: "bar"
>  });
>  ~~~
>
>  callback: 如果执行成功，则第一个参数为null，否则就是出错。
>
>  ~~~javascript
>  callback（可选）：
>  
>  如果执行成功，上下文this包含两个属性：lastID和changes。lastID表示在执行INSERT命令语句时，最后一条数据的id；changes表示UPADTE命令和DELETE命令时候，影响的数据行数。
>  ~~~
>
>  Database
>
>  + 用法：new sqlite3.Database(filename,[mode],[callback])
>  + 功能：返回数据库对象并且自动打开和链接数据库，他没有独立打开数据库的方法
>
>  close
>
>  + 用法：close([callback])
>  + 功能：关闭和释放数据库对象
>
>  run 
>
>  + 用法：run(sql,[param,...],[callback])
>  + 功能：运行指定的sql语句，完成之后调用回调函数
>
>  get
>
>  + 用法：get(sql,[param,...],[callback])
>  + 功能：运行指定的sql语句，完成后调用回调函数，回调函数有两个参数，执行成功第一个为null,第二个参数为执行的结果
>
>  all 
>
>  + 用法： all(sql,[param,...],[callback])
>  + 功能：运行指定sql语句，完成后调用回调函数。成功第一个参数为null，第二个参数为查询的结果集
>
>  prepare
>
>  + 用法：prepare(sql,[param,...],[callback])

### sqlite3.close() 

+  警告:不回调而调用异步函数是不赞成的 

![image-20191209111646079](assets/image-20191209111646079.png)

### module.exports 

+ 导出class DB{} 类/构造函数时出错 **是自己将导入的文件名写错了**

+ ![image-20191209141236460](assets/image-20191209141236460.png)


### nodejs支持的字符编码

+ 'ascii' - 仅支持 7 位 ASCII 数据。

  'utf8' - 多字节编码的 Unicode 字符。

  'utf16le' - 2 或 4 个字节，小端序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）。

  'ucs2' - 'utf16le' 的别名。

  'base64' - Base64 编码。

  'latin1' - 将 Buffer 编码成单字节编码的字符串。

  'binary' - 'latin1' 的别名。

  'hex' - 将每个字节编码成两个十六进制字符。


### 代码块的注释方法 （vscode和webstorm编辑器）快捷方法

~~~javascript
//vscode "/**"+'enter'
//webstorm "/**"+enter
~~~

### bodyParser.urlencoded({extended:false/true})

~~~css
extended:false/true的区别
bodyParser.urlencoded 用来解析 request 中 body的 urlencoded字符， 只支持utf-8的编码的字符,也支持自动的解析gzip和 zlib
返回的对象是一个键值对，当extended为false的时候，键值对中的值就为'String'或'Array'形式，为true的时候，则可为任何数据类型。
extended: false：表示使用系统模块querystring来处理，也是官方推荐的 默认的值为false
extended: true：表示使用第三方模块qs来处理

~~~

### 用axios调用后台接口时,baseurl自己变成了localhost,怎么改呢

~~~css
不在后端设置完整的路径
在前端调用时 baseURL+文件名称 可以解决baseURL变成localhost
~~~

### express中 重新定义html静态文件的文件夹名称

~~~css
app.set('views',path.join(__dirname,'src'))//views 是默认值 后面是修改值
~~~

### 数据库

#### mysql

##### 创建数据库的操作

~~~css
1.create database python3(这是数据库创建的名称，自己定义) charset=utf8;
2.show databases;列出所有的database
3.select database(); 当前的数据库名
4.use python3(数据库名)； 切换数据库
5.drop python3; 删除数据库
~~~

##### 数据库中表的操作

~~~css
1.create table students(
	id int not null auto_increment primary key,
	name varchar(18) not null,
	gender bit default 0
);
2.alter table students add|drop|change （字段的类型和约束）；
	alter table students add isDel bit default 0;
3.drop table students(表名);删除表名
4.desc students; 查看表结构
5.rename table xxx(旧表名) to xxx(新表名)
~~~

##### 数据库中表的数据操作

~~~css
1.select * from xxx（表名）;
~~~

~~~css
2.insert into xxx values();
全列插入：insert into 表名 values(...)
缺省插入：insert into 表名(列1,...) values(值1,...)
同时插入多条数据：insert into 表名 values(...),(...)...;
或insert into 表名(列1,...) values(值1,...),(值1,...)...;
~~~

~~~css
3.update
update xxx(表名) set 列1=值1,... where 条件
~~~

~~~css
4.delete
delete from 表名 where 条件 //物理删除
update xxx set isDel=1 where...;
~~~

##### mysql中的子查询

~~~css
**子查询中，程序先运行嵌套在最内层的语句，再运行外层的语句**

子查询一共有三种方法
1.where 中的子查询
	select goods_id,goods_name,goods_price from goods where goods_id=(select max(goods_id) from goods)
where 列=（内层sql）则内存sql返回的必须是单行单列 单个值
where 列 in (内层sql) 则内层sql返回的必须是单列，但是可以是多行

2.from 型子查询
***查询结果在结构上可以当成表看，即可以再次查询

~~~



### Express 

#### https和http协议的使用

~~~css
express=require('express')
app=express()
http=require('http').createServer(app)
//https需要openssl生成ssl证书
fs=require('fs')
let credential={}
credential.key=fs.readFileSync('private.pem')
credential.cert=fs.readFileSync('file.crt')
https=require('https').createServer(credential,app)
~~~

+ openssl证书的生成的方法

  ~~~css
  //生成私钥key文件
  1.openssl genrsa 1024 private.pem
  //通过私钥生成CSR证书签名
  2.openssl req -new -key private.pem -out csr.pem
  //通过私钥文件和csr证书签名生成证书文件
  3.openssl x509 -req -days 365 -in csr.pem -signkey private.pem -out file.crt
  
  private.pem 私钥
  csr.pem		证书签名
  file.crt	证书文件
  ~~~


#### express 设置返回的状态值的方法

~~~css
res.status(204)
~~~

#### express中动态路由的设置方法

~~~css
1.app.get('/user/:aid',(req,res)=>{}) 获取动态路由的参数方法为 req.params 返回的结果为：{aid:'你在路由user/后面中输入的字符串'}
2.app.get('/user/*.png',(req,res)=>{}) 这也是一种动态的路由方式 获取它的参数的方法为 req.params 
返回的结果为 {'0':'你在/user/后面输入的所有字符'} 如{'0':'1234'} {'0','324asdfa'} 都行
req.params[0] 即可获取到值
~~~

#### cookie 的过期时间是世界时间

~~~css 
expires=date.toUTCString()

在原生模块中设置 cookie的方式 为
res.setHeader('Set-Cookie','key=value; key=value; path=/; expires=')
注意的是：path expires 有默认值

~~~



### cheerio 只能是对标签使用juqery的dom操作

~~~css
$=cheerio.load(url)
//出现乱码的问题
$=cheerio.load(url,{decodeEntities:false})

~~~

### nodejs 的优缺点

~~~css
优点：
1、高并发
2.异步
3.事件驱动
4.单线程
缺点：
1.大量的匿名函数，使得异常错误的解读困难
2.try/catch只能捕获同步代码的异常，nodejs对异步代码的异常捕获较为困难
~~~

### 开发server端和前端的区别

~~~css
server服务端需要考虑的有：
1.服务的稳定性
	server端可能会遭受各种恶意攻击和误操作
	单个客户端可以意外挂掉，但是服务端不能
	node中用pm2做进程守候，一旦挂掉，自己会重启
2.安全
	server端要随时准备接收各种恶意攻击，前端则少很多
	如越权操作，数据库攻击等
	nodejs会登陆验证，防止越权操作。预防xss攻击和sql注入
3.cpu和内存(优化和扩展)
	客户端独占一个浏览器，内存和cpu都不是问题
	server端要承载很多请求，cpu和内存都是稀缺资源
	node用stream写日志，使用redis存session
4.日志的管理=> 日志的写入、日志的分析、日志的管理
    前端也会参与写日志，但只是日志的发起方，不关心后续
    server端要记录日志，存储日志，分析日志，前端不关心
    nodejs会有多种日志记录方式，以及如何分析日志
5.集群和服务拆分
	产品发展速度快，流量可能会迅速增加
	如何通过扩展机器和服务拆分来承载大流量？
	nodejs是单机器开发，但是从设计上支持服务拆分
~~~

### path内置模块

~~~css 
path.join(__dirname,'') //前面的__dirname和后面的路径进行拼接  当前文件的根路径和后面的输入路径拼接
path.resolve(__dirname,'') //不管后面的路径是哪个，返回的都是 绝对路径

let ret=path.resolve(__dirname,'E:\\download\\file\\01Nodejs+MongoDb')
console.log(ret)//返回 E:\download\file\01Nodejs+MongoDb
let ret2=path.join(__dirname,'E:\\download\\file\\01Nodejs+MongoDb')
console.log(ret2)//G:\study\html5W2\d9\E:\download\file\01Nodejs+MongoDb
~~~

### 网络通信

+ 网络分层

  ~~~css
  OSI七成模型
  	(数据链路层、物理层)、网络层、传输层、（应用、会话、表示层）
  TCP/IP四层模型
  	链路层、网络层、传输层、应用层
  ~~~

+ 端口数量

  ~~~css
  linux系统中的端口数量为 2**16 =65536 0~65535
  
  知名端口：0~1023
  80给http
  21给ftp
  443给https
  
  端口的作用：就是通过IP+端口号区别不同的服务
  
  ~~~

+ IP

  ~~~css
  IP就是在网络中标记一台电脑的一串数字
  在网络中标记一台电脑的数字 在本地局域网中是唯一的
  
  子网掩码：确定IP的网络号和主机号，他不能单独存在，必须和IP地址一起使用
  
  子网掩码：也是32位
  左边网络号 ，用二级制1表示
  右边主机号：用二进制0表示
  255.255.255.0 
  255.255.255 代表的是网络号
  0		代表的是主机号 
  主机号尾端不为 0 和  1
  
  ~~~


### nginx 部署问题

~~~css
nginx的目录
/usr/local
启动 注意是以root权限操作
/usr/local/nginx/sbin   ./nginx 
停止
/usr/local/nginx/sbin   ./nginx -s stop
重启
/usr/local/nginx/sbin   ./nginx -s reload
配置 
/usr/local/nginx/conf    打开配置文件 nginx.conf 

配置方法 
1.反向代理 **反向代理时 proxy_pass 后面的路由路径需要 加上 http:协议**
	输入 www.123.com 打开百度
	a.先修改 本地 hosts 文件 添加 虚拟机IP地址（nginx所在的机器） 和www.123.com 绑定
	b.nginx中 server块中 
		｛
			listen 80;
			server_name 192.168.1.115;
            location / {
				proxy_pass http://www.baidu.com
            }
		｝
** 如果配置报了 404的错误时 是因为 路径配置有错误
 默认会将location后带的URL添加到路径的最末尾
    如：location ~ /edu/ {
        root /html/;
        index index.html;
    }
    web中真实匹配的路径为：url/html/edu/index.html
        不是我以为的 url/edu/index.html
~~~

~~~css
***相当重要的部分
location [=|~|^~|~*] uri{}
=:的意思是 uri中没有正则匹配
~：uri中有正则匹配的规则,而且区分大小写
^~:很少用暂不用理解
~*：uri中有正则匹配的规则，而且不区分大小写
~~~

#### 负载均衡

~~~css
在server块的前面（不在server{}内,在http{}内）
upstream [本次负载均衡服务的名称]{
    **使用哪种方式 实现**;
    server ip地址； 不加 http 
    server ip地址；
}
location / {
    proxy_pass http://my_servers;  *********重要的事件说三遍 加上http:// http:// http://
    index index.html;
}
1.轮询
每个请求按时间顺序逐一分配到不同的后端服务器，服务器挂掉一个，自动剔除；
2.权重
weight代表权重 默认为1；数值越大分配到的客户端越多
3.ip_hash
每个请求按访问IP的hash结果分配，这样每个访客固定访问一个服务器
4.fair
按照服务器的响应时间来分配请求，响应时间短的优先分配

~~~

####  invalid URL prefix

~~~css
这种错误多是 没有添加 http://协议
~~~

#### 动静分离

~~~css
设置两个location 
一个配置 动态文件
一个配置 静态资源文件

静态资源的location /static/ {
    root /html/;
    autoindex on;*****重要的事情说三遍 audoindex on audoindex on autoindex on autoindex autoindex autoindex autoindex autoindex autoindex autoindex autoindex on 
}
~~~

