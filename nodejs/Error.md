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

