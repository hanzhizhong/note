# 	一级标题

> 引用内容1

> 引用内容2
>
> > 引用内容3
> >
> > helloworld >空格+书写的内容就是引用格式的书写方法
>
> ##### 世界那么大，我想去看看



`string str1="hello;"单行代码的引用方法  用"``的反向单引号就好"`

`我是那个渺小的存在，不存在于过去，将来也不会留下什么`;

~~~javascript
let a=123;
function show(){
    console.log(a)
}
show()


~~~

~~~javascript
//这是javascript的代码展示区域
//箭头函数
/*
多行的注释
*/
写入代码区域的方法：~~~后面加上语言
~~~

#### 列表

##### 无序列表

* 无序列表1
* 无序列表二 使用方法为：*空格+文字即可

+ 无序列表3 的使用方法 ：+空格+文字就好

- 无序列表4的使用方法 ： -空格+文字就好

##### 多行无序列表

* 多行无序列表
  * * 多行无序列表2
    * 多行无序列表3
    * 首发打对方



* 多行无序列表的使用方法 *tag后面加文字 
  * 即可的交罚款 的会计法快递费
  * 受到发达啥的积分卡
    * 受到罚款的积分开始f 也可以在*tagtag后面加文字

##### 有序列表

1. 有序列表的使用方法 数字1.空格后面加文字
2. 按enter键后自动就是有序列表
3. 这是效果

##### 多行有序列表

1. 多行有序列表的使用
   1. 和多行无序列表的使用方法一样 数字1.tag+文字就好
      1. 也可以使用1.tagtag的格式

##### 任务列表

- [ ] 抽烟
- [ ] 喝酒
  - [ ] 烫头阿萨德积分卡京东方山东发		
    - [ ] ​	使用方法为 -空格[空格]空格文字

- [ ] 使用方法-空格[空格]空格文字说明

#### 表格

| 姓名 | 年龄 | 性别 | 手机号 |
| ---- | ---- | ---- | ------ |
|      |      |      |        |

`使用表格的第一种方法：|text|text|text|回车键`

| :--d | :--- | :--- | :--- |
| ---- | ---- | ---- | ---- |
|      |      |      |      |

`第二种方法：|:---|:---|:---|:---|回车键`

| 张三 | 23   | 男   | 17323446745 |
| ---- | ---- | ---- | ----------- |
|      |      |      |             |

| name | age  | gender |
| ---- | ---- | ------ |
|      |      |        |



## 	二级标题

### 链接

#### 图片

![image-20191126094642323](assets/image-20191126094642323.png)

#### 超链接

[百度][https://www.baidu.com] [http://www.baidu.com]



`参考式链接`

[CSDN][csdn网址]

`自动链接`<https://github.com>



### 三级标题

#### 斜体

`加粗的方法：`**加粗的文字**

~~~javascript
加粗的使用方法为：**加粗的文字**
~~~

#### 下划线

<u>下划线的使用方法</u>

`在 <u>标签中写入</u>`

#### 删除线

~~这是删除的效果~~

`使用的方法为：~~要删除的文字说明~~`

#### 分隔线

***

---

___

`使用方法为：***或者---`



#### 脚注

这就是脚注[^1].

[^1]: 这是脚注内容

脚注的使用方法[^font]

[^font]:sdfaldfj

~~~javascript
使用的方法为:[^脚注名称]
换行
[^脚注名称]:冒号一定要有 后面加上说明文字
~~~





#### 四级标题

##### 五级标题

###### 六级标题及

### 生成项目的目录结构图方法

+ cd 到当前的项目根目录下
+ tree /f > 文件名.txt

#### 生成的文件名 统一命名方法

~~~css
文件名-作用-作用.[扩展名]
文件夹：大类别的使用 名词 【单个】
嵌套的文件夹使用 文件的名称方式 配合-
~~~



### github的开源项目查找方法

+ 搜索栏中输入： in:name nodejs stars:>1000 
+ 搜索栏中输入：in:readme nodejs stars:>1000 
+ 搜索栏中输入：in description 微服务 language:javascript pushed:>2019-09-03
### nodejs express框架插件

~~~css
svg-captcha //图片验证码
art-template/express-art-template //html的引擎
body-parser
express
cros //跨域解决方案
cherrio//正则插件
nodemailer//邮箱插件
multer
crusf
cookie-parser
express-session
connect-redis//需要和redis,express-session配合使用
	const redis=require('redis')
	const session=require('express-session')
	const RedisStore=require('connect-redis')
	let client=redis.createClient()
app.use(session({
    secret:"your secret string",
    resave:false,
    saveUninitial....,
        store:new RedisStore({client:client})
}))

passport：第三方登陆验证
~~~

### nodejs koa框架 插件

~~~css
koa
koa-static
koa-router 
//router=require('koa-router'();
//app.use(router.routes());启动路由
//app.use(router.allowedMethods()); 在http请求options方法是 在请求头 allow 会显示所有的路径下所有定义的请求方法（get,post,patch，put,delete）2.错误提示 405（没实现）501（不允许）

art-template
koa-art-template

//static
app.use(static(path.join(__dirname,'public')))
//模板渲染
render({
    root:path.join(__dirname,'views'),
    extname:'.html',//文件后缀名
    debug:true||process.env.NODE_ENV!=='production'
})

koa-bodyparse
//用法
const bodyParser=require('koa-bodyparser')
app.use(bodyParser()) 提前使用
//post提交的信息存储在cxt.request.body中，空时body=｛｝
koa-body 和koa-bodyparse的区别就是koa-body支持multipart/form-data

koa-json-error 
jsonError({
    postFormat:(e,{stack,...rest})=>{
        return process.env.NODE_EVN==='production'?rest:{stack,...rest}
    }
})


~~~

### electron 插件

~~~css
electron-rebuild //编译原生的模块 
robot.js//智能自动化 控制鼠标、键盘、阅读屏幕
vkey //根据键值找对应的键盘字母
mousetrap
node-ffi//node版本低于10
ffi-napi //node版本大于10
electron-is-dev
electron-notification-state 检测是否可以使用通知
~~~

### meta 键

~~~css
如果使用的是windows，那么meta键表示的就是键盘上的win键（及键盘上有窗口图案的键）

如果使用的是苹果电脑，meta键表示的是Cmd键；
~~~

### 虚拟机的操作

+ 在windows系统中使用FTPL连接 虚拟机 

  ~~~css
  1.安装vsftpd
  2.配置vsftpd.conf
  3.安装sudo apt install firewalld
  4.开启firewalld的服务 sudo firewall-cmd --state
  5.开启http服务 sudo firewall-cmd --add-service=http --permanent
  5.开启20、21端口 sudo firewall-cmd --add-port=20/tcp --permanent
  6.客户端使用 filezilla 软件 连接方式 ：
  			加密：选择 普通的ftp(不安全)的
  	在传输设置：传输协议 选择 主动模式
  	
  ~~~


### ftp win7系统服务器远程的链接方法

~~~css 
win7的磁盘管理在 【计算机】右键 【管理】
win7 【控制面板】-【程序或功能】-【打开或关闭windows功能】
win7的 【控制面板】-【管理工具】中选择IIs中添加ftp服务器

1. 打开"控制面板->系统和安全->windows防火墙"

2. 点击左侧的"高级设置"

3. 在"出站规则"中, 启用与FTP server相关的一切规则

4. 在"入站规则"中, 在右侧点击"新建规则", 然后按照步骤一步一步操作, 开放端口21
~~~

### windows中重置 cmd的设置

~~~css
win+r 运行
regedit 注册表打开
HKEY_CURRENT_USER console =>删除带.cmd文件夹

设置cmd 显示中文乱码的问题
regedit 
如下图
~~~

![image-20200707165214192](assets/image-20200707165214192.png)

### 服务器ubuntu

#### firewall-cmd的操作方法

~~~css
所有的操作是在 sudo 超级管理员权限下设置的
1.添加 
firewall-cmd --add-port=80/tcp --permanent
2.删除
firewall-cmd --remove-port=80/tcp --permanent 
3.所有的有新操作时需要重新加载 reload
firewall-cmd --reload 
4.查看所有的开放的端口号
firewall-cmd --list-all

~~~

### OpenSSL是一个开放源代码的软件包库

~~~css
应用程序可以使用这个包进行安全通信，避免窃听，同时确认另一端连接人的身份
~~~

#### 基本功能

~~~css
SSL协议库、应用程序以及密码算法库
作为基于密码学的安全开发包，OpenSSL提供的功能相当强大和全面，攘括了主要的密码算法、常用的密码和证书管理功能以及SSL协议
~~~

#### 基本操作

~~~css
>openssl list -digest-algorithms 显示所有的加密算法
~~~

#### 证书的形式

~~~css
1.带私钥的证书
	包含了公钥和私钥的二进制格式的证书形式，以.pfx作为证书的后缀名
2.二进制编码的证书
	证书中没有私钥，DER编码二进制格式的证书文件，以cer作为证书文件后缀名
3.Base64编码证书
	证书中没有私钥，base64编码格式的证书文件，也以cer作为证书文件的后缀名

pfx既可以导出pfx证书，也可以导出cer证书
cer证书不能导出pfx证书
~~~

#### 知识点

~~~css
对称加密：用同一个密码 加密/解密 文件
非对称加密：加密用一个密码，解密用另一个密码
加密解密：公钥加密，私钥解密
	公钥加密的数据只有到了有私钥解密的人手里才能解开成有效的数据

签名和验证签名：私钥加密数据，公钥解密
	用私钥进行数据签名，那这个数据就只有配对的公钥可以解开，有这个私钥的只有你，配对的公钥解开了数据，就说明这数据是你发的，这个被称为签名

加密的算法：RSA/DSA/SHA/MD5
RSA:可用于加密/解密，也可用于签名验证
DSA:只能用于签名
SHA和MD5:摘要算法
	就是通过一种算法，依据数据内容生成一种固定长度的摘要，这串摘要值与原始数据存在对应关系
	实际应用过程中,因为需要加密的数据可能会很大,进行加密费时费力,所以一般都会把原数据先进行摘要,然后对这个摘要值进行加密,将原数据的明文和加密后的摘要值一起传给你.这样你解开加密后的摘要值,再和你得到的数据进行的摘要值对应一下就可以知道数据有没有被修改了。
	实际应用中,一般都是和对方交换公钥,然后你要发给对方的数据,用他的公钥加密,他得到后用他的私钥解密,他要发给你的数据,用你的公钥加密,你得到后用你的私钥解密,这样最大程度保证了安全性.

CA/PEM/DER/X509/PKCS
	一般的公钥不会用明文传输给别人的,正常情况下都会生成一个文件,这个文件就是公钥文件,然后这个文件可以交给其他人用于加密,但是传输过程中如果有人恶意破坏,将你的公钥换成了他的公钥,然后得到公钥的一方加密数据,不是他就可以用他自己的密钥解密看到数据了吗,为了解决这个问题,需要一个公证方来做这个事,任何人都可以找它来确认公钥是谁发的.这就是CA,CA确认公钥的原理也很简单,它将它自己的公钥发布给所有人,然后一个想要发布自己公钥的人可以将自己的公钥和一些身份信息发给CA,CA用自己的密钥进行加密,这里也可以称为签名.然后这个包含了你的公钥和你的信息的文件就可以称为证书文件了.这样一来所有得到一些公钥文件的人,通过CA的公钥解密了文件,如果正常解密那么机密后里面的信息一定是真的,因为加密方只可能是CA,其他人没它的密钥啊.这样你解开公钥文件,看看里面的信息就知道这个是不是那个你需要用来加密的公钥了.

　　实际应用中,一般人都不会找CA去签名,因为那是收钱的,所以可以自己做一个自签名的证书文件,就是自己生成一对密钥,然后再用自己生成的另外一对密钥对这对密钥进行签名,这个只用于真正需要签名证书的人,普通的加密解密数据,直接用公钥和私钥来做就可以了.

CSR:证书签名请求
CRLs:证书回收列表
crt:是CA认证后的证书文件，签署人用自己的key给你签署凭证
	server.crt:最重要的是有一个common name，可以写你的名字或者域名。如果为了https申请，这个必须和域名吻合，否则会引发浏览器警报。生成的csr文件讲给CA签名后形成服务端自己的证书。

秘钥文件的格式用openssl生成的就只有PEM和DER两种格式，
PEM:是将秘钥用base64编码表示出来的，直接打开看到一串英文字母
DER：是二进制的秘钥文件
X509:是通用的证书文件格式定义
PKCS:指定的存放秘钥的文件标准

pem der x509 pkcs这几种格式是可以互相转换的

~~~



#### 生成证书

~~~js
1.创建私钥
openssl genrsa -out ca-key.pem 1024
2.创建证书请求 ：将私钥转成证书
openssl req -new -out ca-req.csr -key ca-key.pem
3.将证书请求转成证书
openssl x509 -req -in ca-req.csr -out ca.cer -signkey ca-key.pem -days 3650

ca.cer ca证书
~~~

#### 服务端生成正式

~~~js
server 证书: opsenssl genrsa -out server-key.pem 1-34 

~~~

