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

#### react脚手架配置代理

##### 方法一

~~~css
在package.json的文件中添加：
proxy:"http://localhost:5000"//数据的接口地址

配置简单
不能配置多个代理
请求不存在的3000端口时，会转发到5000的端口中
~~~

##### 方法二

~~~js
创建配置文件 文件名：setupProxy.js 
const proxy=require('http-proxy-middleware')
module.exports=function(app){
    app.use(
    	proxy(
        	'^/api',{
                target:"http://localhost:5000",
                pathRewrite:'',
                changeOrigin:true/false
                //为true时服务器收到的请求头为：localhost:5000的
                //为false时服务器收到的请求头为:localhost:3000
            }
        ),
        proxy()
    )
}

上面一版已经过时了
const {createProxyMiddleware} =require('http-proxy-middleware')
module.exports=function(app){
    app.use(
    	createProxyMiddleware(
        	'^/api',{
                target:"http://localhost:5000",
                pathRewrite:'',
                changeOrigin:true/false
                //为true时服务器收到的请求头为：localhost:5000的
                //为false时服务器收到的请求头为:localhost:3000
            }
        ),
        createProxyMiddleware()
    )
}
~~~

#### React脚手架

##### 创建

~~~css
create-react-app node>10

不使用 yard包管理器 使用npm的方式
create-react-app 创建的项目名称  --use-npm

~~~

##### 事件发布与订阅机制

~~~css
pubsub-js //和vue 用的是同一个插件
import PubSub from 'pubsub-js'
PubSub.subscribe('事件名',回调函数(msg,index)) 
PubSub.publish('事件名',传入的参数)
~~~

#### 路由 react-router-dom 库

##### key value的形式组成

~~~css
value是component，用于展示页面

注册路由
<Route path="/test" component={}/>
~~~

##### react-router-dom 的内置组件

~~~css
<BrowserRouter>
<HashRouter>
<Route>
<Redirect>
<Link>
<NavLink>
<Switch>
~~~

##### 其他的属性包含在this.props中

~~~css
history
match
withRouter
~~~

##### 路由中报错的问题

~~~css
1.You should not use <Route component> and ...
报这种错误的原因是：<Route></Route>使用了这种路由并且中间有空格
~~~

##### BrowserRouter 和HashRouter的区别

~~~css
1.BrowserRouter使用的是history的api，而hashrouter使用的是URL的哈希值
2.path的表现形式不一样：hashrouter包含#,browserrouter不包含
3.页面刷新后对state的影响：browserrouter页面刷新后state不会消失,而hashrouter会将state的记录消除
4.
~~~

##### redux和react-redux是配合使用的，当然redux是可以单独使用的

~~~css
1.import {createStore,applyMiddleware,combineReducers} from 'redux'
2.导入reducer方法
3.如果有异步的actions需要使用 redux-thunk插件
4.如果需要使用redux的调试工具 需要插件 redux-devtools-extension插件
import {composeWithDevTools} from 'redux-devtools-extension'

const allReducers=combineReducers({
    key:value....
})

export default createStore(allReducers,composeWithDevTools(applyMiddleware(thunk)))

~~~

#### 类组件中 方法的this问题

~~~css
add(){
    this=>undefined  解决方法是：在初始化时 constructor(){
        									this.add=this.add.bind(this)
    									}
}
add=()=>{
    this=> 当前类的实例对象
}
~~~

#### 组件懒加载lazy报错的问题

~~~css
需要配合 Suspense方法 fallback={} 属性不可缺失 不然会报错"A React component suspended while rendering, but no fallback UI was specified."
const Home =lazy(()=>import('./pages/...'))

将组件包含在 Suspense 标签中
~~~

#### 消息提供者和消费者 ｛Provider,Consumer｝=React.createContext()

~~~js
在类组件中使用的是 React.createContext()
在函数时组件中使用的是 React.useContext()

Provider\Consumer是当成标签使用的
但是在Consumer的标签中放置的是函数
~~~

#### 组件优化 => PureComponent

~~~css
只要执行setState，不管数据有没有更新，render()函数都会重新触发
父组件只要重新render，子组件就会重新render,即使子组件没有用到父组件的任何数据

产生上述的原因是：Component中的shouldComponentUpdate()总是返回true
解决方法是：比较新旧的state和props


~~~

