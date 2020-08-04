### nativeImage 是NativeImage的实例

> + 使用的方法
>   + const {nativeImage}=require('electron')
>   + ![1575296953208](assets/1575296953208.png)
>   + 使用

### Tray的使用注意事项

> + 在入口文件中可以使用全局变量的方法 let tray=null;
>   +  **解决tray显示一会就消失的问题**这个问题，似乎很诡异。其实就是你的写法问题，非全局变量会被定期回收。你的`tray`肯定是个函数内部的局部变量，而不是最顶层的全局变量。这就会导致，你的托盘被系统当成垃圾回收啦。就这么回事，所以把你的变量定义，提升到顶层即可。 
>   + ![image-20191203094117151](assets/image-20191203094117151.png)
> + 或者在优雅加载页面的方式下面加载tray
>   + ![image-20191203094143125](assets/image-20191203094143125.png)
> + 不知道拿错了 **现在解决了: 就是tray中要使用绝对路径**
>   +  原因在 tray的图标应该设置成绝对路径，不要用相对路径。 
>   + ![image-20191203103004478](assets/image-20191203103004478.png)

### require is not defined

> + ![image-20191203102156615](assets/image-20191203102156615.png)
>   + 错误原因是浏览器不是node的集成环境，不认识require关键字

### path.join 和path.resolve的区别

~~~javascript
path.join(__dirname,'路径名次')//路径名称是相对路径的 返回的就是相对路径，如果是绝对路径的那就返回绝对路径
path.resolve(__dirname,'路径名称')//不管路径名次是什么返回的都是绝对路径
~~~

### user-select:none;input\textarea中的还是可以选中的

> 禁止用户按下鼠标左键是选中文字的样式
>
> ![image-20191210154315953](assets/image-20191210154315953.png)

### cursor: 

> 改变鼠标指针的样式
>
> ![image-20191210154910192](assets/image-20191210154910192.png)
>
> **使用自定义的鼠标样式**
>
> cursor:url('./light.png'), default; 一定要有default  图片的格式png\gif \jpg

### frame | transparent 在windows中不要一起用

> 原因是： win.isMaximize()  方法返回的结果都是false

### electron 使用import 模块化时报错的原因

+ [ 查看链接 ][http://www.itkeyword.com/doc/6155234927226421x791/es6-syntax-import-electron-require]
+ 需要使用第三方的模块

![image-20191212100411642](assets/image-20191212100411642.png)

### This is probably not a problem with npm. 

~~~javascript	
//解决方法
rm -rf node_modules
rm -rf package-lock.json
npm cache clear --force
npm install
~~~

### nsis 配置

~~~javascript
"build": {

    "productName":"xxxx",//项目名 这也是生成的exe文件的前缀名

    "appId": "com.leon.xxxxx",//包名  

    "copyright":"xxxx",//版权  信息

    "directories": { // 输出文件夹

      		"output": "build"

    	}, 

    "nsis": {

      "oneClick": false, // 是否一键安装

      "allowElevation": true, // 允许请求提升。 如果为false，则用户必须使用提升的权限重新启动安装程序。

      "allowToChangeInstallationDirectory": true, // 允许修改安装目录

      "installerIcon": "./build/icons/aaa.ico",// 安装图标

      "uninstallerIcon": "./build/icons/bbb.ico",//卸载图标

      "installerHeaderIcon": "./build/icons/aaa.ico", // 安装时头部图标

      "createDesktopShortcut": true, // 创建桌面图标

      "createStartMenuShortcut": true,// 创建开始菜单图标

      "shortcutName": "xxxx", // 图标名称

      "include": "build/script/installer.nsh", // 包含的自定义nsis脚本

    },

    "files": [

		      "dist/electron/**/*"
	
	    ],
    "win": {

     	 "icon": "build/icons/aims.ico",

	      "target": [
	
		        {
		
			          "target": "nsis",
			
			          "arch": [
			
			            "ia32",//32位的 
                          "x64"//64位的
			
			          ]
		
		        }
		
	      ]

    },

  }
~~~

### sqlite3 和electron-vue的结合使用方法

~~~javascript	
1.npm install gyp -S
2.npm install node-pre-gyp -S
3.npm install sqlite3 -S
4.//在package.json中添加 
	"postinstall":"install-app-deps"
5.npm run postinstall

还有一个插件 better-sqlite3说是比sqlite3好用

~~~

### tray托盘的使用时一定要定义全局变量 

~~~ 
let tray=null;
tray=new Tray()

~~~



### 视频音频的MIME类型的书写方法

~~~css
从根本上讲，您可以使用简单的MIME类型（例如video/mp4或）来指定媒体文件的类型audio/mpeg。但是，许多媒体类型（尤其是支持视频轨道的媒体类型）都可以从更精确地描述其中的数据格式的能力中受益。例如，仅以MIME类型在MPEG-4文件中描述视频，video/mp4并没有说明实际媒体所采用的格式。但是，每种MIME类型都是模糊的。所有这些文件类型都支持多种编解码器，并且这些编解码器可以具有任意数量的配置文件，级别和其他配置因素。因此，您可以将codecs参数添加到媒体类型。

因此，codecs可以将参数添加到描述媒体内容的MIME类型中。有了它，可以提供特定于容器的信息。此信息可能包括诸如视频编解码器的配置文件，用于音轨的类型等内容。
use-methods:
请在前面;加上分号（）codecs=，然后再加上描述文件内容格式的字符串。某些媒体类型仅允许您指定要使用的编解码器的名称，而其他媒体类型还允许您对这些编解码器指定各种约束。您可以使用逗号分隔多个编解码器。

audio/ogg; codecs=vorbis
包含Vorbis音轨的Ogg文件。

video/webm; codecs="vp8, vorbis"
video/webm;codecs="h264"//使用的方法为{mimeType:"video/webm;codecs=‘h264’"}
甲WebM的含有文件VP8视频和/或Vorbis的音频。

video/mp4; codecs="avc1.4d002a"
一个MPEG-4文件包含AVC（H.264）的视频，主轮廓，级别4.2。

与任何MIME类型参数一样，如果编解码器的任何属性使用特殊字符，则必须将codecs其更改为codecs*（请注意星号字符*），这些特殊字符必须按照RFC 2231第4节“ MIME参数值和编码”进行百分比编码词扩展。您可以使用JavaScript encodeURI()函数对参数列表进行编码。同样，您可以decodeURI()用来解码以前编码的参数列表。
~~~

### 第三方库的使用方法问题

+ robotjs 控制鼠标和键盘  使用时是需要编译的

  ~~~css
  1.window 环境下
  	npm install --global --production windows-build-tools 最好是以管理员运行cmd
  	npm install -g node-gyp
  	node-gyp rebuild
	npm install robotjs -S
  	npm install electron-rebuild -D //当前的项目下
  	npx electron-rebuild 自动编译
  ~~~
  
+ 上面是一种方法还有一种方法

  ~~~css
  卸载掉所有的 npm uninstall -g windows-build-tools
  npm uninstall -g node-gyp 
  在c://user/appdata/roming/ 删除 npm 
  删除 目录下的.electron.gyp
  删除 .node-gyp
  删除 .window-build-tools 
  清理软件清一下垃圾
  重新启动电脑
  
  清理一下环境变量
  下载并安装python2.7的环境,环境变量中添加工作路基
  下载vs2017的安装工具
  安装2017的社区版 选则 工作负载为 c++的桌面开发
  
  npm config set --global python 环境变量的路径
  npm config set --global msvs_version 2017
  
  下面就可以下载c++等库了
  
  ~~~

  

### FFmpeg的使用方法

~~~css
"ffplay播放没有声音SDL_OpenAudio (2 channels, 44100 Hz): WASAPI can't initialize audio client" 出现这种错误的解决方法

1.打开系统环境变量
2.点击新建环境变量
3.‘变量名’：=》SDL_AUDIODRIVER
4.‘变量值’:=》 directsound或winmm

将webm转成MP4的方法
ffmpeg -i default.webm -vcodec copy default.mp4
~~~

### electron-builder安装和使用方法

~~~css
npm install -g --production windows-build-tools (用管理员启动cmd安装，windows中必备)
1.npm install electron-builder -D 
2.npm install cross-env -D //兼容不同平台的设置的环境变量
在package.json中设置	cross-env NPM_CONFIG_ELECTRON_MIRROR='http://npm.taobao.org/mirrors/electron/' electron-builder build --mac
	cross-env NPM_CONFIG_ELECTRON_MIRROR='http://npm.taobao.org/mirrors/electron/' electron-builder build --win --ia32
3.如果先前下载了electron-rebuild 的插件需要先卸载它，因为 electron-builder 有c++ c 语言等编译插件 只要在package.json中加入 postinstall:钩子函数命令： “electron-builder install-app-deps”

这是在基于electron-builder 打包的squirrel包 做更新时的需要的插件
npm install electron-builder-squirrel-windows -D 

**快速解决安装错误重新安装的方法
npm install [name] --registry=https://registry.npm.taobao.org
~~~

#### electron-builder的公共配置项

~~~css
appId:'',应用名称 格式：‘com.example.app’
productName:'' 应用的名称
copyright:'' 版权信息
asar:true/false 是否使用asar加密
files:'',最终的需要打包的文件路径
directories:
extraFiles:[],在files中的需要打包的文件中有不要打包的文件路径名
~~~

### 软件的更新

~~~css
S3(simple storage service) 简单存储服务
	S3理论上是一个全球存储区域网络 (SAN)，它表现为一个超大的硬盘，您可以在其中存储和检索数字资产
~~~



#### Mac

~~~css 
搭建的服务器中需要返回的数据：
1.有更新时返回的数据格式
{
    url:'软件包的地址',
    name:'这次发布的名名字如：1.1.0或者1.0.1'，
    'notes':‘这次发布的文案’，
    pub_date:'发布的时间'
}
没有更新时返回
status 204 
~~~

#### window

~~~css
搭建的服务器返回的数据内容：
1.路由 feedURL/RELEASES
2.有更新返回RELEASES文件内容（打包时出现的 现在我打包会出错，在squirrel文件夹中没有这个文件），如
‘BBC6F98A5CD32C675AAB6737A5F67176248B900C Mercurius-1.0.1-full.nupkg 62177782’
3.redirect()重新定向到静态文件服务 中
	req.redirect('/pbulic/.....nupkg')
https://github.com/electron-userland/electron-builder/issues/359#issuecomment-21485113
0
~~~

### 崩溃日志 crash

~~~css 
electron 的奔溃日志是 dump格式的 它不可读 （mac 和 window都是dump）
需要搭建崩溃服务器

收集到crash报告后 需要两个库 socorror 和 mini-breakpad-server 
结合 electron symbol来做处理


~~~



### 集成C++能力

~~~css
N-API:nodejs 的一部分，独立于runtime v8,就是对同一个ABI无需重新编译
以C的风格来提供稳定的ABI接口
（ABI:应用程序二进制接口，描述了应用程序和操作系统之间，一个应用和它的库之间，或者应用的组成部分之间的低接口）
（API:应用程序接口又叫应用编程接口）
	本身是基于C的API
	c++封装 node-addon-api


windows-build-tools 是构建原生模块必备的工具
node-gyp 创建项目的生成工具，解决来跨平台的一些问题
gyp generate your project

编写N-API
bindings 是用来帮我们去查找路径用的
N-API本质上是一个C的api 如果要用C++的话，要引入 node-addon-api
*** 在 package.json中 增加 “gyp”:true ,别人在安装你的模块的时候会自动编译

执行编译

npx node-gyp rebuild --arch=x64 
npx node-gyp rebuild --arch=i32
~~~

#### 集成动态链接库 (dll)

~~~css 
node-ffi 是一个javascript 加载和调用动态库的nodejs的扩展，他可以让我们在不编写任何c++代码的情况下创建于本地dll库的绑定，同时还负责javascript和C的类型转换

和 node-addon-api相比 优点 
	不需要源代码
	不需要每次都重新编译
	不需要写c的代码，只需要一定的c的了解即可
缺点：
	黑盒调用，调试困难


mac 中有内置的脚本 applescript 
直接通过 node_applescript 这个库然后去集成
	1.npm install -S applescript 

const applescript=require('applescript')
const script='tell application "WeChat" to activate end'

applescript.execString(script,(err,res)=>{
    if(err){
        console.log(err);
        return 
    }
    console.log(res)
})

ffi-napi 第三方集成c++能力
安装完成后需要到node_modules的改模块下执行 node-gyp rebuild

~~~



### 在App 上挂载全局方法

~~~css 
app.fn=require('....').init()

console.log(app.fn)
~~~

### electron-builder 

~~~css
1.npm install --global --production windows-build-tools
2.npm install -g node-gyp 
3.注意在package.json 脚本中 
sciprt:{
    "pack:win":"electron-builder --win --ia32"
}
build:{
    win:{
        target:[nsis] //不需要再写 squirrel包了，自动更新使用 electron-updater第三方模块
    }
}
~~~

### electron-updater

~~~css
事件
checking-for-update  audoUpdater.checkForUpdate()触发
update-available 当有可用更新时发出
update-downloaded 下载完成
download-progress 下载进度

方法
autoUpdater.downloadUpdate()发起更新，开始下载
autoUpdater.quitAndInstall()
autoUpdater.checkForUpdate()
audoUpdater.setFeedURL()
~~~

### electron 中的global顶级全局变量

~~~css
在主进程中 global.setWinId={curWin:win.id}
在渲染进程中 使用remote.getGlobal('setWinId').curWin
~~~

### 模块

~~~css
electron-notification-state检测是否允许发送通知
~~~

### 知识点

#### 任务栏

~~~css
闪烁提示
win=new BrowserWindow()
win.on('focus',()=>{win.flashFrame(false)})
win.flashFrame(true)

叠加型的任务栏
win=new BrowserWindow()
win.setOverlayIcon()

//缩略图工具栏
win.setThumbarButtons([])
win.setThumbarButtons()

弹出列表 弹出列表 弹出列表弹出列表 弹出列表 
弹出列表
app.setUserTasks()
~~~

#### 快捷键

~~~css
本地快捷键 必须指定 accelerator 2.app必须是处于焦点状态
cosnt {Menu,MenuItem}=require('electron')
const menu=new Menu()

menu.append(new MenuItem({
    label:"Print",
        accelerator:"CmdOrCtrl+P",
        click:()=>{console.log('time to print ')}
}))

全局快捷键 globalShortcut 
const {app,globalShortcut}=require('electron')
app.whenReady().then(()=>{
    globalShortcut.register('CommandOrControl+X',()=>{
        console.log('CommandOrControl+X is pressed')
    })
})

浏览器窗口中的快捷键
window.addEventListener('keyup',()=>{},true)
注意第三个参数 true，这意味着当前监听器总是在其他监听器之前接收按键，以避免其它监听器调用 stopPropagation()

mousetrap() 库 高级的按键检测库
~~~

#### 离线或者在线 online and offline

~~~css
使用 html5自带的api 
navigator.onLine =>true/false
~~~

#### 原生文件拖放事件

~~~css
在渲染进程中接收 ondragstart事件，并发消息给主进程
document.querySelector('#drag').ondragstart=(e)=>{
    e.preventDefault()
    ipcRenderer.send('drag-start','....path路径')
}
~~~

#### 在electron中嵌入第三方的web内容

~~~css
<iframe> 和常规的浏览器中的iframe一样
<webview>是一个自定义元素（<webview>），仅在Electron内部起作用，加载和与第三方内容进行通信以及处理各种事件方面提供了更大的控制权
<BrowserViews>它们是在主流程中创建并由其控制的，它们只是现有窗口之上的另一层Web内容，这意味着它们与您自己的BrowserWindow内容完全分开，并且它们的位置不受DOM或CSS的控制，而是通过在主过程中设置界限来控制的

~~~

#### ws/wss的区别

~~~css
websocket(ws)是html5的新协议。浏览器和服务器的全双工通信
http的url使用"http://或者https://"开头；而websocket使用"ws://"开头
wss是ws的加密版本
~~~

#### 安全

~~~css
仅加载安全内容 https wss ftps
不为远程内容启用Node.js集成
~~~

#### 部分不明插件的作用介绍

~~~css
Widevine CDM 是google推出的一种DRM，支持从google指定的服务器上，下载经google加密的版权文件，如视频、应用等。
Pepper flash
~~~

#### 术语

~~~css
asar (atom shell archive format)atom shell归档格式 ,一个asar就是一个简单的tar包
ASAR格式的创建主要是为了提高Windows上的性能

crt:CRT
c运行时库

dmg:DMG
指macOS上使用的苹果系统的磁盘镜像打包格式

ime:IME
输入法编辑器

idl:IDL
接口描述语言，编写函数签名和数据类型的格式，用于生成java,c++,javascript等接口

ipc:IPC
进程间通信。主进程和渲染进程之间发送序列化的json数据

mas:MAS
苹果的Mac应用程序商店的首字母缩写

mojo:MOJO
一种用于进程内部或进程间通信的ipc系统

native modules
原生模块(nodejs中叫做addons) 是一些使用 C or C++ 编写的能够在 Node.js 中加载或者在 Electron 中使用 require() 方法来加载的模块，它使用起来就如同 Node.js 的模块。 它主要用于桥接在 JavaScript 上运行 Node.js 和 C/C++ 的库

nsis:NSIS
是一个微软 Windows 平台上的脚本驱动的安装制作工具

osr:OSR
OSR(屏幕外渲染)可以用于在后台加载重载页面，然后在后台显示它(这样会快得多)。它允许你渲染页面而不显示在屏幕上。

userland 用户层/用户空间
Userland 让用户能够创造和分享一些工具来提额外的功能在这个能够使用的 "core（核心）"之上。

webview
webview标签用于集成guest内容(如外部网页)在你的electron应用内。它们类似于iframe,但是不同的是每个webview运行在独立的进程中。作为页面它拥有不一样的权限并且所有的嵌入内容和你应用之间的交互都将是异步的。这将保证你的应用对于嵌入的内容的安全性
~~~

#### 命令行开关

~~~css
检查主进程
electron --inspect=5858 【主进程的入口文件】

或者在package.json中
"dev":"electron --inspect=5858 ."

app.commandLine.append
~~~

#### webContents

~~~css
调用event.preventDefault()事件，可以阻止Electron自动创建新的BrowserWindow实例.调用event.preventDefault()事件后，你还可以手动创建新的BrowserWindow实例,不过接下来你必须使用event.newGuest=win(手动创建的BrowserWindow实例)方法引用BrowserWindow.如果不这样做会产生异常
~~~

#### webPreferences中的启用项说明

~~~css
nodeIntegration:true	启用nodejs集成功能
nativeWindowOpen:true 	允许启用window.open(url,framename,features)
features 类型为字符串 字符串遵循标准浏览器的格式 如：'width=300 height=200 modal=true'
enableRemoteModule:true		允许启用remote模块
~~~

####  `NSUserActivity`  

~~~css
Handoff的基本思想就是：用户在一个应用里所做的任何操作都包含着一个activity，一个activity可以和一个特定用户的多台设备关联起来。用行话来说，抽象出这种activity的类叫做NSUserActivity，大部分时间我们都会和这个类打交道。
~~~

#### 硬件加速

~~~css
硬件加速是指：利用硬件模块来替代软件算法以充分利用硬件所固有的快速特性

app.disableHardwareAcceleration() 禁用硬件加速
只能在ready()之前调用

~~~



### UML 统一建模语言

#### 示例1

~~~css
骰子游戏：投掷两个骰子，如果总点数为7则赢得游戏
过程：定义用例=》定义领域模型=》定义交互图=》定义设计类图
定义用例：需求分析的一种工具，是一些情节描述
	1.游戏者请求骰子
	2.系统展示结果：总点数为7，游戏者赢，否则输
定义领域模型：OOA。识别问题中的概念，它是对真实世界的描述和想象可视化，与具体实现的软件技术无关（c++、c）
	游戏者
	骰子
	骰子游戏

~~~

#### 标准定义

~~~css
统一建模语言是描述、构造、和文档化系统制品的可视化语言
UML是一个庞大的图形化表示法体系
三种应用方式：
	草图
	蓝图
	编程语音
学习要素：
	表示法
	过程
	工具

什么是UP:软件开发过程描述了构造、部署以及软件维护的方式。统一过程是一种流行的构造面向对象系统迭代软件开发过长。RUP是对统一过程的详细精华，并且被广泛采纳
~~~

#### 使用的方法

~~~css
0...*
1..* 一对多
1  一对一
m..n多对多

空白箭头为继承 箭头指向父级
封闭的菱形箭头  组合关系 不能脱离整体存在 箭头指向父组合 商场和厕所的关系
没有箭头的折现表示 关系 类之间没有依赖性，只是基本的关联关系

没有封闭的菱形箭头 聚合关系 说明整体和部分的关系 箭头指向整体 部分可以脱离整体  乌龟和乌龟群的关系

在属性和方法前面加上+/-是公有或私有
#只能由相同的类或者其子类存取

~~~



