### 秒转时分秒

~~~javascript
var itime=9045
let h=Math.floor(itime/3600)
let m=Math.floor(itime%3600/60)
let s=Math.floor(itime%3600%60)
h=h>10?h:'0'+h
m=m>10?m:'0'+m
s=s>10?s:'0'+s

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

