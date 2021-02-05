# note
笔记

~~~css
ENOENT 没有这样的文件或目录路径名中的目录不存在或是无效的符号连接
~~~

#### 平方和亩的换算

~~~css
平方=>亩
	加半左移3位小数点
	128m*m + 64m*m=192m*m => 0.192亩
亩=>平方
	除三加倍右移三
	5亩=> 5/3 *2=3.3333 => 3333.3m*m
~~~

#### 异或交换定律

~~~css
如果：a^b=c
那么: a^c=b

a^b=c
A^b^b=a
~~~



#### Linux中使用gulp报 “Error:watch error ---ENOSPC”

~~~css
这是因为在一个系统里，能够被监听的文件数量上限是一定的，node的监听其实是调用了fs.watch，从上面的错误信息也可以看到相关提示，fs.watch的监听数量在不同的系统里面不一样，所以有可能我的windows下面这个值很大，而在虚拟机里面很小（在家里的Ubuntu下也比较大，应该是跟虚拟机有关），所以我们应该想办法增加这个监听数量值。在终端运行下面的代码：
echo fs.inotify.max_user_watches=582222 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
~~~

