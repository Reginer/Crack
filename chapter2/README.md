要实现的功能是，登录成功。

打开Apk反编译工具，将上一篇中生成的apk拖入其中,点击 反编译apk。

![开始反编译](https://github.com/Reginer/Crack/blob/master/chapter2/开始反编译.png)

将apk使用压缩文件打开，解压出其中的dex文件，正常只有一个，分包会有多个。打开Apk反编译中的打开jadx，将dex文件拖入其中，可以看到伪码。

![查看伪码](https://github.com/Reginer/Crack/blob/master/chapter2/查看伪码.png)

打开MainActivity.smali文件，发现代码稍乱，那么需要简单了解几个smali语法([这部分抄袭](https://www.cnblogs.com/lee0oo0/p/3728271.html)):

```
.field private isFlag:z　　定义变量

.method　　方法

.parameter　　方法参数

.prologue　　方法开始

.line 12　　此方法位于第12行

invoke-super　　调用父函数

const/high16  v0, 0x7fo3　　把0x7fo3赋值给v0

invoke-direct　　调用函数

return-void　　函数返回void

.end method　　函数结束

new-instance　　创建实例

iput-object　　对象赋值

iget-object　　调用对象

invoke-static　　调用静态函数

条件跳转分支：

"if-eq vA, vB, :cond_**"   如果vA等于vB则跳转到:cond_**

"if-ne vA, vB, :cond_**"   如果vA不等于vB则跳转到:cond_**

"if-lt vA, vB, :cond_**"    如果vA小于vB则跳转到:cond_**

"if-ge vA, vB, :cond_**"   如果vA大于等于vB则跳转到:cond_**

"if-gt vA, vB, :cond_**"   如果vA大于vB则跳转到:cond_**

"if-le vA, vB, :cond_**"    如果vA小于等于vB则跳转到:cond_**

"if-eqz vA, :cond_**"   如果vA等于0则跳转到:cond_**

"if-nez vA, :cond_**"   如果vA不等于0则跳转到:cond_**

"if-ltz vA, :cond_**"    如果vA小于0则跳转到:cond_**

"if-gez vA, :cond_**"   如果vA大于等于0则跳转到:cond_**

"if-gtz vA, :cond_**"   如果vA大于0则跳转到:cond_**

"if-lez vA, :cond_**"    如果vA小于等于0则跳转到:cond_**
```

了解这些基础语法，比对伪码看懂smali还是问题不大的。

![smali讲解](https://github.com/Reginer/Crack/blob/master/chapter2/smali讲解.png)

看到这里想达到目的就有很多方法了，我这里讲解我最想说的方法。让你们知道为什么if的做法是烂做法。

只要将其中的两个```if-eqz```改为```if-nez```，那么结果就是只要不是输入的之前正确的账号密码都能登录成功。

打开Apk反编译，先配置一个签名。之后将反编译后的apk目录拖入其中，点回编译apk。

目的就达到了。


