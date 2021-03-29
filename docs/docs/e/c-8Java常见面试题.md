# 1. Java常见面试题：评论回复表设计

**简介：** 如果要进行评论回复处理的话，实际上这里面需要考虑以下几种情况：一是评论的回复是回复一次，二是一直进行回复处理。如果现在只是进行一次回复处理，那么最简单的做法就是直接在表中增加一个字段，改字段描述的就是一次回复。



## 评论回复表设计

如果要进行评论回复处理的话，实际上这里面需要考虑以下几种情况：
（1）评论的回复是回复一次
（2）一直进行回复处理
如果现在只是进行一次回复处理，那么最简单的做法就是直接在表中增加一个字段，改字段描述的就是一次回复。

![image.png](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011101451.png)

假如现在要在网易新闻客户端，今日头条的客户端上进行无限制的回复处理，最简单的做法就是保存上次的回复编号以及回复的内容。如果不存放内容，则评论显示的时候就会造成大量的性能浪费。

![image.png](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011101459.png)

这样的操作就相当于实现了无限极的评论处理，就是现在见到的最多的情况，但这样的处理只能够针对评论有限的情况，在评论很多的情况下，就需要考虑库表分离设计原则等等。
除了要进行有效的数据存储之外，还需要去考虑数据的缓存处理问题，对于缓存就需要考虑使用哪种缓存策略以及缓存的标注。
很多时候为了提升性能，在进行页面分析的时候也可以做一些伪装的处理，例如将新闻的数据变为静态文件进行分享，取热门的几个回复做为默认的显示，这些就属于程序层次上的改良。





# 2. Java常见面试题：高并发处理包

**简介：** 在Java里有一个包：java.util.concurrent包，这组开发包是从JDK1.5的时候开始添加到JDK系统之中的，主要目的是进行高并发访问的处理，也就是说通过这个程序实现的开发包都将基于线程池的高速操作完成，而对于线程池一共有四种：任意扩张的线程池、定长线程池、线程调度池、单线程池。

## 高并发处理包

在Java里有一个包：java.util.concurrent包，这组开发包是从JDK1.5的时候开始添加到JDK系统之中的，主要目的是进行高并发访问的处理，也就是说通过这个程序实现的开发包都将基于线程池的高速操作完成，而对于线程池一共有四种：任意扩张的线程池、定长线程池、线程调度池、单线程池。

在进行项目的开发过程中，如果某一个操作按照原始方式进行代码开发，则可以无限制的进行线程的扩展。

![image.png](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011102245.png)

前提：公用信息需要进行重复使用。

![image.png](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011102255.png)

它依然是Map接口的子类，操作方法还是以Map接口定义的为主。接口有一个特点：不同的子类针对同一方法有不同的实现。

**那么HashMap、Hashtable、ConcurrentHashMap有什么区别的？**

**Hashtable**：进行公共数据保存的时候线程的安全性是最高的，因为同一时刻只允许一个线程进行操作；

**HashMap**：所有的方法都属于异步处理的，这样可以保证操作速度快，前提是：处理多个用户并发访问，但是不安全；

**ConcurrentHashMap**：在进行数据存储或读取的时候并不是简单的按照HashCode（）进行简单存放，而是经过了一些处理之后以保证高速的响应，前提是：需要有一个并发访问的Map的高效数据。

例如：现在有一些内容需要临时记录在一个Map集合里面，这个内容有可能有几类用户去看：送餐员、客户、管理者，这个时候这个集合一定是一个公共的集合，对于这样的公共集合数据，就必须进行快速响应，而且还需要保存大量的内容。

![image.png](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011102301.png)





# 3. Java常见面试题：方法变量与同步

**简介：** 方法中的变量都是局部变量，如果我们去考虑线程安全性问题，一定是在多个线程访问同一个资源的时候进行的。既然是同一个资源，就必须考虑Runnable、Callable接口来实现多线程处理关系。



## 方法中的变量是否是线程安全？

方法中的变量都是局部变量，如果我们去考虑线程安全性问题，一定是在多个线程访问同一个资源的时候进行的。既然是同一个资源，就必须考虑Runnable、Callable接口来实现多线程处理关系。
方法的定义上如果使用了synchronized，那么这个方法中就表示同步的处理操作，这个同步的处理操作指的是当前的方法只允许有一个线程进入。

![image-20201011103311343](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011103656.png)

每次执行的时候都会只有一个线程进入到sale（）方法，但是会有其它的方法等待进入处理。在一个同步线程的处理之中，肯定变量是同步的，如果说你现在取消了同步。



![image-20201011103722405](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011103723.png)



线程同步处理之中不会去考虑方法中的参数，只会考虑类中的属性。





# 4. Java常见面试题：线程池

**简介：** 在项目的开发里面对于线程池¬应用最多的地方就在数据库的连接池上，如果要想实现线程池，需要一个专门的类完成（java.util.concurrent包）。



## 线程池

在项目的开发里面对于线程池¬应用最多的地方就在数据库的连接池上，如果要想实现线程池，需要一个专门的类完成（java.util.concurrent包）：public interface ExecutorService extends Executor。

（1）线程的执行操作：public void execute（Runnable command）；
如果想取得ExecutorService子接口对象，则必须利用java.util.concurrent.Executors类完成实例化；
（2）创建一个无限大小的线程池：public static ExecutorService newCachedThreadPool（）；
（3）创建有限大小的线程池：public static ExecutorService newFixedThreadPool（int nThreads）

*范例：创建一个无限大小的线程池*

![image.png](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011105216.png)

*范例：创建3个大小的线程池*

![image.png](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011105223.png)

此时由于线程池的空间只能够存放有三个线程的对象，所以对于不能保存的线程，将会在队列之中进行等待。
如果要是不确定能放多少线程池的话，可以通过以下的操作取得系统可用进程的数量，可以用它确定线程池的大小。

![image.png](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011105231.png)







# 5. Java常见面试题：泛型中“T”与“？”的区别

**简介：** 这两个操作的特点是一个用于泛型类型的声明上，一个用于方法的接收参数或者返回类型上。



#### 泛型中“T”与“？”的区别

这两个操作的特点是一个用于泛型类型的声明上，一个用于方法的接收参数或者返回类型上。

![image.png](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011105457.png)

![image.png](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011105528.png)

![image.png](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011105511.png)

![image.png](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011105541.png)
使用“类<?>”表示只能够取得内容，但是不允许设置内容。





# 6. Java常见面试题：泛型通配符问题

**简介：** 对于这两种操作大部分情况下我们是不进行比较的，因为两者的使用环境是不同的，在大部分情况下如果要进行方法的参数接收考虑使用“？”，它的特点是可以保证程序不出现不必要的修改。



#### \<T>和<?>的区别?

对于这两种操作大部分情况下我们是不进行比较的，因为两者的使用环境是不同的，在大部分情况下如果要进行方法的参数接收考虑使用“？”，它的特点是可以保证程序不出现不必要的修改。

![image-20201011105841192](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011105842.png)



因为泛型可以设置所有的类型，所以面对当前的开发环境就发现fun（）方法将出现问题，也就是说如果Message使用的时候设置的不是String，那么就有可能无法使用。



![image-20201011105949027](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011105950.png)

对于泛型的应用除了可以在自定义类上使用，其它使用最多的环境可能就是反射机制了。

![image-20201011110025106](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011110026.png)

以上的工厂类只为一个IMessage接口服务，但是从实际的开发来讲，可能会有无数个借口对象都需要通过工厂类获得，在这样的情况下就可以利用泛型来解决问题。

![image-20201011110058012](https://cdn.jsdelivr.net/gh/CoderMerlin/blog-image/images/interview/java20201011110059.png)

如果所有的程序代码都这样编写实际上也会比较辛苦，在很多实际开发中，对于以上的代码有个基本的认识即可，毕竟有开发框架，开发框架里面帮助开发者隐藏了所有的实现细节。