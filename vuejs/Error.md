### Vue-component

> ~~~javascript
> <item-li :options="optionObj"></item-li>
> 
> props:{
> 	options:Object;
> }
> /*
> 在使用props传递参数值时，在父组件中的属性命名方法为optionObj{title:'aaa',statement:'bbbb'},
> 不能使用optionObj.title='aaaa',optionObj.statement="bbbb"的方法
> 因为不能获取到任何数据
> */
> ~~~
>
> ![image-20191126140635514](assets/image-20191126140635514.png)
>
> ![image-20191126140556850](assets/image-20191126140556850.png)

#### [Vue warn]: Unknown custom element

> ![image-20191126152343142](assets/image-20191126152343142.png)

#### Component template should contain exactly one root element.

~~~css
解决的方式是 template中使用一个大标签,包裹住所有的子标签
~~~

#### Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders

~~~css
大概的意思是：通过props传递给子组件的 属性值，不能在子组件内部修改props中的 属性值。
需要通过 this.$emit的方式 事件触发机制，让父组件自己改变 属性值
~~~



### filters 

#### Error in render: "TypeError: Cannot read property

+ 过滤器中的this不是指向的vm实例对象的，所以会在页面中报告如下的错误
  + ![image-20191129112718596](assets/image-20191129112718596.png)
  + 解决的方法是将this.属性名定义为全局的变量, 在filters中不用使用this关键字

### keep-alive

> **主要用于组件状态保存或者避免重复渲染**
>
> > exclude 不缓存 || include 缓存项
>
> ~~~html
> /*exclude是指不需要缓存的组件名*/ a和b是在每个组件中的命名名称 ：如图
> <keep-alive :exclude="['a','b']"></keep-alive> 数组中的为字符串形式
> <keep-alive :exclude="/a|b/"></keep-alive> 配合正则
> <keep-alive exclude="a,b"></keep-alive> 不是用属性绑定的形式 注意直接写组件名
> ~~~
>
> ![image-20191129154436591](assets/image-20191129154436591.png)

### vue-router 中不能多次点击同一个router-link 的问题

+  点击同一个路由，页面报错 

![image-20191211165945596](assets/image-20191211165945596.png)

+ 解决的方法 在后面加上catch(err=>{err})

  ![image-20191211170146197](assets/image-20191211170146197.png)

+ 嵌套路由的默认页面设置

  ![image-20191211175625648](assets/image-20191211175625648.png)

### 导入图片路径时出现错误：静态资源找不到

![image-20191216170320434](assets/image-20191216170320434.png)

+ 解决方法为：使用import defaultImagePath from '...../.../' (相对或者绝对路径)

### Unexpected token u in JSON at position 0

+  其实很简单，这个错误是**由于JSON.parse解析了undefined**。 

### Failed to resolve directive: mode

+  未能解析指令：mode   => 有未识别的v-model  即有可能是将model单词平措了

### filter 过滤器中转换html标签

~~~javascript	
<td v-html="$options.filters.turn2CurrentWord(item.isCurrent)"></td>
Vue.filter('turn2CurrentWord',(val)=>{
    return '<span>ddd</span>'
})
~~~

###  Prop "allprogram" is passed to component  

+ 错误原因是 prop传值时的命名方法错误，如果传入的是allProgram="allProgram"就会报错，使用下面的方法就不会报错
+ ![image-20200108165705928](assets/image-20200108165705928.png)

### 单击和双击的时间间隔设定为200ms

### Duplicate keys detected: '0'.

> 错误原因是在使用v-for的时候，都要加上一个唯一的key值，但是在这里写了两个for循环，尽管都加上了key值，但是又将key值写成一样的了，所以就会导致报错
>
> 解决方法
>
> + 修改其中的绑定的key值
>
>   ~~~html
>   <div class="info" v-for="(item,index) in itemList" :key="'info1'+index"></div>
>   <div class="info" v-for="(item,index) in itemList" :key="'info2'+index"></div>
>   ~~~
>
>   
>
>   

###  在radio输入框中操作v-model的注意事项

~~~css
1.相同name类型的input type=radio中 v-model绑定的数据属性名称 相同
2.注意 绑定的数据值是 字符串类型的 ，在遇到true 和false时是需要注意 值的类型转换，不然的话 在使用 v-if 对绑定的数据属性名称 进行判断时 是不会有 交互效果的
~~~

### vue-cli 

#### public和assets的区别

~~~css
public中的文件是不会经过webpack的打包处理的
assets 文件夹中的文件会经过webpack的打包处理

public中的资源在引用是 **一定要使用绝对路径**,不然会找不到，报错
使用方式为：img/filename.jpg      不需要加上public /public/img/filename.jpg 这是不对的
~~~

#### nextTick

~~~css
nextTick的回调函数，在下一次DOM更新后立即执行
即在数据初次发生变化后，在使用js的方法调用DOM中的值，得到的结果还是原来的初始值，不是更新后的值，需要二次点击才可以获取到变化的值，
因此 调用$nextTick() 函数 可以得到在 需要在下一次DOM更新后的新值
~~~

### Vue.js知识点

~~~css
new Vue({
    选择对象：data\props\propsData\computed\watch\methods
})
注意点：只有在实例对象创建时已经在data对象中的属性才是 响应式的 响应式 响应式

计算属性：是基于它们的响应式依赖进行缓存的 就是说data中的属性值变成才能引起计算属性的变化

class 和style 绑定
方式除了 有字符串外 还有 对象绑定 数组绑定的方式 
:class="{active:isActive,color-style:isGray}" 常用来启用动态的样式切换
对象中可以是多键值对的形式
动态的绑定的class 不仅仅是可以存放在内联样式内，还可以放在 data的对象中
:class="classObject"
data(){
    return {
        classObject:{
            active:true,
            'color-style':false
        }
    }
}

class 放在数组中的方式 [active,error]这需要 data对象的关联

:style样式绑定 
1.内联的对象方式绑定 :style="{color:data对象中的属性名}"
2.在data对象中直接使用 属性对象绑定 styleobject:{color:'',fontSize:'23px'}注意一定要使用 驼峰的命名方式
3.数组的绑定方式

~~~

#### 条件渲染

~~~css
使用template 元素当做一个不可见的包裹元素 v-if 条件渲染后浏览器不会显示template
注意点 v-if="loginType==='username'"
	  v-else
	这里的loginType 将不会清除 用户已经输入的内容，如果应为两个模板使用了相同的元素input 那么input 不会被替换掉，只是替换了placeholder的提示信息 用户的输入内容是一应的 如下图所示

解决方式： 这两个元素完全独立，不要复用它们
	在条件编译v-if的容器中在相同的元素标签中 加上 key属性 
	如：<input placeholder="your name please" key="username">
	   <input placeholder="your email" key="email">


~~~

![](assets/image-20200715102031563.png)

![image-20200715102052545](assets/image-20200715102052545.png)

~~~css
v-if 和v-show的区别是：
v-show是始终都会存在DOM的结构树中的，只是简单切换元素的display
~~~

#### 遍历渲染

~~~css
可以使用of 替代in
v-for="(item,index) in list"
v-for="(item,index) of list"

v-for 遍历对象 object:{name:'jane',title:'nothing seek,nothing find'}
v-for="(v,k) in object" :key=k

~~~

#### vue不能检测数组和对象的变化,回避方式有如下：

~~~css
对象：vue无法检测property 的添加或者移除
var vm=new Vue({
    data(){
        //data中没有someobject 属性 
    }
});

Vue.set(vm.someobject,'b',2) 就会有响应式的根级别的属性someobject 
或者
this.$set(this.someobject,'b',2)

对于已经存在的对象 添加新的属性时，使用Object.assign()方式
this.someObject=Object.assign({},this.someobject,{a:1,b:2})

数组不响应的地方有
	利用数组索引直接改变一个数组项 不响应
	修改数组的长度时 不响应 
对于第一个问题的解决方案 是使用 this.$set(items,'索引值',newVaule)
对于第二个问题的解决方案 是使用 this.items.splice(index,1)
~~~

#### 事件处理

~~~css
事件修饰符：
stop\prevent\once\capture\self\passive
<a :click.stop="doThis"></a>
<form :submit.prevent="onSubmit"></form>
once事件只会触发一次
capture:使用事件捕获模式，即内部触发的事件现在此处理，然后才交给内部元素处理
self:只有当event.target时当前的元素自身时才会触发处理函数，即内部元素不会触发

按键修饰符
enter\tab\delete\esc\space\up\down\left\right

系统修饰符
ctrl\alt\shift\meta

.exact 精确的系统修饰符
:click.exact="onClick" 没有其他的修饰符下，按下才能触发
:click.ctrl.exact="onCtrlClick"

鼠标修饰符
right
left
middle
~~~

#### 表单输入绑定

~~~css
使用v-model指令 创建双向数据绑定 语法糖
有效的标签元素 select input textarea

**v-model 会忽略 表单元素的的value checked selected 的初始值， 而是将vue实例中的根数据当做数据来源

text/textarea使用 value 绑定 触发 input事件
checkbox radio 使用checked 绑定 触发change事件
select 使用value绑定 触发change事件

** <textarea>{{text}}</textarea> 这是错误的解析模式
应该使用 v-model

v-model的修饰符
	v-model.lazy 数据不会立即双向改变，需要通过事件change事件之后触发

	v-model.number 数据转成number类型的
	v-model.trim 过滤掉收尾的空白字符

~~~

#### 注册组件

~~~css
组件名的命名规范：字母全小写且必须包含一个连字符
***重要的事情说三遍 组件名：字母全部小写，必须包含连字符
~~~

