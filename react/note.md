#### @babel/standalone的作用

~~~css
1.在浏览器中调用babel API将 ES6转成ES5
2.当babel发现 <script type="text/babel"></script>时会自动将script中的内容转成javascript
~~~

#### jsx语法规则

~~~css
{}中是接表达式不是表示语句 表示语句=》的含义是if\while\for等
表达式是有返回值


函数时组件中的this 指向undefined
~~~



#### 事件处理

~~~css
1.使用驼峰式命名
2.必须绑定一个函数 如：onClick={handleClick}
3.绑定的函数一定不能是字符串
~~~

#### 类组件

~~~css
1.constructor(){}一定要有 super()；this.state={} //state相当于vue中的data
2.修改state中的属性或对象时：this.setState({
    
})
~~~

