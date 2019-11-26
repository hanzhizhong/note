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