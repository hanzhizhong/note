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



## 	二级标题

### 链接

#### 图片

![image-20191126094642323](assets/image-20191126094642323.png)

#### 超链接

[百度][https://www.baidu.com]

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

## 生成项目的目录结构图方法

+ cd 到当前的项目根目录下
+ tree /f > 文件名.txt

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


~~~

### nodejs koa框架 插件

~~~css
koa
koa-static
koa-router 
//router=require('koa-router'();
//app.use(router.routes());启动路由
//app.use(router.allowedMethods());allowedMethods处理的业务是当所有路由中间件执行完成之后,若ctx.status为空或者404的时候,丰富response对象的header头.

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

~~~

### electron 插件

~~~css
electron-rebuild //编译原生的模块 
robot.js//智能自动化 控制鼠标、键盘、阅读屏幕
vkey //根据键值找对应的键盘字母
~~~

### meta 键

~~~css
如果使用的是windows，那么meta键表示的就是键盘上的win键（及键盘上有窗口图案的键）

如果使用的是苹果电脑，meta键表示的是Cmd键；
~~~

