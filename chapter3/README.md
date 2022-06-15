# 操作apk



废话不多说了，直接上才艺吧。这次我手把手教你操作apk，可以学到如何将apk完整内置，编辑apk。

![上才艺](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2662fbcbea8f47ee969985555966e353~tplv-k3u1fbpfcp-watermark.image?)


排除干扰项，我直接用我之前一个开源项目做样本，[样本下载地址](https://github.com/Reginer/Renju)，下载里面的 `app_debug.apk` 。

# 内置apk

下面讲解的方法是以aar的方式内置，apk本质上和aar可以说没什么大区别，简单做一下转换就可以把apk变为aar的方式内置，下面直接操作。

打开Android Studio，新建项目，选择No Activity，之后一路默认，不用选择Kotlin，选择也可以，我是这个版本 

![Android Studio](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0f3babd8272344ebb1a8fedacf178825~tplv-k3u1fbpfcp-watermark.image?)

创建完就是下面这个样子，再创建个Module，有人不知道为什么创建Module ？ Module可以直接生成aar，为了留给你实操的空间，我不创建Module了，直接在app模块中操作。

![AS空项目](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/829e6a66749140c188fb30def89b149d~tplv-k3u1fbpfcp-watermark.image?)

简单操作一下这个空项目，把依赖全部移除掉，为什么移除掉？因为完整apk已经具备全部环境了，就使用它的环境就可以。包括Android X配置也都可以移除，不移除也可以，什么情况下可以不移除，保留实操空间。


![画线的都删除](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cbf68188fd8c4852accab0d8743f4180~tplv-k3u1fbpfcp-watermark.image?)


下面开始正式操作apk，需要一个工具，[我将它开源了，点这看源码](https://github.com/Reginer/AutoAPKTool)

## 简单介绍一下apk构成，由两部分构成
- 代码，包括class代码，表现为dex；包括native代码，表现为so
- 资源，包括res文件夹，AndroidManifest.xml，和assets文件夹

我们的操作很简单，把dex转为jar集成，把各种xml反编译直接放到AS新建项目内，把so放到jniLibs下，assets有的话直接覆盖。

1. 反编译apk，得到资源文件

![反编译apk](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9b2a9bb714664f429e36c631c3b5cf9f~tplv-k3u1fbpfcp-watermark.image?)

反编译后查看 `apktool.yml`取得minSdk，将空项目minSdk改为同样值。

![反编译后](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/76201ee6890a49e8bb2fa0081dae5192~tplv-k3u1fbpfcp-watermark.image?)

2. 取得资源后，将资源文件覆盖到新建项目内


![res和AndroidManifest.xml](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fd89912247824f9bb6b9f5f65caa2bb4~tplv-k3u1fbpfcp-watermark.image?)

3.将dex转为jar，so直接放到jniLibs下

用压缩软件打开apk文件，取得其中的dex文件，用反编译软件将dex转成jar。

![将dex转成jar](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/13786f25781a474fb5af16360f261f1e~tplv-k3u1fbpfcp-watermark.image?)

转完之后，将jar包依赖到项目，也就是放到libs目录下，同时增加如下配置,然后Sync项目:

````
implementation fileTree(include: ['*.jar'], dir: 'libs')
````
lib下的so文件对应放到新建的jniLibs下

![操作jar和so](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/53b7bcbfed6e4a4ca9ab3729eb918d9e~tplv-k3u1fbpfcp-watermark.image?)

4.处理一下现有错误，报错的大多可以删除，部分根据提示修改，然后就可以直接运行项目了

![AndroidManifest.xml](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fa0e06803dc343a18b458d168673b534~tplv-k3u1fbpfcp-watermark.image?)

哪里报错删哪里:

![删除错误文件](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/10209bb2ffc0427cbd032557a96fb394~tplv-k3u1fbpfcp-watermark.image?)


![删除同名文件](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8ffa63fb0d8f4b05aeeea8cfe9ff4b48~tplv-k3u1fbpfcp-watermark.image?)

其他的不截图了，到这里报错的全删除，到了这里:

![类重名冲突](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/99d2eab73f2a424c86f5e9c090208224~tplv-k3u1fbpfcp-watermark.image?)

解决办法是删除classes3.jar里的BuildConfig文件，删另外一个也行

![删除BuildConfig](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c1f6eaed8a0c466f9e8fe3502f755613~tplv-k3u1fbpfcp-watermark.image?)


# 不出意外的话到这里apk已经可以运行起来了
但是不幸的是，运行报错了 

![报错日志](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8b5015519fb94bc19e0e5d1052b9bb79~tplv-k3u1fbpfcp-watermark.image?)


![初始化失败](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/81ab427b9627410f81941b5eb3fcd461~tplv-k3u1fbpfcp-watermark.image?)

通过查看发现是调用的R获取的drawable，那么显然是编译的问题，内部资源和已编译的资源无法对应。

到这里就涉及到第二个技能，修改原apk代码。其实非常简单，创建同名文件，替换内部代码。


![替换同名文件](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eea2af26924c44aab726a5f4912205b7~tplv-k3u1fbpfcp-watermark.image?)



![将原jar包内文件删除](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/801b6955e5344c0293a3e3740189caea~tplv-k3u1fbpfcp-watermark.image?)


进行到这里之后，整个修改就算是完成了。



![成果](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1d435f4486f94b8398dfb41cc644dedc~tplv-k3u1fbpfcp-watermark.image?)


### 我们之后主要会用到的技能是查找和修改，我前面只演示了修改一个文件，把原代码替换进去，之后的路还会有置换逻辑，都作为实操自己实现吧。


