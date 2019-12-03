### child-process

>核心模块的名称为： child_process 不是child-process
>
>~~~javascript
>//常用的方法
>exec('shell 命令',(err,stdout,stderr)=>{})
>//还有三种方法，需要查资料
>~~~
>

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

