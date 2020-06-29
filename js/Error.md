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

#### navigator.MediaStream.getUserDevices(constraints)

> 获取媒体设备的能力 ，constraints是约束媒体流的
>
> ~~~javascript
> let constraints={audio:true,video:{width:num,height:num}}
> 
> navigator.mediaDevices.getUserDevices(constraints) 返回的是Promise对象
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


### ECMA 语法

#### switch case 使用范围的方法

~~~css
<script>
    var x=10;
switch(true){
    case x>0&&x<10:   //条件必须分开书写
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





### ES6 数组方法

~~~javascript
arr.forEach((item,index,arr)=>{})
arr.map((item,index,arr)=>{ ...  return{}})//做映射改变原数据
arr.filter((item,index,arr)=>{ return ....}) //
arr.find((item,index,arr)=>{return })//返回最先符合条件的一个
arr.findIndex()
Array.from()
~~~

### ES6中的目前只有静态方法没有静态属性

~~~css
即只支持 static 方法（）{}
而类的静态属性
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
