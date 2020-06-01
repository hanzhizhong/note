### 秒转时分秒

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
window.cancelFrameAnimation(this.stopID) 就会停止动画
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
