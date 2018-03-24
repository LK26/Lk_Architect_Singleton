####1.设计模式分为三个类
* 创建型
* 结构型
* 行为型

####2.创建型:单例模式
* 为什么用单例模式?
如果你看到这个问题,你怎么回答呢?
1. OC编程习惯
2. 4.2版本之前是MRC(手动分配和释放内存)
alloc:开辟内存  release:释放内存
开发者有的时候很容易忽略释放

3. 4.2之后ARC(自动引用计数)
我们创建对象的时候,返回重建对象,不去回收,就会造成内存溢出.

4. 有的时候整个程序中为了避免创建多个对象,引入了单例模式

* 单例模式(特点)?
单例模式有哪些特点,此时在你的脑海中是否已经想出了这个问题的答案?是否知道怎么使用,是否能用语言描述.
1. 有且只有一个实例(全局唯一)
2. 必须自行创建一个实例
3. 必须提供一个全局实例,暴露给外部使用

* 单例模式角色划分
1. 单例类
2. 客户端(使用者)
* 单例模式--如何实现?
约束:
1. 约束一: 提供一个静态实例,一般情况下设置为nil.
2. 约束二: 提供一个函数创建单例,如果单例存在就返回,如果不存在就创建.
3. 约束三:在OC里面重写父类中的allocWithZone方法,保证是一个单例,当我们在调用alloc的时候回调该方法.
4. 约束四:OC需要重写父类copyWithzone等,Swift中你要将构建方法私有化,Java也是一样构造方法私有化.

####3.实现单例
* OC-单例模式
线程安全和非线程安全
1.标准单例模式01
![Singleton01.h.png](https://upload-images.jianshu.io/upload_images/2960658-0ffb01912c2a2ac1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![Singleton01.m.png](https://upload-images.jianshu.io/upload_images/2960658-5ccf86b1c957fddb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 1. allocWithZone方法保证唯一性
> 2. 如果没有多线程情况,就可以使用标准模式
> 3. 标准单例(非线程安全)

2. 标准单例模式02
![Singleton02.h.png](https://upload-images.jianshu.io/upload_images/2960658-ed07de0b8eb81941.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![Singleton02.m.png](https://upload-images.jianshu.io/upload_images/2960658-f08710f9f5b77c09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 使用GCD,线程安全下使用.

```
标准单例模式01和标准单例模式02 是等你需要的时候进行创建
```
3. 标准单例模式03

> 1.标准单例模式03 不管你要不要我都给你
> 2.load方法(当我们应用程序运行的时候,回调该方法)

![Singleton03.h.png](https://upload-images.jianshu.io/upload_images/2960658-5570ad538f91df88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![Singleton03.m.png](https://upload-images.jianshu.io/upload_images/2960658-fc48bb6f86f12d59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. 标准单例模式04
> @synchronized 关键字,同一个时间点,只允许一个来访问,有很多来访问,只需要等待,线程等待,保证了线程的安全.

![Singleton04.h.png](https://upload-images.jianshu.io/upload_images/2960658-6dce05ce80ff5e69.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Singleton04.m.png](https://upload-images.jianshu.io/upload_images/2960658-2e85b967abed468f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* swift-单例模式
1. 标准单例模式01
> 所谓单例模式: 不能够被继承(可选)
在我们的swift中是可以的(final关键字)
非线程安全

![加入final关键字.png](https://upload-images.jianshu.io/upload_images/2960658-13f42dff6de58c69.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Test不能继承Singleton01(报错).png](https://upload-images.jianshu.io/upload_images/2960658-e2e096c0638a3422.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
相比OC单例模式,swift是不是更安全一些.
```
2. 标准单例模式02
> 静态属性实例化:线程安全

![静态属性实例化.png](https://upload-images.jianshu.io/upload_images/2960658-536e0b26ae51d70b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 标准单例模式03
> 结构体实现单例,线程安全

![结构体实现单例.png](https://upload-images.jianshu.io/upload_images/2960658-d3166f3a12b957ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* java --单例模式
1. 标准单例模式01
> 非线程安全,与swift单例模式很相似

![java单例模式--非线程安全.png](https://upload-images.jianshu.io/upload_images/2960658-18069d9901301e3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 标准单例模式02
> 使用synchronized关键字,保证线程安全

![synchronized--线程安全.png](https://upload-images.jianshu.io/upload_images/2960658-10083c01b72efc14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.标准单例模式
> 枚举定义单例

![枚举定义单例.png](https://upload-images.jianshu.io/upload_images/2960658-e4452b92316b887f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####4.UML单例图

![UML单例图.png](https://upload-images.jianshu.io/upload_images/2960658-7de0aa516b1b2fad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>客户端类:持有单例对象引用
临时性的引用,处于依赖关系.

####5.单例模式-使用场景
1.iOS使用场景
UIApplication,NSNotificationCenter等等
2.实际开发中:工具类(单例,数据库等等)

####6.总结
通过对OC,swift,java这些单例模式设计,
是不是对单例有了新的了解.
自己亲自动手一下,单例模式会更加深刻.
代码我稍后整理发到github,大家有需要可以下载.
不过,不建议拿来代码看.
尽量自己敲一遍.
好的,这就是单例模式.
下一节,我们继续进行模式设计.
如果您喜欢这篇文章,请记得点赞哦.













