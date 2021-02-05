### AMD/CMD/CommonJS/ES6的对比

~~~css
都是在js代码模块化中使用的，AMD、CMD、Commonjs是ES5中提供的模块化编程方案，import/export是ES6中定义新增的
AMD-异步模块定义 requireJS脚步语言中对模块定义的规范化
CMD-是 SeaJS在推广过程中对模块定义的规范，Seajs是CMD概念的一个实现，由淘宝团队开发的js框架
CommonJS规范---是通过module.exports定义的，Nodejs端是使用CommonJS规范的，前端浏览器一般使用AMD、CMD、ES6等定义模块化开发的
~~~



### Octal literals are not allowed in strict mode

~~~css
JS严格模式禁止使用八进制字面量，在我的数组中id使用了0，老的八进制（0前缀）
arr=[
{id:01,name:'aaa',age:23},
{id:02,name:'aaa',age:23},
{id:03,name:'aaa',age:23}
]
去掉0

~~~

### iframe的常用方法

~~~js
//设置iframe高度
iframe.height=iframe.contentWindow.document.body.offsetHeight;
//获取iframe中的元素
iframe.contentWindow.document.getElementById('div1').style.color="red"//兼容
iframe.contentDocument.getElementById('div1').style.background='red'//ie6,ie7不兼容
~~~

#### window.parent,window.top,window.self详解

~~~css
当页面中有frameset或者iframe的页面时，parent就是父窗口,top就是最顶端父窗口，self就是当前窗口,opener使用open打开当前的窗口

window.top 顶层窗口，就是浏览器窗口
window.parent 返回父窗口
window.self 	返回窗口自身 
~~~



### 秒转时分秒

#### toUTCString() toGMTString() toString()

~~~css 
toUTCString()和toGMTString()
由于目前UTC已经取代GMT作为新的世界时间标准，使用toGMTString()和toUTCString()两种方法返回字符串的格式和内容均相同
toString() 返回的是 '中国标准时间'
~~~



~~~javascript
var itime=9045
let h=Math.floor(itime/3600)
let m=Math.floor(itime%3600/60)
let s=Math.floor(itime%3600%60)
h=h>=10?h:'0'+h
m=m>=10?m:'0'+m
s=s>=10?s:'0'+s

~~~

### ES6中的Array.from（）方法

> + 针对{ }对象类型的转换方式
>
>   + ｛｝中有length属性的并且key值是数字的，转换后的数据格式为数组
>
>     ![1575293136341](assets/1575293136341.png)
>
>   + ｛｝中没有length属性时 但是key是数字，转成空数组 【】
>
>   + { }中有length 但是key没有数字，而是字符串时，数组为的值为[undefined,undefined。。。]
>
>     ![1575293580779](assets/1575293580779.png)

### canvas

#### 用来做擦除的方法

+ globalCompositeOperation

  ~~~javascript
  destination-out 新画的图型是用来擦除原有的图
  destination-in
  destination-over 
  
  xor
  source-over 
  ~~~

+ canvas.toDataURL()  返回的是一个带有图片格式的数据信息

  ![image-20191211085242416](assets/image-20191211085242416.png)

+ closePath()

  > cxt.fill() 不需要使用closePath()的封闭
  >
  > cxt.stroke() 是需要closePath()的封闭的顺序是 cxt.stroke() ; cxt.closePath()

### navigator

#### navigator.mediaDevices.getUserMedia(constraints)

> 获取媒体设备的能力 ，constraints是约束媒体流的
>
> ~~~javascript
> let constraints={audio:true,video:{width:num,height:num}}
> 
> navigator.mediaDevices.getUserMedia(constraints) 返回的是Promise对象
> 直接使用 .then()的方法取值
> ~~~
>

### 浏览器自带的浏览器调试工具修改代码直接关联到文件并保存到本地

> + 目前只能做到 把本地的文件在浏览器中修改后同步保存
>   + 方法： add folder to workplace
> + 服务器的文件暂时无法做到同步修改后保存到本地

### async/await

> ![image-20191225102103907](assets/image-20191225102103907.png)
>
> + promise.all 是并行执行异步操作  跑的慢的为结束标准
> + promise.race 是并行执行异步操作   跑的快的为结束标准 结束后不在执行resolve 

###  **encodeURI() 函数可把字符串作为 URI 进行编码** 

+  encodeURI是对整个uri进行编码的，而encodeURIComponent是对uri中部分内容进行编码。 
+ ![image-20200102092853925](assets/image-20200102092853925.png)

### git  问题(Non-fast-forward)的出现原因是: git仓库中已有一部分代码, 它不允许你直接把你的代码覆盖上去。于是你有2个选择方式: 

~~~javascript	
1. 强推，即利用强覆盖方式用你本地的代码替代git仓库内的内容: git push -f

2. 先把git的东西fetch到你本地然后merge后再push

    - $ git fetch
    - $ git merge

这2句命令等价于

$ git pull 
~~~

### 游戏框架 Phaser.js

~~~css
专门用于桌面和移动端HTML5 2D游戏开发框架，基于浏览器可支持自由切换
~~~

|                      | Game                                                         | 游戏                                                         |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| core核心             | Group<br />World<br />Loader<br />Time<br />Camera<br />StateManger<br />TweenManger<br />SoundManger<br />InputManger<br />ScaleManger | 组<br />世界<br />载入器<br />时间<br />摄像机<br />状态管理器<br />补间动画管理器<br />声音管理器<br />输入管理器<br />缩放管理器 |
| Game Objects游戏对象 | Factory(game.add)<br />Creator(game.make)<br />Sprite<br />Image<br />Sound<br />Emitter<br />Partical<br />Text<br />Tween<br />BitmapText<br />Tilemap<br />BitmapData<br />RetroFont<br />Button<br />Animation<br />Graphics<br />RenderTexture<br />TileSprite | 工厂<br />创建者<br />精灵<br />图像<br />声音<br />发射器<br />粒子<br />文本<br />补间动画<br />位图文字<br />瓦片地图<br />位图数据<br />复古字体<br />按钮<br />动画<br />图形<br />渲染纹理<br />瓦片精灵 |
| Gemotry几何图形      | Circle<br />Rectangle<br />Point<br />Line<br />Ellipse<br />Polygon | 圆<br />矩形<br />点<br />线<br />椭圆<br />多边形           |
| Physics物理引擎      | Aracde Physics<br />Body<br />P2 Physics<br />Spring<br />CollisionGroup<br />ContactMaterial<br />Ninja Physics | Arcade（拱廊）物理引擎<br />刚体<br />P2<br />弹簧<br />碰撞组<br />接触物质<br />Ninja物理引擎 |
| Input输入            | Input Handler<br />Pointer<br />Mouse<br />Keyboard<br />Key<br />Gamepad | 输入处理<br />指针<br />鼠标<br />键盘<br />按键<br />游戏手柄 |

#### 核心 core

~~~css
Game
Game是游戏的核心，提供一个快速调用公共函数和处理启动过程的渠道

Group
Group（组）用于显示各种对象（包括Sprites和Images）的容器
Group 将 显示/场景图 组成了逻辑树的结构，应用到Group上的变化会应用到它的子对象上。
如：当Group旋转、缩放、移动时，所有的子对象同时也会移动、旋转、缩放
此外，Group也提供了对快速移动对象池和对象回收的支持
Group可以显示对象，同时也可以作为其他组的子对象。

World
一个游戏只拥有一个World。Wrold是一个抽象空间，所有的游戏对象都生存在World中。可以是任意尺寸的大小，不受舞台边界的限制。可以通过相机查看世界。所有的游戏对象都以基于世界的坐标而生存与world。默认情况下world和舞台尺寸一致

Loader
Loader加载器 用于处理所有外部内容的加载，例如图像、声音、纹理图集和数据文件
它把iamge载入和XMLHttpRequest对象集合在一起，提供了载入进度显示和载入完成的回调功能

Time
核心内部游戏时钟
维护了一个消逝时间，计算消逝时间值。用于游戏对象的运动、补间动画，还处理一个标准的定时器池。
要创建一个普通的定时事件，可使用Phaser.Timer.

Camera
Camera摄像机 观察游戏世界的视野。有一个确定的位置和大小，并且只渲染在它视野范围内的对象。游戏启动的时候，会自动创建一个跟舞台相同大小的摄像机。通过改变Phaser.Camera.x/y的值可以在世界中移动摄像机

State Manager
状态管理器负责载入，设置、切换游戏状态。

Tween Manager
Phaser.Game维护了一个单一的TweenManager实例，所有补间动画对象都是由它创建和更新的。补间被钩入游戏时钟中，使系统暂停，并根据游戏状态而调整。

Sound Manager
声音管理器负责通过传统的html音频标签或者web音频来播放音频

Input Manager 
Phaser.Input是所有输入设备的管理器，包括鼠标、键盘、触摸和MSPointer. 输入管理器在游戏主循环中会自动被更新。

Scale Manager
ScaleManager对象控制了缩放、大小变化和游戏大小与显示画布之间的对齐操作
游戏大小是游戏的逻辑尺寸，显示画布作为HTML元素也有自己的尺寸
~~~

#### 游戏对象 Game objects

~~~css
Factory(game.add)
GameObjectFactory是一个使用game.add来创建很多常见游戏对象的快速方法。
创建出来的对象会自动被添加到适当的管理器、世界、或者用户指定的组中

Creator game.make
GameObjectCreator是一个创建游戏对象的快速方法，但是并不会把对象添加到游戏世界中。对象可以被game.make访问

Sprite
精灵是游戏的生命体，几乎可用于所有的可视化物体。
基本上，精灵是有一套坐标和渲染在画布上的纹理所组成。

Image
图像是一个轻量级对象，你可以使用它来显示任何不需要物理引擎或者动画的任务东西。它可以旋转、缩放、剪切、并接收输入事件。可以完美的用于标识、背景、简单按钮和其他非精灵类图形

Emitter
Emitter是一个使用Arcade物理引擎的轻量级粒子发射器。它可用于一次性的爆炸，或者像雨、火那样的连续性效果。它所有真正做的就是在设定的时间间隔里发射出 Particle（粒子）对象，并相应的修正他们的位置和速度。

Particle
粒子是精灵的扩展类，由粒子发射器(Phaser.Particles.Arcade.Emitter)发射出去。

Tween
补间 允许你在一个指定的时间周期内更改一个目标对象的一个或多个属性。

Button
按钮是一个特殊类型的精灵，他能自动建立对指针事件的处理。
四种按钮相应的状态 over out down up

~~~

#### 物理引擎 Physics

~~~css
Arcade
Arcade引擎包含了一些碰撞、重叠、运动等函数。

Body
刚体是一个单一的精灵，所有的物理操作都是针对这个刚体，而不是这个精灵本身。如：你设置的速度、加速度、边界值都是针对的刚体

P2 
P2物理引擎可以用来创建材料、监听事件、在物理仿真中添加刚体

Spring
创建一根线性的链接两个刚体的弹簧。弹簧有静止长度、阻力、刚度等属性。

Ninja Physics
Ninja物理引擎作用于Flash
~~~





### Mock.js的使用方法

#### 1.mock.js假数据文件的创建

~~~js
//方法
function login(){
    return Mock.mock({'username':'hell'})
}
//拦截的ajax的请求路径
Mock.mock('/login','post',login)
~~~

#### 2.调用方法

~~~js
//先引用上面的mock的假数据js文件
<button onclick="loginIn()"></button>
async function loginIn(){
    try{
        let ret=await axios.post('/login')
        console.log(ret.data)
    }catch(err){
        console.error(err)
    }
}

~~~



### ECMA 语法

#### [].slice.call

~~~js
[].slice.call() 常用来将类数组转化为真正的数组
[].slice() === Array.prototype.slice() //true
~~~

#### [].shift.call(arguments)

~~~js
取出数组中第一元素

~~~

#### fn&&fn()和typeof fn==="function"&&fn()

~~~css
都是表示 fn不存在就什么都不做，不会报错，fn存在才尝试执行fn
但是第一中情况中有错误： fn存在，但是它可能不是function类型，执行fn()就会报错。
所以 正确的写法为 typeof fn==='function'&&fn()
~~~



#### switch case 使用范围的方法

~~~css
<script>
    var x=10;
switch(true){
    case x>0&&x<10:   //条件必须分开书写 不能写成 0<x<10
    	console.log('sss')
    	break;
   	case x>10:
    	console.log('bb')
    	break;
    case x===10:
    	console.log('ccc')
    	break;
}
</script>
~~~

#### requestFrameAnimation的停止方法

~~~css
this.stopID=window.requestFrameAnimation(函数名)调用后会返回一个唯一的ID值
window.cancelAnimationFrame(this.stopID) 就会停止动画
~~~

#### new FormData()的使用方法

~~~css
//获得页面当中的form表单元素
var form=document.querySelector("#form");
//将获得的表单元素作为参数，对formData进行初始化
var formdata=new FormData(form);

使用 formdata.get('name') input中的name属性
get(key) 获取
set(key,value) 修改
append(key,value) 末尾添加
delete(key) 删除
has(key) 判断

遍历的方法为 for(let temp of formdata){}
~~~

#### DOMContentLoaded事件和loaded区别

~~~css
DOMContentLoaded是在html解析完文档时触发：
	1.如果文档中没有脚步，那么就在解析完dom树和css样式后触发
	2.如果文档中有脚步，那么会脚步出阻塞，在解析完脚步和css dom后触发
DOMContentLoaded是不需要等待图片等其他资源的加载完成
loaded 不一样，是在所有资源加载完成后触发，页面上所有的资源（图片，音频，视频等）被加载以后才会触发load事件，简单来说，页面的load事件会在DOMContentLoaded被触发之后才触发。
~~~

#### 内存泄漏

~~~css
内存泄露的意思是：动态分配的堆内存由于某种原因程序未释放或者无法释放，导致内存的浪费，而导致程序运行速度的减慢甚至系统的崩溃等严重后果
~~~

#### 正则 replace的回调函数的匹配方法

~~~css
str.replace(/[<>&]*/gi,function(match){
    //match为匹配到的的字符串
    switch(match){
        case '<':
        	return '&lt;'
       	case ">":
			return "&gt;"
    }
})
~~~

#### prop和attr的属性区别

~~~css
prop使用时针对html标签固有的属性 如 title value 等
attr使用时针对 html的自定义的属性名 
具有true/false的属性值 的属性名如 disabled checked selected 时使用 prop

<input type="text" name="username">
使用原生js在input发生变化后获取值的 结果
input.value='你输入的当前值'
input.getAttrbuite('value') 为null

在使用jquery时
$('input').prop('value')显示的是发生变化的值
$('input').attr('value')显示的是undefined 
~~~

#### 函数的节流

~~~js
函数节流的意思是：在一段时间内，核心代码块只执行一次
优化高频率执行js代码的方法
应用场景：上啦加载更多 滚动浏览器滚动条的时候

节流：连续触发的事件，在一定时间内只有第一次触发，其他时间不触发
~~~

#### 函数防抖

~~~js
事件保持触发，一定事件内没有触发
核心：在一定时间内连续的函数调用，只让其执行一次
防抖：连续触发的事件，在一定时间内不触发，到达设定的时间后执行一次，即只有最后一次有效，其他触发没有效果
应用场景： 搜索框
用户停止输入的时候才去触发查询的请求，这时候函数防抖可以帮到我们。
~~~

#### 箭头函数和普通函数的区别

~~~js
1.this的指向不同
	普通声明式函数的this在定义函数时是不确定是谁的，只有在调用当前的函数时才能知道this是指向谁
    function show(){
		console.log(this)
    }
	show() //this指向的是window
	document.onclick=show; //this指向的是doucment
	
	箭头函数
    let fn=()=>{console.log(this)}//this 指向的是fn在创建时的父级上下文环境，而且不会变更，这里指向的是window
   
2.箭头函数中是没有arguments这个 函数参数的类数组对象
3.箭头函数不能用作构造函数
4.箭头函数不能用作生成器
  
~~~



#### Symbol 新数据类型

~~~css
typeof 的结果为 "symbol"
用法：let n=Symbol('sss')
这样就定义了一个 Symbol('sss')的常量 
常用来做 私有变量的 key值
function Person(name,gender){
    this.name=name;
    this[Symbol('gender')]=gender
}
let per=new Person('tom','male')
调用不到gender的属性值
~~~

#### 字符串模板

~~~css
unicode的使用方法
console.log('\u{1F603}') emoji地址：https://unicode.org/emoji/charts/full-emoji-list.html
或者 console.log(`\u{1f603}`)

startsWith()
endsWith()
repeat()
includes()

~~~

#### Object常用方法

~~~js
Object.defineProperty()
let title='sss'
let options={}
Object.defineProperty(options,'title',{
    configurable:true,//是否可以删除当前属性，默认为false
    value:'nothing seek,nothing find',
    writable:true,//value值是否可以改变，默认为false
    enumerable:true//是否可以用for in Object.keys()等方式遍历
})
Object.defineProperty(options,'title',{
    get(){return title},//当在调用 options[attr]的时候会自动调用get()的方法
    set(val){ title=val}//设置新值
})

Object.assign()
let obj1=Object.create() //是在obj1的__proto__指向的原型对象上增加新的属性和值

Object.is()
Object.keys()
Object.values()
Object.entries()
~~~

#### 迭代器

~~~js
迭代协议
迭代器：
	是一个对象，通过使用next()方法实现 迭代协议的任何一个对象，方法返回两个属性对象：value这是序列中的next值；done; 如果迭代到最后一个值，则它为true.
   
可迭代对象：
	必须拥有一个 [Symbol.iterator]键的属性
    
for ... in 是遍历了对象的可枚举属性
for ... of..是遍历整个对象

四种数据结构：map,set, 数组，对象
凡是具有Symbol.iterator属性的数据结构都是可迭代的对象

凡是可迭代的对象都具有的方法：
	解构赋值	let [x,y]=new Set(['a','b'])
    扩展运算符 ...

遍历的方法为 for...of 

可以调用可迭代对象的Symbol.iterator的属性进行 一步一步的遍历
let set=new Set(['a','b','c'])
let itSet=set[Symbol.iterator]();
console.log(itSet.next())//{value:'a',done:false}
	
for循环跳过当次的循环进入下一个循环 使用continue;


~~~

#### Math内置对象的常用方法

~~~js
Math.min()
Math.max()
Math.ceil()
Math.floor()

~~~

#### async/await并行和串行

~~~css
await getValue(name1)
await getValue(name2)

for(let tmp of [name1,name2]){
    await getValu(tmp)
}
上面是串行模式

并行模式
let ret1=getValue(name1)
let ret2=getValue(name2)
await ret1;
await ret2;

await Promise.all([getValue(name1),getValue(name2)])

let ret=[name1,name2].map(item=>getValue(item))
for(let tmp of ret){
    await tmp;
}

~~~

### WebAPI

#### webRTC

##### 编码器

~~~css
软件编码：x264
	用CPU进行编码
	稳定，质量好。
硬件编码：NVENC
	用显卡进行编码
	稳定性相对较低，对cpu要求低
~~~

##### 音频比特率

~~~css
比特率越高，每秒传送的数据就越多，音质就越清晰。

~~~

##### 基础知识点

~~~css
DTLS:datagram transport level security即数据报安全传输协议，提供了UDP传输场景下的安全解决方案，能防止消息窃听，篡改，身份冒充等问题

RTSP:real-time-stream-protocol协议——作为一个应用的协议层，提供了一个可扩展的框架，是的流媒体的受控和点播成为可能。具体控制具有实时特性数据的发送，单其本身不用于传输流媒体数据，依赖下层协议（RTP/RTCP）

RTMP:real-time-messaging-protocol实时消息传输协议：基于tcp
目前最流行的流媒体传输协议，广泛用于直播领域，绝大数的直播产品都是这个协议。 RTMP使用设计用来进行实时数据通信的网络协议

webRTC：web-real-time communication网页即时通信，网页浏览器实时语音对话或视频对话的API
~~~

##### webRTC和RTMP

~~~css
rtmp是客户端到服务器（peer-to-server）,webRTC是客户端到客户端的技术，peer-to-peer.
如果一个服务端实现了webRTC客户端的能力，那么它也可以被认为是一个peer，与用户端的浏览器创建链接，获得客户端推送过来的媒体数据，就完成了peer-to-server的转换
~~~

##### webrtc直播方案

~~~css
主播与连麦 用户采用 P2P 方式进行交互，然后在主播端进行混流，然后在 CDN 上进行混流，发送到观众端。
~~~

##### webrtc的视频会议方案

~~~css
1. 网状模型
A,B,C三人会议
A和B链接，B和C链接 A和C链接
三人会议就需要建立三条链接 ：3*(3-1)/2
人数和链接数的关系: n*(n-1)/2

星型模型
分为两种：1.通过某一个终端转发 2.通过服务器合成转发
1.A，B，C参加会议
	A和B链接
	B和C链接
	B转发A的音视频给C，B转发C的音视频给A
	这就是需要终端B的性能非常高
2.通过服务合成转发
每个人都将自己采集到的音视频发送到服务端，经过服务器的合成，分发给每个参加会议的人
	A,B,C都和服务器建立链接
	A，B，C把采集到的数据都发送到服务器
	服务器把A，B，C发过来的视频合成后发送给A，B，C
	有个问题是：服务器不应该把A发送到服务器的数据再发送给A
~~~

##### MCU / SFU 的区别

~~~css
MCU：multipoint control unit 多点控制单元
SFU:selective forwarding unit 选择性转发单元

~~~

##### 创建一个新的RTCPeerConnection并且配置bundlePolicy

~~~js
bundlePolicy：max-compat最大限度的兼容性，同时优化网络使用。
const config={
    iceServers:[
        {url:"turn.turn.172.17.13.14"}
    ],
    bundlePolicy:'max-compat'
}
~~~

#### webRTC Api

~~~css
new RTCPeerConnection()
new RTCPeerSessionDescription({
    type,
    sdp
})
new RTCDataChannel()
~~~



~~~js
let client_peer=new RTCPeerConnection(config)
//事件
client_peer.onaddstream=function(){
    
}
client_peer.onremovestream=function(){
    
}
//方法
client_peer.createOffer({offerToReceiveVideo:true,offerToReceiveAudio:true})//promise offer描述
client_peer.addStream()
client_peer.setLocalDescription(offer)
client_peer.setRemoveDescription(answer)

~~~



#### URL

~~~css
new URL().searchParams 
返回 URLSearchParams对象 可迭代 的
获取结果需要遍历 for of 
扩展运算符
~~~

#### web audio api

~~~css
1.创建音频上下文
var oCtx=new AudioContext();
2.创建源对象，输入流（mp3文件）
var audioSrc=oCtx.createMediaElementSource(mp3文件选择器)//
3.加工源对象，创建效果节点（3d环绕， 重音，混响...）
var analyser=oCxt.createAnalyser()
4.输出音频，为音频选择有一个目的地（耳机，扬声器）
oCxt.destination
5.连接源对象到效果器，对目的地进行效果播放


~~~

#### Worker

~~~css
html5中新增了Worker函数,Worker是构造函数，开启额外的线程。相当于过线程操作，在同一个时间内执行多个任务

new Worker('执行的js文件') 
如果报错：不支持跨域

需要开启服务才能使用
~~~

#### Worker中 数据传递和接收

~~~js
worker多线程中的 this指向和正常的js文件中指向window 不同，多线程中this指向 ：DedicatedWorkerGlobalScope
woker函数的全局

//dedicatedWorkerGlobalScope中传递数据的方法
全局对象中有一个 postMessage的方法
使用 postMessage（"传递的数据"）传递数据

主进程中获取额外线程传递的数据
var wk=new Worker('.js文件')
使用wk实例对象来接收数据
wk.onmessage=function(){
    
}

~~~

#### EventSource 服务端主动向客户端发送数据信息

~~~js
var es=new EventSource('/path')//请求服务器的路径
es.onmessage
~~~

#### 拖拽

~~~js
ondrag:拖拽	
ondragenter	//拖拽进入
ondragleave	//拖拽离开
ondragstart	//拖拽开始
ondragend 	//拖拽结束
ondragover 	//悬浮
ondrop		//丢弃事件 有一个bug需要配合 ondragover 阻止默认事件

原型对象中有 getData()和 setData()的函数
setData(key,value)
getData(key)


~~~

#### history

~~~js
history.length

history.go()
history.back()
history.forword()
history.pushState()
history.replaceState()

history.pushState(obj,title,url) //向历史记录中添加新的历史记录
obj:添加的数据是一个对象
title:'新的网页标题,一般省略了'
url:"新网页的网址"

history.replaceState(obj,title,url)//替换当前的历史记录

监听histtory的改变
window.onpopstate=function(e){
    e.state//只有通过前进或者后退箭头或者history.back() history.forword() history.go()方法操作才能获得传递的数据
}
~~~



### jsonp的使用方法

~~~css
第一种  创建标签的使用方法
function createJSONP(wd){
    let script=document.createElement('script')
    srcipt.src=`http://suggestion.baidu.com/su?wd=${wd}`
    document.body.appendChild(script)
    script.addEventListener('load',(ret)=>{
        document.body.removeChild(script)
    })
}
function callback(ret){
    console.log(ret)
}

第二种 jquery 和ajax配合使用
$.ajax({
    url:'http://suggestion.baidu.com/su?',
        data:{wd:''}//传入的参数
    type:'get',
    dataType:"jsonp",//服务器返回的数据类型为 jsonp 有回调函数的名称,
    jsonp:'callback的函数名==》在前端本地的自己定义的函数名称'//window.baidu.sug
    jsonpCallback:"window.baidu.sug"
})
//在使用$.ajax的请求方式时，error和success是使用不到的

window.baidu={
    sug:function(ret){
        console.log(ret)
    }
}
~~~

### ES6 数组方法

~~~javascript
arr.forEach((item,index,arr)=>{})
arr.map((item,index,arr)=>{ ...  return{}})//做映射改变原数据
arr.filter((item,index,arr)=>{ return ....}) //
arr.find((item,index,arr)=>{return })//返回最先符合条件的一个
arr.findIndex()
Array.from()

arr.reduce((pre,current)=>{},pre0=0) 这里的pre是初始结果，这是由第二个参数pre0指定的，current是arr数组每次的遍历值
arr.reduce((pre,current)=>{},[]) pre初始值为[]

数组中的中文排序
arr.sort((item1,item2)=>{
    return item1.localeCompare(item2,'zh')
})
~~~

### ES6 中new Map()



### ES6中的目前只有静态方法没有静态属性

~~~css
即只支持 static 方法（）{}
而类的静态属性
constructor(){}//不可以添加async 的异步方法
~~~

#### 类中的extends super

~~~css
当调用了extends时，就是完整的复制了一个父级类
的属性和方法

在子类的constructor中调用了super()方法表示的是调用父类的constructor()

super这个关键字，既可以当作函数使用，也可以当作对象使用。两种情况完全不同。

1.当作函数时，代表父类的构造函数。子类的构造函数必须执行一次super函数
这时的super()相当于 A.prototype.constructor.call(this)

2.super作为对象时，指向父类的原型对象。
	super.print() 

super在使用的时候，必须显示的指定是作为函数还是对象使用，否则会报错
	console.log(super)//报错
~~~

#### static 关键字 类的静态方法

~~~css
类相当于实例的原型，在类中定义的所有方法，都会被实例继承，但是在方法前加上 static关键字， 就表示该方法不会被实例继承，而是直接通过类来调用，这就是静态方法
~~~

![image-20200803170556323](assets/image-20200803170556323.png)

### 延时执行的方法

~~~css
function sleep(time){
    return new Promise(resolve=>{
        console.log('222')
        setTimeout(resolve,time)
    })
}

(async function(){
    console.log('111')
    await sleep(2000);
    console.log('333')
})();
~~~



### ES6的module加载实现

~~~css
传统浏览器支持脚步的异步加载
<script src="./jquery.min.js" defer></script>
<script src="./jquery.min.js" async></script>
这两种的区别是：下载完成后的执行时间不同
defer:执行时间是 在DOMContentLoaded之后执行：即在DOM结构渲染和其他同步执行的脚步加载完成后执行
async:执行时间是 在下载完成后立即执行

ES6的模块加载方式是
外联的方式是
<script type="module" src="./jquery.min.js"></script>
内联的方式是
<script type="module">
	import './jquery.min.js'
</script>
type="module" 也是异步的加载 效果等同于 defer
<script type="module" src="./jquery.min.js" defer></script>
<script type="module" src="./jquery.min.js" async></script>


~~~



### webpack 的是使用

~~~css
1.在webpack中使用css-loader style-loader less-loader时 注意 加载导入的文件路径问题
	文件路径一定要以（./.../.../）
2.loader 使用规则：下载 使用（use）
	要使用多个loader时 使用关键字 ‘use’
	使用单个loader时 使用 loader:''
3.plugins 使用规则：下载 引入 使用

~~~

### blob:javascript的对象类型

~~~css
Blob对象表示一个不可变、原始数据的类文件对象
创建方式：
	new Blob(array,options)
如：new Blob(['<a>ssss</a>'],{type:"text/html"})
返回的数据:Blob(25){size: 25, type: "text/html"}

可以使用 FileReader 借口从 blob 读取数据，也可以使用 URL.createObjectURL() 从 blob 创建一个新的 URL 对象
~~~

