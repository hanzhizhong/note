















### 低功耗蓝牙和普通蓝牙的区别

~~~css
1.传统蓝牙4.0向下兼容，BLE4.0不向下兼容
2.BLE优点：快速搜索，快速链接，超低功耗保持链接和传输数据，弱点:数据传输速率低，物理宽带只有1M,实际在1~6KB之间
~~~

#### 五大区别

~~~css
1.低功耗蓝牙会以最快的速度完成发送和传输任务，完成后低功耗蓝牙会暂停发射无线（但是还是会接受），等待下一次的连接激活，传统蓝牙保持持续连接
2.低功耗的蓝牙广播信道(为保证网络互不干扰而划分)仅有3个，传统蓝牙有32个
3.低功耗蓝牙完成一次连接需要3ms（扫描其他设备，建立链路，发送数据，认证和适当时结束）;传统蓝牙完成一次相同的连接周期需要数百毫秒
4.低功耗的蓝牙使用非常短的数据包，多用于实时性要求比较高，但是数据传输率比较低的产品，如遥控类的键盘、鼠标，传感设备的数据发生，如心跳带，血压针，温度传感器等；传统蓝牙使用的数据包比较长，用于数据量比较到的传输，如语音，音乐，较高的数据量传输等
5.低功耗蓝牙无功率级别，一般发送功率在+4dBm,一般在空旷距离，达到70m的传输距离；传统蓝牙有3个级别，class1,class2,class3,分别支持100m,10m,1m的传输
~~~



### uni.request()

~~~javascript
msg:"Invalid request" 
可能的问题是：在请求参数中加入header:{"Content-type":"application/x-www-form-urlencoded"}
~~~

### 项目目录

~~~css 
components:组件目录
hybird：存放本地网页的目录
platforms ：各个平台下专用页面
pages：业务代码的页面
static 存放应用引用的静态资源（图片、视频）
wxcomponents ：小程序的组件
main.js：Vue初始化的入口文件
App.vue ：应用配置，用来配置app全局样式和监听应用的生命周期
manifest：应用的配置项，如应用的名称、appid、logo、打包发布的版本信息
page.json 配置页面的路由、导航、选项卡等页面类信息 配置页面的路由、导航、选项卡等信息 

static 下的js文件不会被编译 
css/less/sass/scss 等资源不要放在static下，创建common文件夹，存放通用的文件css/less/scss 
~~~

### 各个平台的名称

~~~css
app-plus App 
h5 H5 
mp-weixin
mp-alipay mp-alipay alipay 
mp-baidu 

~~~

### template内引入静态资源 image/video 等标签的 src的路径

~~~css 
1.绝对路径
<image src="@/static/logo.png">
<image src="/static/logo.png">
2.相对路径
<image src="../../static/logo.png">

@开头的相对路径或者绝对路径会经过 base64的转换规则校验 
@开头的相对路径或者绝对路径会经过 base64的转换规则校验 
~~~

### 应用的生命周期

~~~css 
onLaunch()
onShow()
onHide()
onError()
onUniNviewMessage
~~~

### 页面中的生命周期

~~~css 
onLoad()
onReady()
onShow()
onHide()
onUnLoad()
onPullDownRefresh()
onReachBottom()
onTabItemTap()
onShowAppMessage()
onPageScroll()
onNavigationBarButtonTap()
onBackPress()
onBackPress()
onNavigationBarSearchInputChanged()
onNavigationBarSearchInputConfirmed()
onNavigationBarSearchInputClicked()

~~~

### uni-app的页面路由

~~~css
页面路由为框架 统一管理 
在 page.json统一注册管理 和vue中的vue-router不同

uni-app路由跳转的两种方式
标签组件跳转
	<navigator open-type="navigateTo"></navigator>
	open-type
		navigateTo 
		redirectTo 

Api
	uni.navigateTo()
	uni.redirectTo()
	uni.reLaunch()
	uni.switchTab()
	uni.navigateBack()
	
~~~

~~~css
uni-app是通过 process.env.NODE_ENV 判断是生产环境还是开发环境 

点击运行是 开发环境
点击发行 是 生产环境
~~~

### 条件编译

~~~css 
#ifdef 平台标识 #endif
#ifdef 平台标识 #endif
#ifdef 平台标识 #endif 
平台标识：H5 、APP-PLUS、MP-WEIXIN 

条件编译在不同平台下打包编译出不同的平台代码 

运行期判断
uni.getSystemInfoSync()
uni.getSystemInfoSync()
uni.getSystemInfoSync()
~~~

### 尺寸单位

~~~css 
750是uni-app的屏幕基准宽度 
uni-app屏幕基准宽度 750 

尺寸转换的公式：
uniapp的基准750* 当前元素的设计尺寸/设计稿的设计宽度尺寸
750*100/750 
750*100/640 

~~~

### 样式导入

~~~css 
使用@import 导入外联样式表 
在uni-app中不能使用 * 通配符
~~~

### 事件修饰符会有平台的兼容问题

~~~css 
stop 全端兼容
prevent H5端 其他不兼容

select 标签使用picker替代

~~~

### uniapp的全局变量的使用方法

~~~css
推荐两种常用的方法 
	1.Vue.prototype.[name]=...
	2.Vuex 状态管理模式
	3.文件的导入模式

~~~

### 注意开发事项

~~~css
1.h5正常App异常
    不支持的选择器
    不适用*通配符
    body元素选择器改为 page 
    div ul li 改为view 
    img 改为image 
    font 改为text 
    a 改为 navigate 
2.h5正常但是小程序异常
	v-html在小程序中不支持
	小程序要求链接的网址都要配白名单
3.小程序或APP端正常，但是h5端异常
	App端使用了App特有的API和功能 如：plus\native

4.区别于传统的web开发
	a.js注意
		非h5端，不能使用浏览器自带的对象 如window,document,cookie,
	b.tag标签使用注意
		和html的tag有区别
5.css注意：
	单位方面：现在uni-app默认为rpx 已经将upx修改了，统一了为rpx 

~~~

### iBeacon 是苹果公司在移动设备上配置的新功能

~~~css 
配置iBeacon模块的话，便可让iphone和ipad上运行的一些资讯告知服务器，或者由服务器向iphone和ipad客户发送折扣券及进店积分
~~~

### NFC 近距离无线通信

~~~css 
nfc的主要功能是智能手机开启nfc功能后，可以将手机变成公交卡，可以在下充值公交卡、读取卡内数据

目前的应用范围：公交、支付、还有接触通过浏览、链接等功能
~~~

### 解决console.log() 在真机环境下不显示的问题



~~~css
1、重新开启USB调试模式，或者重启手机后重试；

2、确认你的手机日志级别：进入手机的工程模式（不同手机进入工程模式的指令可能不同），选择日志输出等级，确定Log print enable 为 Enable，如果不是，设置为enable。设置完再次运行即可。
	我的乐视真机 1.【开发者选项】在调大了 日志的记录器缓存区的大小，
				2.【开发者选项】 启用WLAN详细日志记录功能  true

~~~

### 开发中的注意项

#### uni.request

~~~css
url:使用ipv4的地址，用127.0.0.1或localhost有时会报错 request:fail abort statusCode:-1
sslVerify:false 不验证 ssl 安全协议层 验证 ssl 证书 （app端）
~~~

