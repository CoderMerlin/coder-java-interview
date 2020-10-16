图片：值传递，如何正确定义接口的返回值(boolean/Boolean)类型及命名(success/isSuccess)，字符串的不可变性，JDK 6和JDK 7中substring的原理及区别,字符串拼接的几种方式和区别,Class常量池



## 值传递

## 1.什么是值传递、引用传递？

### 实参与形参

我们都知道，在Java中定义方法的时候是可以定义参数的。比如Java中的main方法，`public static void main(String[] args)`，这里面的args就是参数。参数在程序语言中分为形式参数和实际参数。

> 形式参数：是在定义函数名和函数体的时候使用的参数,目的是用来接收调用该函数时传入的参数。
>
> 实际参数：在调用有参函数时，主调函数和被调函数之间有数据传递关系。在主调函数中调用一个函数时，函数名后面括号中的参数称为“实际参数”。

简单举个例子：

```
public static void main(String[] args) {
  ParamTest pt = new ParamTest();
  pt.sout("Hollis");//实际参数为 Hollis
}

public void sout(String name) { //形式参数为 name
  System.out.println(name);
}
```

实际参数是调用有参方法的时候真正传递的内容，而形式参数是用于接收实参内容的参数。

### 值传递与引用传递

上面提到了，当我们调用一个有参函数的时候，会把实际参数传递给形式参数。但是，在程序语言中，这个传递过程中传递的两种情况，即值传递和引用传递。我们来看下程序语言中是如何定义和区分值传递和引用传递的。

> 值传递（pass by value）是指在调用函数时将实际参数`复制`一份传递到函数中，这样在函数中如果对`参数`进行修改，将不会影响到实际参数。
>
> 引用传递（pass by reference）是指在调用函数时将实际参数的地址`直接`传递到函数中，那么在函数中对`参数`所进行的修改，将影响到实际参数。

那么，我来给大家总结一下，值传递和引用传递之前的区别的重点是什么:

<img src="http://www.hollischuang.com/wp-content/uploads/2018/04/pass.jpg" alt="pass" width="474" height="73" class="aligncenter size-full wp-image-2289" />

这里我们来举一个形象的例子。再来深入理解一下值传递和引用传递：

你有一把钥匙，当你的朋友想要去你家的时候，如果你`直接`把你的钥匙给他了，这就是引用传递。这种情况下，如果他对这把钥匙做了什么事情，比如他在钥匙上刻下了自己名字，那么这把钥匙还给你的时候，你自己的钥匙上也会多出他刻的名字。

你有一把钥匙，当你的朋友想要去你家的时候，你`复刻`了一把新钥匙给他，自己的还在自己手里，这就是值传递。这种情况下，他对这把钥匙做什么都不会影响你手里的这把钥匙。


### 参考资料

[Evaluation strategy][7] 

[关于值传递和引用传递][8] 

[按值传递、按引用传递、按共享传递][9] 

[Is Java “pass-by-reference” or “pass-by-value”?][2]

[2]: https://stackoverflow.com/questions/40480/is-java-pass-by-reference-or-pass-by-value
[7]: https://en.wikipedia.org/wiki/Evaluation_strategy
[8]: http://chenwenbo.github.io/2016/05/11/%E5%85%B3%E4%BA%8E%E5%80%BC%E4%BC%A0%E9%80%92%E5%92%8C%E5%BC%95%E7%94%A8%E4%BC%A0%E9%80%92/
[9]: http://menzhongxin.com/2017/02/07/%E6%8C%89%E5%80%BC%E4%BC%A0%E9%80%92-%E6%8C%89%E5%BC%95%E7%94%A8%E4%BC%A0%E9%80%92%E5%92%8C%E6%8C%89%E5%85%B1%E4%BA%AB%E4%BC%A0%E9%80%92/


## 2.为什么说Java中只有值传递？

对于初学者来说，要想把这个问题回答正确，最初思考这个问题的时候，我发现我竟然无法通过简单的语言把这个事情描述的很容易理解，遗憾的是，我也没有在网上找到哪篇文章可以把这个事情讲解的通俗易懂。所以，就有了我写这篇文章的初衷。

### 辟谣时间

关于这个问题，在[StackOverflow][2]上也引发过广泛的讨论，看来很多程序员对于这个问题的理解都不尽相同，甚至很多人理解的是错误的。还有的人可能知道Java中的参数传递是值传递，但是说不出来为什么。

在开始深入讲解之前，有必要纠正一下大家以前的那些错误看法了。如果你有以下想法，那么你有必要好好阅读本文。

> 错误理解一：值传递和引用传递，区分的条件是传递的内容，如果是个值，就是值传递。如果是个引用，就是引用传递。
>
> 错误理解二：Java是引用传递。
>
> 错误理解三：传递的参数如果是普通类型，那就是值传递，如果是对象，那就是引用传递。

### 实参与形参

我们都知道，在Java中定义方法的时候是可以定义参数的。比如Java中的main方法，`public static void main(String[] args)`，这里面的args就是参数。参数在程序语言中分为形式参数和实际参数。

> 形式参数：是在定义函数名和函数体的时候使用的参数,目的是用来接收调用该函数时传入的参数。
> 
> 实际参数：在调用有参函数时，主调函数和被调函数之间有数据传递关系。在主调函数中调用一个函数时，函数名后面括号中的参数称为“实际参数”。

简单举个例子：

    public static void main(String[] args) {
      ParamTest pt = new ParamTest();
      pt.sout("Hollis");//实际参数为 Hollis
    }
    
    public void sout(String name) { //形式参数为 name
      System.out.println(name);
    }
    

实际参数是调用有参方法的时候真正传递的内容，而形式参数是用于接收实参内容的参数。

### 求值策略

我们说当进行方法调用的时候，需要把实际参数传递给形式参数，那么传递的过程中到底传递的是什么东西呢？

这其实是程序设计中**求值策略（Evaluation strategies）**的概念。

在计算机科学中，求值策略是确定编程语言中表达式的求值的一组（通常确定性的）规则。求值策略定义何时和以何种顺序求值给函数的实际参数、什么时候把它们代换入函数、和代换以何种形式发生。

求值策略分为两大基本类，基于如何处理给函数的实际参数，分为严格的和非严格的。

#### 严格求值

在“严格求值”中，函数调用过程中，给函数的实际参数总是在应用这个函数之前求值。多数现存编程语言对函数都使用严格求值。所以，我们本文只关注严格求值。

在严格求值中有几个关键的求值策略是我们比较关心的，那就是**传值调用**（Call by value）、**传引用调用**（Call by reference）以及**传共享对象调用**（Call by sharing）。

*   传值调用（值传递） 
    *   在传值调用中，实际参数先被求值，然后其值通过复制，被传递给被调函数的形式参数。因为形式参数拿到的只是一个"局部拷贝"，所以如果在被调函数中改变了形式参数的值，并不会改变实际参数的值。
*   传引用调用（引用传递） 
    *   在传引用调用中，传递给函数的是它的实际参数的隐式引用而不是实参的拷贝。因为传递的是引用，所以，如果在被调函数中改变了形式参数的值，改变对于调用者来说是可见的。
*   传共享对象调用（共享对象传递） 
    *   传共享对象调用中，先获取到实际参数的地址，然后将其复制，并把该地址的拷贝传递给被调函数的形式参数。因为参数的地址都指向同一个对象，所以我们也称之为"传共享对象"，所以，如果在被调函数中改变了形式参数的值，调用者是可以看到这种变化的。

不知道大家有没有发现，其实传共享对象调用和传值调用的过程几乎是一样的，都是进行"求值"、"拷贝"、"传递"。你品，你细品。

![][1]￼

但是，传共享对象调用和内传引用调用的结果又是一样的，都是在被调函数中如果改变参数的内容，那么这种改变也会对调用者有影响。你再品，你再细品。

那么，共享对象传递和值传递以及引用传递之间到底有很么关系呢？

对于这个问题，我们应该关注过程，而不是结果，**因为传共享对象调用的过程和传值调用的过程是一样的，而且都有一步关键的操作，那就是"复制"，所以，通常我们认为传共享对象调用是传值调用的特例**

我们先把传共享对象调用放在一边，我们再来回顾下传值调用和传引用调用的主要区别：

**传值调用是指在调用函数时将实际参数`复制`一份传递到函数中，传引用调用是指在调用函数时将实际参数的引用`直接`传递到函数中。**

![pass-by-reference-vs-pass-by-value-animation][2]￼

所以，两者的最主要区别就是是直接传递的，还是传递的是一个副本。

这里我们来举一个形象的例子。再来深入理解一下传值调用和传引用调用：

你有一把钥匙，当你的朋友想要去你家的时候，如果你`直接`把你的钥匙给他了，这就是引用传递。

这种情况下，如果他对这把钥匙做了什么事情，比如他在钥匙上刻下了自己名字，那么这把钥匙还给你的时候，你自己的钥匙上也会多出他刻的名字。

你有一把钥匙，当你的朋友想要去你家的时候，你`复刻`了一把新钥匙给他，自己的还在自己手里，这就是值传递。

这种情况下，他对这把钥匙做什么都不会影响你手里的这把钥匙。

### Java的求值策略

前面我们介绍过了传值调用、传引用调用以及传值调用的特例传共享对象调用，那么，Java中是采用的哪种求值策略呢？

很多人说Java中的基本数据类型是值传递的，这个基本没有什么可以讨论的，普遍都是这样认为的。

但是，有很多人却误认为Java中的对象传递是引用传递。之所以会有这个误区，主要是因为Java中的变量和对象之间是有引用关系的。Java语言中是通过对象的引用来操纵对象的。所以，很多人会认为对象的传递是引用的传递。

而且很多人还可以举出以下的代码示例：

    public static void main(String[] args) {
      Test pt = new Test();
    
      User hollis = new User();
      hollis.setName("Hollis");
      hollis.setGender("Male");
      pt.pass(hollis);
      System.out.println("print in main , user is " + hollis);
    }
    
    public void pass(User user) {
      user.setName("hollischuang");
      System.out.println("print in pass , user is " + user);
    }
    

输出结果：

    print in pass , user is User{name='hollischuang', gender='Male'}
    print in main , user is User{name='hollischuang', gender='Male'}
    

可以看到，对象类型在被传递到pass方法后，在方法内改变了其内容，最终调用方main方法中的对象也变了。

所以，很多人说，这和引用传递的现象是一样的，就是在方法内改变参数的值，会影响到调用方。

但是，其实这是走进了一个误区。

### Java中的对象传递

很多人通过代码示例的现象说明Java对象是引用传递，那么我们就从现象入手，先来反驳下这个观点。

我们前面说过，无论是值传递，还是引用传递，只不过是求值策略的一种，那求值策略还有很多，比如前面提到的共享对象传递的现象和引用传递也是一样的。那凭什么就说Java中的参数传递就一定是引用传递而不是共享对象传递呢？

那么，Java中的对象传递，到底是哪种形式呢？其实，还真的就是共享对象传递。

其实在 《The Java™ Tutorials》中，是有关于这部分内容的说明的。首先是关于基本类型描述如下：

> Primitive arguments, such as an int or a double, are passed into methods by value. This means that any changes to the values of the parameters exist only within the scope of the method. When the method returns, the parameters are gone and any changes to them are lost.

**即，原始参数通过值传递给方法。这意味着对参数值的任何更改都只存在于方法的范围内。当方法返回时，参数将消失，对它们的任何更改都将丢失。**

关于对象传递的描述如下：

> Reference data type parameters, such as objects, are also passed into methods by value. This means that when the method returns, the passed-in reference still references the same object as before. However, the values of the object’s fields can be changed in the method, if they have the proper access level.

**也就是说，引用数据类型参数(如对象)也按值传递给方法。这意味着，当方法返回时，传入的引用仍然引用与以前相同的对象。但是，如果对象字段具有适当的访问级别，则可以在方法中更改这些字段的值。**

这一点官方文档已经很明确的指出了，Java就是值传递，只不过是把对象的引用当做值传递给方法。你细品，这不就是共享对象传递么？

**其实Java中使用的求值策略就是传共享对象调用，也就是说，Java会将对象的地址的拷贝传递给被调函数的形式参数。**只不过"传共享对象调用"这个词并不常用，所以Java社区的人通常说"Java是传值调用"，这么说也没错，因为传共享对象调用其实是传值调用的一个特例。

### 值传递和共享对象传递的现象冲突吗？

看到这里很多人可能会有一个疑问，既然共享对象传递是值传递的一个特例，那么为什么他们的现象是完全不同的呢？

难道值传递过程中，如果在被调方法中改变了值，也有可能会对调用者有影响吗？那到底什么时候会影响什么时候不会影响呢？

其实是不冲突的，之所以会有这种疑惑，是因为大家对于到底是什么是"改变值"有误解。

我们先回到上面的例子中来，看一下调用过程中实际上发生了什么？

<img src="http://www.hollischuang.com/wp-content/uploads/2018/04/pass21.png" alt="pass2" width="832" height="732" class="aligncenter size-full wp-image-2307" />

在参数传递的过程中，实际参数的地址`0X1213456`被拷贝给了形参。这个过程其实就是值传递，只不过传递的值得内容是对象的应用。

那为什么我们改了user中的属性的值，却对原来的user产生了影响呢？

其实，这个过程就好像是：你复制了一把你家里的钥匙给到你的朋友，他拿到钥匙以后，并没有在这把钥匙上做任何改动，而是通过钥匙打开了你家里的房门，进到屋里，把你家的电视给砸了。

这个过程，对你手里的钥匙来说，是没有影响的，但是你的钥匙对应的房子里面的内容却是被人改动了。

也就是说，**Java对象的传递，是通过复制的方式把引用关系传递了，如果我们没有改引用关系，而是找到引用的地址，把里面的内容改了，是会对调用方有影响的，因为大家指向的是同一个共享对象。**

那么，如果我们改动一下pass方法的内容：

    public void pass(User user) {
      user = new User();
      user.setName("hollischuang");
      System.out.println("print in pass , user is " + user);
    }
    

上面的代码中，我们在pass方法中，重新new了一个user对象，并改变了他的值，输出结果如下：

    print in pass , user is User{name='hollischuang', gender='Male'}
    print in main , user is User{name='Hollis', gender='Male'}
    

再看一下整个过程中发生了什么：

<img src="http://www.hollischuang.com/wp-content/uploads/2018/04/pass1.png" alt="pass1" width="859" height="721" class="aligncenter size-full wp-image-2293" />

这个过程，就好像你复制了一把钥匙给到你的朋友，你的朋友拿到你给他的钥匙之后，找个锁匠把他修改了一下，他手里的那把钥匙变成了开他家锁的钥匙。这时候，他打开自己家，就算是把房子点了，对你手里的钥匙，和你家的房子来说都是没有任何影响的。

**所以，Java中的对象传递，如果是修改引用，是不会对原来的对象有任何影响的，但是如果直接修改共享对象的属性的值，是会对原来的对象有影响的。**

### 总结

我们知道，编程语言中需要进行方法间的参数传递，这个传递的策略叫做求值策略。

在程序设计中，求值策略有很多种，比较常见的就是值传递和引用传递。还有一种值传递的特例——共享对象传递。

值传递和引用传递最大的区别是传递的过程中有没有复制出一个副本来，如果是传递副本，那就是值传递，否则就是引用传递。

在Java中，其实是通过值传递实现的参数传递，只不过对于Java对象的传递，传递的内容是对象的引用。

**我们可以总结说，Java中的求值策略是共享对象传递，这是完全正确的。**

但是，为了让大家都能理解你说的，**我们说Java中只有值传递，只不过传递的内容是对象的引用。这也是没毛病的。**

但是，绝对不能认为Java中有引用传递。

OK，以上就是本文的全部内容，不知道本文是否帮助你解开了你心中一直以来的疑惑。欢迎留言说一下你的想法。

### 参考资料

[The Java™ Tutorials][3]

[Evaluation strategy][4]

[Is Java “pass-by-reference” or “pass-by-value”?][5]

[Passing by Value vs. by Reference Visual Explanation][6]

 [1]: https://www.hollischuang.com/wp-content/uploads/2020/04/15865905252659.jpg
 [2]: https://www.hollischuang.com/wp-content/uploads/2020/04/pass-by-reference-vs-pass-by-value-animation.gif
 [3]: https://docs.oracle.com/javase/tutorial/java/javaOO/arguments.html
 [4]: https://en.wikipedia.org/wiki/Evaluation_strategy
 [5]: https://stackoverflow.com/questions/40480/is-java-pass-by-reference-or-pass-by-value
 [6]: https://blog.penjee.com/passing-by-value-vs-by-reference-java-graphical/


 ## 基本数据类型

## 1. 8种基本数据类型

Java中有8种基本数据类型分为三大类。

### 字符型

char

### 布尔型

boolean

### 数值型

1.整型：byte、short、int、long 

2.浮点型：float、double

*String不是基本数据类型，是引用类型。*

## 2. 整型中byte、short、int、long的取值范围

Java中的整型主要包含byte、short、int和long这四种，表示的数字范围也是从小到大的，之所以表示范围不同主要和他们存储数据时所占的字节数有关。

先来个简答的科普，1字节=8位（bit）。java中的整型属于有符号数。

先来看计算中8bit可以表示的数字：
最小值：10000000 （-128）(-2^7)
最大值：01111111（127）(2^7-1)
具体计算方式参考：[Java中，为什么byte类型的取值范围为-128~127? - CSDN博客](https://blog.csdn.net/qq_23418393/article/details/57421688)

整型的这几个类型中，

　　byte：byte用1个字节来存储，范围为-128(-2^7)到127(2^7-1)，在变量初始化的时候，byte类型的默认值为0。

　　short：short用2个字节存储，范围为-32,768 (-2^15)到32,767 (2^15-1)，在变量初始化的时候，short类型的默认值为0，一般情况下，因为Java本身转型的原因，可以直接写为0。

　　int：int用4个字节存储，范围为-2,147,483,648 (-2^31)到2,147,483,647 (2^31-1)，在变量初始化的时候，int类型的默认值为0。

　　long：long用8个字节存储，范围为-9,223,372,036,854,775,808 (-2^63)到9,223,372,036, 854,775,807 (2^63-1)，在变量初始化的时候，long类型的默认值为0L或0l，也可直接写为0。

上面说过了，整型中，每个类型都有一定的表示范围，但是，在程序中有些计算会导致超出表示范围，即溢出。如以下代码：

        int i = Integer.MAX_VALUE;
        int j = Integer.MAX_VALUE;

        int k = i + j;
        System.out.println("i (" + i + ") + j (" + j + ") = k (" + k + ")");

输出结果：`i (2147483647) + j (2147483647) = k (-2)`

这就是发生了溢出，溢出的时候并不会抛异常，也没有任何提示。所以，在程序中，使用同类型的数据进行运算的时候，一定要注意数据溢出的问题。


## 3. 什么是浮点型？

在计算机科学中，浮点是一种对于实数的近似值数值表现法，由一个有效数字（即尾数）加上幂数来表示，通常是乘以某个基数的整数次指数得到。以这种表示法表示的数值，称为浮点数（floating-point number）。

计算机使用浮点数运算的主因，在于电脑使用二进位制的运算。例如：4÷2=2，4的二进制表示为100、2的二进制表示为010，在二进制中，相当于退一位数(100 -> 010)。

1的二进制是01，1.0/2=0.5，那么，0.5的二进制表示应该为(0.1)，以此类推，0.25的二进制表示为0.01，所以，并不是说所有的十进制小数都能准确的用二进制表示出来，如0.1，因此只能使用近似值的方式表达。

也就是说，，十进制的小数在计算机中是由一个整数或定点数（即尾数）乘以某个基数（计算机中通常是2）的整数次幂得到的，这种表示方法类似于基数为10的科学计数法。

一个浮点数a由两个数m和e来表示：a = m × be。在任意一个这样的系统中，我们选择一个基数b（记数系统的基）和精度p（即使用多少位来存储）。m（即尾数）是形如±d.ddd...ddd的p位数（每一位是一个介于0到b-1之间的整数，包括0和b-1）。如果m的第一位是非0整数，m称作正规化的。有一些描述使用一个单独的符号位（s 代表+或者-）来表示正负，这样m必须是正的。e是指数。

位（bit）是衡量浮点数所需存储空间的单位，通常为32位或64位，分别被叫作单精度和双精度。





## 4.什么是单精度和双精度？

单精度浮点数在计算机存储器中占用4个字节（32 bits），利用“浮点”（浮动小数点）的方法，可以表示一个范围很大的数值。

比起单精度浮点数，双精度浮点数(double)使用 64 位（8字节） 来存储一个浮点数。 



## 5. 为什么不能用浮点型表示金额？

由于计算机中保存的小数其实是十进制的小数的近似值，并不是准确值，所以，千万不要在代码中使用浮点数来表示金额等重要的指标。

建议使用BigDecimal或者Long（单位为分）来表示金额。


## 自动装箱，自动拆箱

## 1. 什么是自动装箱，自动拆箱？
本文主要介绍 Java 中的自动拆箱与自动装箱的有关知识。

### 基本数据类型

基本类型，或者叫做内置类型，是 Java 中不同于类(Class)的特殊类型。它们是我们编程中使用最频繁的类型。

Java 是一种强类型语言，第一次申明变量必须说明数据类型，第一次变量赋值称为变量的初始化。

Java 基本类型共有八种，基本类型可以分为三类：

> 字符类型 `char`
>
> 布尔类型 `boolean`
>
> 数值类型 `byte`、`short`、`int`、`long`、`float`、`double`。

数值类型又可以分为整数类型 `byte`、`short`、`int`、`long` 和浮点数类型 `float`、`double`。

Java 中的数值类型不存在无符号的，它们的取值范围是固定的，不会随着机器硬件环境或者操作系统的改变而改变。

实际上，Java 中还存在另外一种基本类型 `void`，它也有对应的包装类 `java.lang.Void`，不过我们无法直接对它们进行操作。

#### 基本数据类型有什么好处

我们都知道在 Java 语言中，`new` 一个对象是存储在堆里的，我们通过栈中的引用来使用这些对象；所以，对象本身来说是比较消耗资源的。

对于经常用到的类型，如 int 等，如果我们每次使用这种变量的时候都需要 new 一个 Java 对象的话，就会比较笨重。所以，和 C++ 一样，Java 提供了基本数据类型，这种数据的变量不需要使用 new 创建，他们不会在堆上创建，而是直接在栈内存中存储，因此会更加高效。

#### 整型的取值范围

Java 中的整型主要包含`byte`、`short`、`int`和`long`这四种，表示的数字范围也是从小到大的，之所以表示范围不同主要和他们存储数据时所占的字节数有关。

先来个简答的科普，1 字节= 8 位（bit）。Java 中的整型属于有符号数。

先来看计算中 8 bit 可以表示的数字：

    最小值：10000000 （-128）(-2^7)
    最大值：01111111（127）(2^7-1)


整型的这几个类型中，

*   byte：byte 用 1 个字节来存储，范围为 -128(-2^7) 到 127(2^7-1)，在变量初始化的时候，byte 类型的默认值为 0。

*   short：short 用 2 个字节存储，范围为 -32,768(-2^15) 到 32,767(2^15-1)，在变量初始化的时候，short 类型的默认值为 0，一般情况下，因为 Java 本身转型的原因，可以直接写为 0。

*   int：int 用 4 个字节存储，范围为 -2,147,483,648(-2^31) 到 2,147,483,647(2^31-1)，在变量初始化的时候，int 类型的默认值为 0。

*   long：long 用 8 个字节存储，范围为 -9,223,372,036,854,775,808(-2^63) 到 9,223,372,036, 854,775,807(2^63-1)，在变量初始化的时候，long 类型的默认值为 0L 或 0l，也可直接写为 0。

#### 超出范围怎么办

上面说过了，整型中，每个类型都有一定的表示范围，但是，在程序中有些计算会导致超出表示范围，即溢出。如以下代码：
```java
    int i = Integer.MAX_VALUE;
    int j = Integer.MAX_VALUE;

    int k = i + j;
    System.out.println("i (" + i + ") + j (" + j + ") = k (" + k + ")");
```

输出结果：i (2147483647) + j (2147483647) = k (-2)

**这就是发生了溢出，溢出的时候并不会抛异常，也没有任何提示。** 所以，在程序中，使用同类型的数据进行运算的时候，**一定要注意数据溢出的问题。**

### 包装类型

Java 语言是一个面向对象的语言，但是 Java 中的基本数据类型却是不面向对象的，这在实际使用时存在很多的不便，为了解决这个不足，在设计类时为每个基本数据类型设计了一个对应的类进行代表，这样八个和基本数据类型对应的类统称为包装类(Wrapper Class)。

包装类均位于 `java.lang` 包，包装类和基本数据类型的对应关系如下表所示

| 基本数据类型 | 包装类 |
| ------- | --------- |
| byte    | Byte      |
| boolean | Boolean   |
| short   | Short     |
| char    | Character |
| int     | Integer   |
| long    | Long      |
| float   | Float     |
| double  | Double    |

在这八个类名中，除了 Integer 和 Character 类以后，其它六个类的类名和基本数据类型一致，只是类名的第一个字母大写即可。

#### 为什么需要包装类

很多人会有疑问，既然 Java 中为了提高效率，提供了八种基本数据类型，为什么还要提供包装类呢？

这个问题，其实前面已经有了答案，因为 Java 是一种面向对象语言，很多地方都需要使用对象而不是基本数据类型。比如，在集合类中，我们是无法将 int 、double 等类型放进去的。因为集合的容器要求元素是 Object 类型。

为了让基本类型也具有对象的特征，就出现了包装类型，它相当于将基本类型“包装起来”，使得它具有了对象的性质，并且为其添加了属性和方法，丰富了基本类型的操作。

### 拆箱与装箱

那么，有了基本数据类型和包装类，肯定有些时候要在他们之间进行转换。比如把一个基本数据类型的 int 转换成一个包装类型的 Integer 对象。

我们认为包装类是对基本类型的包装，所以，把基本数据类型转换成包装类的过程就是打包装，英文对应于 boxing，中文翻译为装箱。

反之，把包装类转换成基本数据类型的过程就是拆包装，英文对应于 unboxing，中文翻译为拆箱。

在 Java SE5 之前，要进行装箱，可以通过以下代码：

```java
    Integer i = new Integer(10);
```

### 自动拆箱与自动装箱

在 Java SE5 中，为了减少开发人员的工作，Java 提供了自动拆箱与自动装箱功能。

自动装箱: 就是将基本数据类型自动转换成对应的包装类。

自动拆箱：就是将包装类自动转换成对应的基本数据类型。
```java
    Integer i = 10;  //自动装箱
    int b = i;     //自动拆箱
```

`Integer i=10` 可以替代 `Integer i = new Integer(10);`，这就是因为 Java 帮我们提供了自动装箱的功能，不需要开发者手动去 new 一个 Integer 对象。

### 自动装箱与自动拆箱的实现原理

既然 Java 提供了自动拆装箱的能力，那么，我们就来看一下，到底是什么原理，Java 是如何实现的自动拆装箱功能。

我们有以下自动拆装箱的代码：

```java
    public static  void main(String[]args){
        Integer integer=1; //装箱
        int i=integer; //拆箱
    }
```

对以上代码进行反编译后可以得到以下代码：

```java
    public static  void main(String[]args){
        Integer integer=Integer.valueOf(1);
        int i=integer.intValue();
    }
```

从上面反编译后的代码可以看出，int 的自动装箱都是通过 `Integer.valueOf()` 方法来实现的，Integer 的自动拆箱都是通过 `integer.intValue` 来实现的。如果读者感兴趣，可以试着将八种类型都反编译一遍 ，你会发现以下规律：

> 自动装箱都是通过包装类的 `valueOf()` 方法来实现的.自动拆箱都是通过包装类对象的 `xxxValue()` 来实现的。

### 哪些地方会自动拆装箱

我们了解过原理之后，在来看一下，什么情况下，Java 会帮我们进行自动拆装箱。前面提到的变量的初始化和赋值的场景就不介绍了，那是最简单的也最容易理解的。

我们主要来看一下，那些可能被忽略的场景。

#### 场景一、将基本数据类型放入集合类

我们知道，Java 中的集合类只能接收对象类型，那么以下代码为什么会不报错呢？

```java
    List<Integer> li = new ArrayList<>();
    for (int i = 1; i < 50; i ++){
        li.add(i);
    }
```

将上面代码进行反编译，可以得到以下代码：

```java
    List<Integer> li = new ArrayList<>();
    for (int i = 1; i < 50; i += 2){
        li.add(Integer.valueOf(i));
    }
```

以上，我们可以得出结论，当我们把基本数据类型放入集合类中的时候，会进行自动装箱。

#### 场景二、包装类型和基本类型的大小比较

有没有人想过，当我们对 Integer 对象与基本类型进行大小比较的时候，实际上比较的是什么内容呢？看以下代码：

```java
    Integer a = 1;
    System.out.println(a == 1 ? "等于" : "不等于");
    Boolean bool = false;
    System.out.println(bool ? "真" : "假");
```

对以上代码进行反编译，得到以下代码：

```java
    Integer a = 1;
    System.out.println(a.intValue() == 1 ? "等于" : "不等于");
    Boolean bool = false;
    System.out.println(bool.booleanValue ? "真" : "假");
```

可以看到，包装类与基本数据类型进行比较运算，是先将包装类进行拆箱成基本数据类型，然后进行比较的。

#### 场景三、包装类型的运算

有没有人想过，当我们对 Integer 对象进行四则运算的时候，是如何进行的呢？看以下代码：

```java
    Integer i = 10;
    Integer j = 20;

    System.out.println(i+j);
```

反编译后代码如下：

```java
    Integer i = Integer.valueOf(10);
    Integer j = Integer.valueOf(20);
    System.out.println(i.intValue() + j.intValue());
```

我们发现，两个包装类型之间的运算，会被自动拆箱成基本类型进行。

#### 场景四、三目运算符的使用

这是很多人不知道的一个场景，作者也是一次线上的血淋淋的 Bug 发生后才了解到的一种案例。看一个简单的三目运算符的代码：

```java
    boolean flag = true;
    Integer i = 0;
    int j = 1;
    int k = flag ? i : j;
```

很多人不知道，其实在 `int k = flag ? i : j;` 这一行，会发生自动拆箱（ JDK1.8 之前，详见：[《阿里巴巴Java开发手册-泰山版》提到的三目运算符的空指针问题到底是个怎么回事？](https://www.hollischuang.com/archives/4749) ）。

反编译后代码如下：

```java
    boolean flag = true;
    Integer i = Integer.valueOf(0);
    int j = 1;
    int k = flag ? i.intValue() : j;
    System.out.println(k);
```

这其实是三目运算符的语法规范。当第二，第三位操作数分别为基本类型和对象时，其中的对象就会拆箱为基本类型进行操作。

因为例子中，`flag ? i : j;` 片段中，第二段的 i 是一个包装类型的对象，而第三段的 j 是一个基本类型，所以会对包装类进行自动拆箱。如果这个时候 i 的值为 `null`，那么就会发生 NPE。（[自动拆箱导致空指针异常][1]）

#### 场景五、函数参数与返回值

这个比较容易理解，直接上代码了：

```java
    //自动拆箱
    public int getNum1(Integer num) {
     return num;
    }
    //自动装箱
    public Integer getNum2(int num) {
     return num;
    }
```

### 自动拆装箱与缓存

Java SE 的自动拆装箱还提供了一个和缓存有关的功能，我们先来看以下代码，猜测一下输出结果：

```java
    public static void main(String... strings) {

        Integer integer1 = 3;
        Integer integer2 = 3;

        if (integer1 == integer2)
            System.out.println("integer1 == integer2");
        else
            System.out.println("integer1 != integer2");

        Integer integer3 = 300;
        Integer integer4 = 300;

        if (integer3 == integer4)
            System.out.println("integer3 == integer4");
        else
            System.out.println("integer3 != integer4");
    }
```

我们普遍认为上面的两个判断的结果都是 false。虽然比较的值是相等的，但是由于比较的是对象，而对象的引用不一样，所以会认为两个 if 判断都是 false 的。在 Java 中，`==` 比较的是对象引用，而 `equals` 比较的是值。所以，在这个例子中，不同的对象有不同的引用，所以在进行比较的时候都将返回 false。奇怪的是，这里两个类似的 if 条件判断返回不同的布尔值。

上面这段代码真正的输出结果：

    integer1 == integer2
    integer3 != integer4


原因就和 Integer 中的缓存机制有关。在 Java 5 中，在 Integer 的操作上引入了一个新功能来节省内存和提高性能。整型对象通过使用相同的对象引用实现了缓存和重用。

> 适用于整数值区间 -128 至 +127。
>
> 只适用于自动装箱。使用构造函数创建对象不适用。

具体的代码实现可以阅读[Java中整型的缓存机制][2]一文，这里不再阐述。

我们只需要知道，当需要进行自动装箱时，如果数字在 -128 至 127 之间时，会直接使用缓存中的对象，而不是重新创建一个对象。

其中的 Javadoc 详细的说明了缓存支持 -128 到 127 之间的自动装箱过程。最大值 127 可以通过 `-XX:AutoBoxCacheMax=size` 修改。

实际上这个功能在 Java 5 中引入的时候,范围是固定的 -128 至 +127。后来在 Java 6 中，可以通过 `java.lang.Integer.IntegerCache.high` 设置最大值。

这使我们可以根据应用程序的实际情况灵活地调整来提高性能。到底是什么原因选择这个 -128 到 127 范围呢？因为这个范围的数字是最被广泛使用的。 在程序中，第一次使用 Integer 的时候也需要一定的额外时间来初始化这个缓存。

在 Boxing Conversion 部分的 Java 语言规范(JLS)规定如下：

如果一个变量 p 的值是：

- -128 至 127 之间的整数 (§3.10.1)
- true 和 false 的布尔值 (§3.10.3)
- `\u0000` 至 `\u007f` 之间的字符 (§3.10.4)

范围内的时，将 p 包装成 a 和 b 两个对象时，可以直接使用 a == b 判断 a 和 b 的值是否相等。

### 自动拆装箱带来的问题

当然，自动拆装箱是一个很好的功能，大大节省了开发人员的精力，不再需要关心到底什么时候需要拆装箱。但是，他也会引入一些问题。

> 包装对象的数值比较，不能简单的使用 `==`，虽然 -128 到 127 之间的数字可以，但是这个范围之外还是需要使用 `equals` 比较。
>
> 前面提到，有些场景会进行自动拆装箱，同时也说过，由于自动拆箱，如果包装类对象为 null ，那么自动拆箱时就有可能抛出 NPE。
>
> 如果一个 for 循环中有大量拆装箱操作，会浪费很多资源。

### 参考资料

[Java 的自动拆装箱][3]

 [1]: http://www.hollischuang.com/archives/435
 [2]: http://www.hollischuang.com/archives/1174
 [3]: https://www.jianshu.com/p/cc9312104876



## 2. Integer的缓存机制

英文原文：[Java Integer Cache][1] 翻译地址：[Java中整型的缓存机制][2] 原文作者：[Java Papers][3] 翻译作者：[Hollis][4] 转载请注明出处。

本文将介绍Java中Integer的缓存相关知识。这是在Java 5中引入的一个有助于节省内存、提高性能的功能。首先看一个使用Integer的示例代码，从中学习其缓存行为。接着我们将为什么这么实现以及他到底是如何实现的。你能猜出下面的Java程序的输出结果吗。如果你的结果和真正结果不一样，那么你就要好好看看本文了。

    package com.javapapers.java;
    
    public class JavaIntegerCache {
        public static void main(String... strings) {
    
            Integer integer1 = 3;
            Integer integer2 = 3;
    
            if (integer1 == integer2)
                System.out.println("integer1 == integer2");
            else
                System.out.println("integer1 != integer2");
    
            Integer integer3 = 300;
            Integer integer4 = 300;
    
            if (integer3 == integer4)
                System.out.println("integer3 == integer4");
            else
                System.out.println("integer3 != integer4");
    
        }
    }
    

我们普遍认为上面的两个判断的结果都是false。虽然比较的值是相等的，但是由于比较的是对象，而对象的引用不一样，所以会认为两个if判断都是false的。在Java中，`==`比较的是对象应用，而`equals`比较的是值。所以，在这个例子中，不同的对象有不同的引用，所以在进行比较的时候都将返回false。奇怪的是，这里两个类似的if条件判断返回不同的布尔值。

上面这段代码真正的输出结果：

    integer1 == integer2
    integer3 != integer4
    

### Java中Integer的缓存实现

在Java 5中，在Integer的操作上引入了一个新功能来节省内存和提高性能。整型对象通过使用相同的对象引用实现了缓存和重用。

> 适用于整数值区间-128 至 +127。
> 
> 只适用于自动装箱。使用构造函数创建对象不适用。

Java的编译器把基本数据类型自动转换成封装类对象的过程叫做`自动装箱`，相当于使用`valueOf`方法：

    Integer a = 10; //this is autoboxing
    Integer b = Integer.valueOf(10); //under the hood
    

现在我们知道了这种机制在源码中哪里使用了，那么接下来我们就看看JDK中的`valueOf`方法。下面是`JDK 1.8.0 build 25`的实现：

    /**
         * Returns an {@code Integer} instance representing the specified
         * {@code int} value.  If a new {@code Integer} instance is not
         * required, this method should generally be used in preference to
         * the constructor {@link #Integer(int)}, as this method is likely
         * to yield significantly better space and time performance by
         * caching frequently requested values.
         *
         * This method will always cache values in the range -128 to 127,
         * inclusive, and may cache other values outside of this range.
         *
         * @param  i an {@code int} value.
         * @return an {@code Integer} instance representing {@code i}.
         * @since  1.5
         */
        public static Integer valueOf(int i) {
            if (i >= IntegerCache.low && i <= IntegerCache.high)
                return IntegerCache.cache[i + (-IntegerCache.low)];
            return new Integer(i);
        }
    

在创建对象之前先从IntegerCache.cache中寻找。如果没找到才使用new新建对象。

### IntegerCache Class

IntegerCache是Integer类中定义的一个`private static`的内部类。接下来看看他的定义。

      /**
         * Cache to support the object identity semantics of autoboxing for values between
         * -128 and 127 (inclusive) as required by JLS.
         *
         * The cache is initialized on first usage.  The size of the cache
         * may be controlled by the {@code -XX:AutoBoxCacheMax=} option.
         * During VM initialization, java.lang.Integer.IntegerCache.high property
         * may be set and saved in the private system properties in the
         * sun.misc.VM class.
         */
    
        private static class IntegerCache {
            static final int low = -128;
            static final int high;
            static final Integer cache[];
    
            static {
                // high value may be configured by property
                int h = 127;
                String integerCacheHighPropValue =
                    sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
                if (integerCacheHighPropValue != null) {
                    try {
                        int i = parseInt(integerCacheHighPropValue);
                        i = Math.max(i, 127);
                        // Maximum array size is Integer.MAX_VALUE
                        h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                    } catch( NumberFormatException nfe) {
                        // If the property cannot be parsed into an int, ignore it.
                    }
                }
                high = h;
    
                cache = new Integer[(high - low) + 1];
                int j = low;
                for(int k = 0; k < cache.length; k++)
                    cache[k] = new Integer(j++);
    
                // range [-128, 127] must be interned (JLS7 5.1.7)
                assert IntegerCache.high >= 127;
            }
    
            private IntegerCache() {}
        }
    

其中的javadoc详细的说明了缓存支持-128到127之间的自动装箱过程。最大值127可以通过`-XX:AutoBoxCacheMax=size`修改。 缓存通过一个for循环实现。从低到高并创建尽可能多的整数并存储在一个整数数组中。这个缓存会在Integer类第一次被使用的时候被初始化出来。以后，就可以使用缓存中包含的实例对象，而不是创建一个新的实例(在自动装箱的情况下)。

实际上这个功能在Java 5中引入的时候,范围是固定的-128 至 +127。后来在Java 6中，可以通过`java.lang.Integer.IntegerCache.high`设置最大值。这使我们可以根据应用程序的实际情况灵活地调整来提高性能。到底是什么原因选择这个-128到127范围呢？因为这个范围的数字是最被广泛使用的。 在程序中，第一次使用Integer的时候也需要一定的额外时间来初始化这个缓存。

### Java语言规范中的缓存行为

在[Boxing Conversion][5]部分的Java语言规范(JLS)规定如下：

> 如果一个变量p的值是：
> 
> -128至127之间的整数(§3.10.1)
> 
> true 和 false的布尔值 (§3.10.3)
> 
> ‘\u0000’至 ‘\u007f’之间的字符(§3.10.4)
> 
> 中时，将p包装成a和b两个对象时，可以直接使用a==b判断a和b的值是否相等。

### 其他缓存的对象

这种缓存行为不仅适用于Integer对象。我们针对所有的整数类型的类都有类似的缓存机制。

> 有ByteCache用于缓存Byte对象
> 
> 有ShortCache用于缓存Short对象
> 
> 有LongCache用于缓存Long对象
> 
> 有CharacterCache用于缓存Character对象

`Byte`, `Short`, `Long`有固定范围: -128 到 127。对于`Character`, 范围是 0 到 127。除了`Integer`以外，这个范围都不能改变。

 [1]: http://javapapers.com/java/java-integer-cache/
 [2]: http://www.hollischuang.com/?p=1174
 [3]: http://javapapers.com/
 [4]: http://www.hollischuang.com
 [5]: http://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.1.7



## 3. 如何正确定义接口的返回值(boolean/Boolean)类型及命名(success/isSuccess)

在日常开发中，我们会经常要在类中定义布尔类型的变量，比如在给外部系统提供一个RPC接口的时候，我们一般会定义一个字段表示本次请求是否成功的。

关于这个"本次请求是否成功"的字段的定义，其实是有很多种讲究和坑的，稍有不慎就会掉入坑里，作者在很久之前就遇到过类似的问题，本文就来围绕这个简单分析一下。到底该如何定一个布尔类型的成员变量。

一般情况下，我们可以有以下四种方式来定义一个布尔类型的成员变量：
```
    boolean success
    boolean isSuccess
    Boolean success
    Boolean isSuccess
```

以上四种定义形式，你日常开发中最常用的是哪种呢？到底哪一种才是正确的使用姿势呢？

通过观察我们可以发现，前两种和后两种的主要区别是变量的类型不同，前者使用的是boolean，后者使用的是Boolean。

另外，第一种和第三种在定义变量的时候，变量命名是success，而另外两种使用isSuccess来命名的。

首先，我们来分析一下，到底应该是用success来命名，还是使用isSuccess更好一点。

### success 还是 isSuccess

到底应该是用success还是isSuccess来给变量命名呢？从语义上面来讲，两种命名方式都可以讲的通，并且也都没有歧义。那么还有什么原则可以参考来让我们做选择呢。

在阿里巴巴Java开发手册中关于这一点，有过一个『强制性』规定：

![-w656][1]￼

那么，为什么会有这样的规定呢？我们看一下POJO中布尔类型变量不同的命名有什么区别吧。
```
    class Model1  {
        private Boolean isSuccess;
        public void setSuccess(Boolean success) {
            isSuccess = success;
        }
        public Boolean getSuccess() {
            return isSuccess;
        }
     }
    
    class Model2 {
        private Boolean success;
        public Boolean getSuccess() {
            return success;
        }
        public void setSuccess(Boolean success) {
            this.success = success;
        }
    }
    
    class Model3 {
        private boolean isSuccess;
        public boolean isSuccess() {
            return isSuccess;
        }
        public void setSuccess(boolean success) {
            isSuccess = success;
        }
    }
    
    class Model4 {
        private boolean success;
        public boolean isSuccess() {
            return success;
        }
        public void setSuccess(boolean success) {
            this.success = success;
        }
    }
```
以上代码的setter/getter是使用Intellij IDEA自动生成的，仔细观察以上代码，你会发现以下规律：

*   基本类型自动生成的getter和setter方法，名称都是`isXXX()`和`setXXX()`形式的。
*   包装类型自动生成的getter和setter方法，名称都是`getXXX()`和`setXXX()`形式的。

既然，我们已经达成一致共识使用基本类型boolean来定义成员变量了，那么我们再来具体看下Model3和Model4中的setter/getter有何区别。

我们可以发现，虽然Model3和Model4中的成员变量的名称不同，一个是success，另外一个是isSuccess，但是他们自动生成的getter和setter方法名称都是`isSuccess`和`setSuccess`。

**Java Bean中关于setter/getter的规范**

关于Java Bean中的getter/setter方法的定义其实是有明确的规定的，根据[JavaBeans(TM) Specification][2]规定，如果是普通的参数propertyName，要以以下方式定义其setter/getter：
```
    public <PropertyType> get<PropertyName>();
    public void set<PropertyName>(<PropertyType> a);
```
但是，布尔类型的变量propertyName则是单独定义的：
```
    public boolean is<PropertyName>();
    public void set<PropertyName>(boolean m);
```

![-w687][3]￼

通过对照这份JavaBeans规范，我们发现，在Model4中，变量名为isSuccess，如果严格按照规范定义的话，他的getter方法应该叫isIsSuccess。但是很多IDE都会默认生成为isSuccess。

那这样做会带来什么问题呢。

在一般情况下，其实是没有影响的。但是有一种特殊情况就会有问题，那就是发生序列化的时候。

**序列化带来的影响**

关于序列化和反序列化请参考[Java对象的序列化与反序列化][4]。我们这里拿比较常用的JSON序列化来举例，看看看常用的fastJson、jackson和Gson之间有何区别：
```
    public class BooleanMainTest {
    
        public static void main(String[] args) throws IOException {
            //定一个Model3类型
            Model3 model3 = new Model3();
            model3.setSuccess(true);
    
            //使用fastjson(1.2.16)序列化model3成字符串并输出
            System.out.println("Serializable Result With fastjson :" + JSON.toJSONString(model3));
    
            //使用Gson(2.8.5)序列化model3成字符串并输出
            Gson gson =new Gson();
            System.out.println("Serializable Result With Gson :" +gson.toJson(model3));
    
            //使用jackson(2.9.7)序列化model3成字符串并输出
            ObjectMapper om = new ObjectMapper();
            System.out.println("Serializable Result With jackson :" +om.writeValueAsString(model3));
        }
    
    }
    
    class Model3 implements Serializable {
    
        private static final long serialVersionUID = 1836697963736227954L;
        private boolean isSuccess;
        public boolean isSuccess() {
            return isSuccess;
        }
        public void setSuccess(boolean success) {
            isSuccess = success;
        }
        public String getHollis(){
            return "hollischuang";
        }
    }
```

以上代码的Model3中，只有一个成员变量即isSuccess，三个方法，分别是IDE帮我们自动生成的isSuccess和setSuccess，另外一个是作者自己增加的一个符合getter命名规范的方法。

以上代码输出结果：
```
    Serializable Result With fastjson :{"hollis":"hollischuang","success":true}
    Serializable Result With Gson :{"isSuccess":true}
    Serializable Result With jackson :{"success":true,"hollis":"hollischuang"}
```

在fastjson和jackson的结果中，原来类中的isSuccess字段被序列化成success，并且其中还包含hollis值。而Gson中只有isSuccess字段。

我们可以得出结论：fastjson和jackson在把对象序列化成json字符串的时候，是通过反射遍历出该类中的所有getter方法，得到getHollis和isSuccess，然后根据JavaBeans规则，他会认为这是两个属性hollis和success的值。直接序列化成json:{"hollis":"hollischuang","success":true}

但是Gson并不是这么做的，他是通过反射遍历该类中的所有属性，并把其值序列化成json:{"isSuccess":true}

可以看到，由于不同的序列化工具，在进行序列化的时候使用到的策略是不一样的，所以，对于同一个类的同一个对象的序列化结果可能是不同的。

前面提到的关于对getHollis的序列化只是为了说明fastjson、jackson和Gson之间的序列化策略的不同，我们暂且把他放到一边，我们把他从Model3中删除后，重新执行下以上代码，得到结果：
```
    Serializable Result With fastjson :{"success":true}
    Serializable Result With Gson :{"isSuccess":true}
    Serializable Result With jackson :{"success":true}
```

现在，不同的序列化框架得到的json内容并不相同，如果对于同一个对象，我使用fastjson进行序列化，再使用Gson反序列化会发生什么？
```
    public class BooleanMainTest {
        public static void main(String[] args) throws IOException {
            Model3 model3 = new Model3();
            model3.setSuccess(true);
            Gson gson =new Gson();
            System.out.println(gson.fromJson(JSON.toJSONString(model3),Model3.class));
        }
    }
    
    
    class Model3 implements Serializable {
        private static final long serialVersionUID = 1836697963736227954L;
        private boolean isSuccess;
        public boolean isSuccess() {
            return isSuccess;
        }
        public void setSuccess(boolean success) {
            isSuccess = success;
        }
        @Override
        public String toString() {
            return new StringJoiner(", ", Model3.class.getSimpleName() + "[", "]")
                .add("isSuccess=" + isSuccess)
                .toString();
        }
    }
```
以上代码，输出结果：
```
    Model3[isSuccess=false]
```

这和我们预期的结果完全相反，原因是因为JSON框架通过扫描所有的getter后发现有一个isSuccess方法，然后根据JavaBeans的规范，解析出变量名为success，把model对象序列化城字符串后内容为`{"success":true}`。

根据`{"success":true}`这个json串，Gson框架在通过解析后，通过反射寻找Model类中的success属性，但是Model类中只有isSuccess属性，所以，最终反序列化后的Model类的对象中，isSuccess则会使用默认值false。

但是，一旦以上代码发生在生产环境，这绝对是一个致命的问题。

所以，作为开发者，我们应该想办法尽量避免这种问题的发生，对于POJO的设计者来说，只需要做简单的一件事就可以解决这个问题了，那就是把isSuccess改为success。这样，该类里面的成员变量时success，getter方法是isSuccess，这是完全符合JavaBeans规范的。无论哪种序列化框架，执行结果都一样。就从源头避免了这个问题。

引用以下R大关于阿里巴巴Java开发手册这条规定的评价（https://www.zhihu.com/question/55642203 ）：

![-w665][5]￼

所以，**在定义POJO中的布尔类型的变量时，不要使用isSuccess这种形式，而要直接使用success！**

### Boolean还是boolean

前面我们介绍完了在success和isSuccess之间如何选择，那么排除错误答案后，备选项还剩下：
```
    boolean success
    Boolean success
```
那么，到底应该是用Boolean还是boolean来给定一个布尔类型的变量呢？

我们知道，boolean是基本数据类型，而Boolean是包装类型。关于基本数据类型和包装类之间的关系和区别请参考[一文读懂什么是Java中的自动拆装箱][6]

那么，在定义一个成员变量的时候到底是使用包装类型更好还是使用基本数据类型呢？

我们来看一段简单的代码
```
     /**
     * @author Hollis
     */
    public class BooleanMainTest {
        public static void main(String[] args) {
            Model model1 = new Model();
            System.out.println("default model : " + model1);
        }
    }
    
    class Model {
        /**
         * 定一个Boolean类型的success成员变量
         */
        private Boolean success;
        /**
         * 定一个boolean类型的failure成员变量
         */
        private boolean failure;
    
        /**
         * 覆盖toString方法，使用Java 8 的StringJoiner
         */
        @Override
        public String toString() {
            return new StringJoiner(", ", Model.class.getSimpleName() + "[", "]")
                .add("success=" + success)
                .add("failure=" + failure)
                .toString();
        }
    }
```
以上代码输出结果为：
```
    default model : Model[success=null, failure=false]
``` 

可以看到，当我们没有设置Model对象的字段的值的时候，Boolean类型的变量会设置默认值为`null`，而boolean类型的变量会设置默认值为`false`。

即对象的默认值是`null`，boolean基本数据类型的默认值是`false`。

在阿里巴巴Java开发手册中，对于POJO中如何选择变量的类型也有着一些规定：

<img src="http://www.hollischuang.com/wp-content/uploads/2018/12/640.jpeg" alt="" width="1080" height="445" class="aligncenter size-full wp-image-3558" />

这里建议我们使用包装类型，原因是什么呢？

举一个扣费的例子，我们做一个扣费系统，扣费时需要从外部的定价系统中读取一个费率的值，我们预期该接口的返回值中会包含一个浮点型的费率字段。当我们取到这个值得时候就使用公式：金额*费率=费用 进行计算，计算结果进行划扣。

如果由于计费系统异常，他可能会返回个默认值，如果这个字段是Double类型的话，该默认值为null，如果该字段是double类型的话，该默认值为0.0。

如果扣费系统对于该费率返回值没做特殊处理的话，拿到null值进行计算会直接报错，阻断程序。拿到0.0可能就直接进行计算，得出接口为0后进行扣费了。这种异常情况就无法被感知。

这种使用包装类型定义变量的方式，通过异常来阻断程序，进而可以被识别到这种线上问题。如果使用基本数据类型的话，系统可能不会报错，进而认为无异常。

**以上，就是建议在POJO和RPC的返回值中使用包装类型的原因。**

但是关于这一点，作者之前也有过不同的看法：对于布尔类型的变量，我认为可以和其他类型区分开来，作者并不认为使用null进而导致NPE是一种最好的实践。因为布尔类型只有true/false两种值，我们完全可以和外部调用方约定好当返回值为false时的明确语义。

后来，作者单独和《阿里巴巴Java开发手册》、《码出高效》的作者——孤尽 单独1V1(qing) Battle(jiao)了一下。最终达成共识，还是**尽量使用包装类型**。

**但是，作者还是想强调一个我的观点，尽量避免在你的代码中出现不确定的null值。**


### 总结

本文围绕布尔类型的变量定义的类型和命名展开了介绍，最终我们可以得出结论，在定义一个布尔类型的变量，尤其是一个给外部提供的接口返回值时，要使用success来命名，阿里巴巴Java开发手册建议使用封装类来定义POJO和RPC返回值中的变量。但是这不意味着可以随意的使用null，我们还是要尽量避免出现对null的处理的。

 [1]: http://www.hollischuang.com/wp-content/uploads/2018/12/15449439364854.jpg
 [2]: https://download.oracle.com/otndocs/jcp/7224-javabeans-1.01-fr-spec-oth-JSpec/
 [3]: http://www.hollischuang.com/wp-content/uploads/2018/12/15449455942045.jpg
 [4]: http://www.hollischuang.com/archives/1150
 [5]: http://www.hollischuang.com/wp-content/uploads/2018/12/15449492627754.jpg
 [6]: http://www.hollischuang.com/archives/2700
 [7]: http://www.hollischuang.com/archives/883
 [8]: http://www.hollischuang.com/archives/74
 [9]: http://www.hollischuang.com/wp-content/uploads/2018/12/15449430847727.jpg


## String相关

## 1. 字符串的不可变性

### 定义一个字符串

    String s = "abcd";
    

![String-Immutability-1][1]

`s`中保存了string对象的引用。下面的箭头可以理解为“存储他的引用”。

### 使用变量来赋值变量

    String s2 = s;
    

![String-Immutability-2][2]

s2保存了相同的引用值，因为他们代表同一个对象。

### 字符串连接

    s = s.concat("ef");
    

![string-immutability][3]

`s`中保存的是一个重新创建出来的string对象的引用。

### 总结

一旦一个string对象在内存(堆)中被创建出来，他就无法被修改。特别要注意的是，String类的所有方法都没有改变字符串本身的值，都是返回了一个新的对象。

如果你需要一个可修改的字符串，应该使用StringBuffer 或者 StringBuilder。否则会有大量时间浪费在垃圾回收上，因为每次试图修改都有新的string对象被创建出来。

 [1]: http://www.programcreek.com/wp-content/uploads/2009/02/String-Immutability-1.jpeg
 [2]: http://www.programcreek.com/wp-content/uploads/2009/02/String-Immutability-2.jpeg
 [3]: http://www.programcreek.com/wp-content/uploads/2009/02/string-immutability-650x279.jpeg


## 2. JDK 6和JDK 7中substring的原理及区别

String是Java中一个比较基础的类，每一个开发人员都会经常接触到。而且，String也是面试中经常会考的知识点。String有很多方法，有些方法比较常用，有些方法不太常用。今天要介绍的substring就是一个比较常用的方法，而且围绕substring也有很多面试题。

`substring(int beginIndex, int endIndex)`方法在不同版本的JDK中的实现是不同的。了解他们的区别可以帮助你更好的使用他。为简单起见，后文中用`substring()`代表`substring(int beginIndex, int endIndex)`方法。

### substring() 的作用

`substring(int beginIndex, int endIndex)`方法截取字符串并返回其[beginIndex,endIndex-1]范围内的内容。

    String x = "abcdef";
    x = x.substring(1,3);
    System.out.println(x);
    

输出内容：

    bc
    

### 调用substring()时发生了什么？

你可能知道，因为x是不可变的，当使用`x.substring(1,3)`对x赋值的时候，它会指向一个全新的字符串：

![string-immutability1][1]

然而，这个图不是完全正确的表示堆中发生的事情。因为在jdk6 和 jdk7中调用substring时发生的事情并不一样。

### JDK 6中的substring

String是通过字符数组实现的。在jdk 6 中，String类包含三个成员变量：`char value[]`， `int offset`，`int count`。他们分别用来存储真正的字符数组，数组的第一个位置索引以及字符串中包含的字符个数。

当调用substring方法的时候，会创建一个新的string对象，但是这个string的值仍然指向堆中的同一个字符数组。这两个对象中只有count和offset 的值是不同的。

![string-substring-jdk6][2]

下面是证明上说观点的Java源码中的关键代码：

    //JDK 6
    String(int offset, int count, char value[]) {
        this.value = value;
        this.offset = offset;
        this.count = count;
    }
    
    public String substring(int beginIndex, int endIndex) {
        //check boundary
        return  new String(offset + beginIndex, endIndex - beginIndex, value);
    }
    

### JDK 6中的substring导致的问题

如果你有一个很长很长的字符串，但是当你使用substring进行切割的时候你只需要很短的一段。这可能导致性能问题，因为你需要的只是一小段字符序列，但是你却引用了整个字符串（因为这个非常长的字符数组一直在被引用，所以无法被回收，就可能导致内存泄露）。在JDK 6中，一般用以下方式来解决该问题，原理其实就是生成一个新的字符串并引用他。

    x = x.substring(x, y) + ""
    

关于JDK 6中subString的使用不当会导致内存系列已经被官方记录在Java Bug Database中：

<img src="http://www.hollischuang.com/wp-content/uploads/2016/03/leak.png" alt="leak" width="1089" height="744" class="aligncenter size-full wp-image-2660" />

> 内存泄露：在计算机科学中，内存泄漏指由于疏忽或错误造成程序未能释放已经不再使用的内存。 内存泄漏并非指内存在物理上的消失，而是应用程序分配某段内存后，由于设计错误，导致在释放该段内存之前就失去了对该段内存的控制，从而造成了内存的浪费。

### JDK 7 中的substring

上面提到的问题，在jdk 7中得到解决。在jdk 7 中，substring方法会在堆内存中创建一个新的数组。

![string-substring-jdk7][3]

Java源码中关于这部分的主要代码如下：

    //JDK 7
    public String(char value[], int offset, int count) {
        //check boundary
        this.value = Arrays.copyOfRange(value, offset, offset + count);
    }
    
    public String substring(int beginIndex, int endIndex) {
        //check boundary
        int subLen = endIndex - beginIndex;
        return new String(value, beginIndex, subLen);
    }
    

以上是JDK 7中的subString方法，其使用`new String`创建了一个新字符串，避免对老字符串的引用。从而解决了内存泄露问题。

所以，如果你的生产环境中使用的JDK版本小于1.7，当你使用String的subString方法时一定要注意，避免内存泄露。

 [1]: http://www.programcreek.com/wp-content/uploads/2013/09/string-immutability1-650x303.jpeg
 [2]: http://www.programcreek.com/wp-content/uploads/2013/09/string-substring-jdk6-650x389.jpeg
 [3]: http://www.programcreek.com/wp-content/uploads/2013/09/string-substring-jdk71-650x389.jpeg


## 3. replaceFirst、replaceAll、replace有什么区别？

replace、replaceAll和replaceFirst是Java中常用的替换字符的方法,它们的方法定义是：

replace(CharSequence target, CharSequence replacement) ，用replacement替换所有的target，两个参数都是字符串。

replaceAll(String regex, String replacement) ，用replacement替换所有的regex匹配项，regex很明显是个正则表达式，replacement是字符串。

replaceFirst(String regex, String replacement) ，基本和replaceAll相同，区别是只替换第一个匹配项。

可以看到，其中replaceAll以及replaceFirst是和正则表达式有关的，而replace和正则表达式无关。

replaceAll和replaceFirst的区别主要是替换的内容不同，replaceAll是替换所有匹配的字符，而replaceFirst()仅替换第一次出现的字符

### 用法例子

一以下例子参考：http://www.51gjie.com/java/771.html

1. replaceAll() 替换符合正则的所有文字

```
//文字替换（全部） 
Pattern pattern = Pattern.compile("正则表达式"); 
Matcher matcher = pattern.matcher("正则表达式 Hello World,正则表达式 Hello World"); 
//替换第一个符合正则的数据 
System.out.println(matcher.replaceAll("Java")); 

```
   

2. replaceFirst() 替换第一个符合正则的数据

```
//文字替换（首次出现字符） 
Pattern pattern = Pattern.compile("正则表达式"); 
Matcher matcher = pattern.matcher("正则表达式 Hello World,正则表达式 Hello World"); 
//替换第一个符合正则的数据 
System.out.println(matcher.replaceFirst("Java")); 
    
```
    
3. replaceAll()替换所有html标签

```
//去除html标记 
Pattern pattern = Pattern.compile("<.+?>", Pattern.DOTALL); 
Matcher matcher = pattern.matcher("<a href=\"index.html\">主页</a>"); 
String string = matcher.replaceAll(""); 
System.out.println(string); 

```

4. replaceAll() 替换指定文字 

```
//替换指定{}中文字 
String str = "Java目前的发展史是由{0}年-{1}年";
String[][] object = {
    new String[] {
        "\\{0\\}",
        "1995"
    },
    new String[] {
        "\\{1\\}",
        "2007"
    }
};
System.out.println(replace(str, object));
public static String replace(final String sourceString, Object[] object) {
    String temp = sourceString;
    for (int i = 0; i < object.length; i++) {
        String[] result = (String[]) object[i];
        Pattern pattern = Pattern.compile(result[0]);
        Matcher matcher = pattern.matcher(temp);
        temp = matcher.replaceAll(result[1]);
    }
    return temp;
}

```

5. replace()替换字符串

```
System.out.println("abac".replace("a", "\\a")); //\ab\ac 
```

## 4. String对“+”的重载

String s = "a" + "b"，编译器会进行常量折叠(因为两个都是编译期常量，编译期可知)，即变成 String s = "ab"

对于能够进行优化的(String s = "a" + 变量 等)用 StringBuilder 的 append() 方法替代，最后调用 toString() 方法 (底层就是一个 new String())

## 5. 字符串拼接的几种方式和区别

字符串，是Java中最常用的一个数据类型了。

本文，也是对于Java中字符串相关知识的一个补充，主要来介绍一下字符串拼接相关的知识。本文基于jdk1.8.0_181。

### 字符串拼接 

字符串拼接是我们在Java代码中比较经常要做的事情，就是把多个字符串拼接到一起。

我们都知道，**String是Java中一个不可变的类**，所以他一旦被实例化就无法被修改。

> 不可变类的实例一旦创建，其成员变量的值就不能被修改。这样设计有很多好处，比如可以缓存hashcode、使用更加便利以及更加安全等。

但是，既然字符串是不可变的，那么字符串拼接又是怎么回事呢？

**字符串不变性与字符串拼接**

其实，所有的所谓字符串拼接，都是重新生成了一个新的字符串。下面一段字符串拼接代码：

<pre><code class="language-text">String s = "abcd";
s = s.concat("ef");
</code></pre>

其实最后我们得到的s已经是一个新的字符串了。如下图

![][8]￼

s中保存的是一个重新创建出来的String对象的引用。

那么，在Java中，到底如何进行字符串拼接呢？字符串拼接有很多种方式，这里简单介绍几种比较常用的。

**使用`+`拼接字符串**

在Java中，拼接字符串最简单的方式就是直接使用符号`+`来拼接。如：

<pre><code class="language-text">String wechat = "Hollis";
String introduce = "每日更新Java相关技术文章";
String hollis = wechat + "," + introduce;
</code></pre>

这里要特别说明一点，有人把Java中使用`+`拼接字符串的功能理解为**运算符重载**。其实并不是，**Java是不支持运算符重载的**。这其实只是Java提供的一个**语法糖**。后面再详细介绍。

> 运算符重载：在计算机程序设计中，运算符重载（英语：operator overloading）是多态的一种。运算符重载，就是对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型。
> 
> 语法糖：语法糖（Syntactic sugar），也译为糖衣语法，是由英国计算机科学家彼得·兰丁发明的一个术语，指计算机语言中添加的某种语法，这种语法对语言的功能没有影响，但是更方便程序员使用。语法糖让程序更加简洁，有更高的可读性。

**concat**  
除了使用`+`拼接字符串之外，还可以使用String类中的方法concat方法来拼接字符串。如：

<pre><code class="language-text">String wechat = "Hollis";
String introduce = "每日更新Java相关技术文章";
String hollis = wechat.concat(",").concat(introduce);
</code></pre>

**StringBuffer**

关于字符串，Java中除了定义了一个可以用来定义**字符串常量**的`String`类以外，还提供了可以用来定义**字符串变量**的`StringBuffer`类，它的对象是可以扩充和修改的。

使用`StringBuffer`可以方便的对字符串进行拼接。如：

<pre><code class="language-text">StringBuffer wechat = new StringBuffer("Hollis");
String introduce = "每日更新Java相关技术文章";
StringBuffer hollis = wechat.append(",").append(introduce);
</code></pre>

**StringBuilder**  
除了`StringBuffer`以外，还有一个类`StringBuilder`也可以使用，其用法和`StringBuffer`类似。如：

<pre><code class="language-text">StringBuilder wechat = new StringBuilder("Hollis");
String introduce = "每日更新Java相关技术文章";
StringBuilder hollis = wechat.append(",").append(introduce);
</code></pre>

**StringUtils.join**  
除了JDK中内置的字符串拼接方法，还可以使用一些开源类库中提供的字符串拼接方法名，如`apache.commons中`提供的`StringUtils`类，其中的`join`方法可以拼接字符串。

<pre><code class="language-text">String wechat = "Hollis";
String introduce = "每日更新Java相关技术文章";
System.out.println(StringUtils.join(wechat, ",", introduce));
</code></pre>

这里简单说一下，StringUtils中提供的join方法，最主要的功能是：将数组或集合以某拼接符拼接到一起形成新的字符串，如：

    String []list  ={"Hollis","每日更新Java相关技术文章"};
    String result= StringUtils.join(list,",");
    System.out.println(result);
    //结果：Hollis,每日更新Java相关技术文章
    

并且，Java8中的String类中也提供了一个静态的join方法，用法和StringUtils.join类似。

以上就是比较常用的五种在Java种拼接字符串的方式，那么到底哪种更好用呢？为什么阿里巴巴Java开发手册中不建议在循环体中使用`+`进行字符串拼接呢？

<img src="https://www.hollischuang.com/wp-content/uploads/2019/01/15472850170230.jpg" alt="" style="width:917px" />￼

(阿里巴巴Java开发手册中关于字符串拼接的规约)

### 使用`+`拼接字符串的实现原理

前面提到过，使用`+`拼接字符串，其实只是Java提供的一个语法糖， 那么，我们就来解一解这个语法糖，看看他的内部原理到底是如何实现的。

还是这样一段代码。我们把他生成的字节码进行反编译，看看结果。

<pre><code class="language-text">String wechat = "Hollis";
String introduce = "每日更新Java相关技术文章";
String hollis = wechat + "," + introduce;
</code></pre>

反编译后的内容如下，反编译工具为jad。

<pre><code class="language-text">String wechat = "Hollis";
String introduce = "\u6BCF\u65E5\u66F4\u65B0Java\u76F8\u5173\u6280\u672F\u6587\u7AE0";//每日更新Java相关技术文章
String hollis = (new StringBuilder()).append(wechat).append(",").append(introduce).toString();
</code></pre>

通过查看反编译以后的代码，我们可以发现，原来字符串常量在拼接过程中，是将String转成了StringBuilder后，使用其append方法进行处理的。

那么也就是说，Java中的`+`对字符串的拼接，其实现原理是使用`StringBuilder.append`。

### concat是如何实现的

我们再来看一下concat方法的源代码，看一下这个方法又是如何实现的。

<pre><code class="language-text">public String concat(String str) {
    int otherLen = str.length();
    if (otherLen == 0) {
        return this;
    }
    int len = value.length;
    char buf[] = Arrays.copyOf(value, len + otherLen);
    str.getChars(buf, len);
    return new String(buf, true);
}
</code></pre>

这段代码首先创建了一个字符数组，长度是已有字符串和待拼接字符串的长度之和，再把两个字符串的值复制到新的字符数组中，并使用这个字符数组创建一个新的String对象并返回。

通过源码我们也可以看到，经过concat方法，其实是new了一个新的String，这也就呼应到前面我们说的字符串的不变性问题上了。

### StringBuffer和StringBuilder

接下来我们看看`StringBuffer`和`StringBuilder`的实现原理。

和`String`类类似，`StringBuilder`类也封装了一个字符数组，定义如下：

<pre><code class="language-text">char[] value;
</code></pre>

与`String`不同的是，它并不是`final`的，所以他是可以修改的。另外，与`String`不同，字符数组中不一定所有位置都已经被使用，它有一个实例变量，表示数组中已经使用的字符个数，定义如下：

<pre><code class="language-text">int count;
</code></pre>

其append源码如下：

<pre><code class="language-text">public StringBuilder append(String str) {
    super.append(str);
    return this;
}
</code></pre>

该类继承了`AbstractStringBuilder`类，看下其`append`方法：

<pre><code class="language-text">public AbstractStringBuilder append(String str) {
    if (str == null)
        return appendNull();
    int len = str.length();
    ensureCapacityInternal(count + len);
    str.getChars(0, len, value, count);
    count += len;
    return this;
}
</code></pre>

append会直接拷贝字符到内部的字符数组中，如果字符数组长度不够，会进行扩展。

`StringBuffer`和`StringBuilder`类似，最大的区别就是`StringBuffer`是线程安全的，看一下`StringBuffer`的`append`方法。

<pre><code class="language-text">public synchronized StringBuffer append(String str) {
    toStringCache = null;
    super.append(str);
    return this;
}
</code></pre>

该方法使用`synchronized`进行声明，说明是一个线程安全的方法。而`StringBuilder`则不是线程安全的。

### StringUtils.join是如何实现的

通过查看`StringUtils.join`的源代码，我们可以发现，其实他也是通过`StringBuilder`来实现的。

<pre><code class="language-text">public static String join(final Object[] array, String separator, final int startIndex, final int endIndex) {
    if (array == null) {
        return null;
    }
    if (separator == null) {
        separator = EMPTY;
    }

    // endIndex - startIndex &gt; 0:   Len = NofStrings *(len(firstString) + len(separator))
    //           (Assuming that all Strings are roughly equally long)
    final int noOfItems = endIndex - startIndex;
    if (noOfItems &lt;= 0) {
        return EMPTY;
    }

    final StringBuilder buf = new StringBuilder(noOfItems * 16);

    for (int i = startIndex; i &lt; endIndex; i++) {
        if (i &gt; startIndex) {
            buf.append(separator);
        }
        if (array[i] != null) {
            buf.append(array[i]);
        }
    }
    return buf.toString();
}
</code></pre>

### 效率比较

既然有这么多种字符串拼接的方法，那么到底哪一种效率最高呢？我们来简单对比一下。

<pre><code class="language-text">long t1 = System.currentTimeMillis();
//这里是初始字符串定义
for (int i = 0; i &lt; 50000; i++) {
    //这里是字符串拼接代码
}
long t2 = System.currentTimeMillis();
System.out.println("cost:" + (t2 - t1));
</code></pre>

我们使用形如以上形式的代码，分别测试下五种字符串拼接代码的运行时间。得到结果如下：

<pre><code class="language-text">+ cost:5119
StringBuilder cost:3
StringBuffer cost:4
concat cost:3623
StringUtils.join cost:25726
</code></pre>

从结果可以看出，用时从短到长的对比是：

`StringBuilder`<`StringBuffer`<`concat`<`+`<`StringUtils.join`

`StringBuffer`在`StringBuilder`的基础上，做了同步处理，所以在耗时上会相对多一些。

StringUtils.join也是使用了StringBuilder，并且其中还是有很多其他操作，所以耗时较长，这个也容易理解。其实StringUtils.join更擅长处理字符串数组或者列表的拼接。

那么问题来了，前面我们分析过，其实使用`+`拼接字符串的实现原理也是使用的`StringBuilder`，那为什么结果相差这么多，高达1000多倍呢？

我们再把以下代码反编译下：

<pre><code class="language-text">long t1 = System.currentTimeMillis();
String str = "hollis";
for (int i = 0; i &lt; 50000; i++) {
    String s = String.valueOf(i);
    str += s;
}
long t2 = System.currentTimeMillis();
System.out.println("+ cost:" + (t2 - t1));
</code></pre>

反编译后代码如下：

<pre><code class="language-text">long t1 = System.currentTimeMillis();
String str = "hollis";
for(int i = 0; i &lt; 50000; i++)
{
    String s = String.valueOf(i);
    str = (new StringBuilder()).append(str).append(s).toString();
}

long t2 = System.currentTimeMillis();
System.out.println((new StringBuilder()).append("+ cost:").append(t2 - t1).toString());
</code></pre>

我们可以看到，反编译后的代码，在`for`循环中，每次都是`new`了一个`StringBuilder`，然后再把`String`转成`StringBuilder`，再进行`append`。

而频繁的新建对象当然要耗费很多时间了，不仅仅会耗费时间，频繁的创建对象，还会造成内存资源的浪费。

所以，阿里巴巴Java开发手册建议：循环体内，字符串的连接方式，使用 `StringBuilder` 的 `append` 方法进行扩展。而不要使用`+`。

### 总结 

本文介绍了什么是字符串拼接，虽然字符串是不可变的，但是还是可以通过新建字符串的方式来进行字符串的拼接。

常用的字符串拼接方式有五种，分别是使用`+`、使用`concat`、使用`StringBuilder`、使用`StringBuffer`以及使用`StringUtils.join`。

由于字符串拼接过程中会创建新的对象，所以如果要在一个循环体中进行字符串拼接，就要考虑内存问题和效率问题。

因此，经过对比，我们发现，直接使用`StringBuilder`的方式是效率最高的。因为`StringBuilder`天生就是设计来定义可变字符串和字符串的变化操作的。

但是，还要强调的是：

1、如果不是在循环体中进行字符串拼接的话，直接使用`+`就好了。

2、如果在并发场景中进行字符串拼接的话，要使用`StringBuffer`来代替`StringBuilder`。

 [1]: http://www.hollischuang.com/archives/99
 [2]: http://www.hollischuang.com/archives/1249
 [3]: http://www.hollischuang.com/archives/2517
 [4]: http://www.hollischuang.com/archives/1230
 [5]: http://www.hollischuang.com/archives/1246
 [6]: http://www.hollischuang.com/archives/1232
 [7]: http://www.hollischuang.com/archives/61
 [8]: https://www.hollischuang.com/wp-content/uploads/2019/01/15472897908391.jpg


## 6. String.valueOf和Integer.toString的区别

我们有三种方式将一个int类型的变量变成呢过String类型，那么他们有什么区别？

    1.int i = 5;
    2.String i1 = "" + i;
    3.String i2 = String.valueOf(i);
    4.String i3 = Integer.toString(i);

第三行和第四行没有任何区别，因为String.valueOf(i)也是调用Integer.toString(i)来实现的。

第二行代码其实是String i1 = (new StringBuilder()).append(i).toString();，首先创建一个StringBuilder对象，然后再调用append方法，再调用toString方法。



## 7. switch对String的支持

Java 7中，switch的参数可以是String类型了，这对我们来说是一个很方便的改进。到目前为止switch支持这样几种数据类型：`byte` `short` `int` `char` `String` 。但是，作为一个程序员我们不仅要知道他有多么好用，还要知道它是如何实现的，switch对整型的支持是怎么实现的呢？对字符型是怎么实现的呢？String类型呢？有一点Java开发经验的人这个时候都会猜测switch对String的支持是使用equals()方法和hashcode()方法。那么到底是不是这两个方法呢？接下来我们就看一下，switch到底是如何实现的。

### 一、switch对整型支持的实现

下面是一段很简单的Java代码，定义一个int型变量a，然后使用switch语句进行判断。执行这段代码输出内容为5，那么我们将下面这段代码反编译，看看他到底是怎么实现的。
```
    public class switchDemoInt {
        public static void main(String[] args) {
            int a = 5;
            switch (a) {
            case 1:
                System.out.println(1);
                break;
            case 5:
                System.out.println(5);
                break;
            default:
                break;
            }
        }
    }
    //output 5

```
反编译后的代码如下：
```
    public class switchDemoInt
    {
        public switchDemoInt()
        {
        }
        public static void main(String args[])
        {
            int a = 5;
            switch(a)
            {
            case 1: // '\001'
                System.out.println(1);
                break;

            case 5: // '\005'
                System.out.println(5);
                break;
            }
        }
    }
```

我们发现，反编译后的代码和之前的代码比较除了多了两行注释以外没有任何区别，那么我们就知道，**switch对int的判断是直接比较整数的值**。

### 二、switch对字符型支持的实现

直接上代码：
```
    public class switchDemoInt {
        public static void main(String[] args) {
            char a = 'b';
            switch (a) {
            case 'a':
                System.out.println('a');
                break;
            case 'b':
                System.out.println('b');
                break;
            default:
                break;
            }
        }
    }

```
编译后的代码如下： 
```
public class switchDemoChar

    public class switchDemoChar
    {
        public switchDemoChar()
        {
        }
        public static void main(String args[])
        {
            char a = 'b';
            switch(a)
            {
            case 97: // 'a'
                System.out.println('a');
                break;
            case 98: // 'b'
                System.out.println('b');
                break;
            }
      }
    }
```

通过以上的代码作比较我们发现：对char类型进行比较的时候，实际上比较的是ascii码，编译器会把char型变量转换成对应的int型变量

### 三、switch对字符串支持的实现

还是先上代码：

    public class switchDemoString {
        public static void main(String[] args) {
            String str = "world";
            switch (str) {
            case "hello":
                System.out.println("hello");
                break;
            case "world":
                System.out.println("world");
                break;
            default:
                break;
            }
        }
    }


对代码进行反编译：

    public class switchDemoString
    {
        public switchDemoString()
        {
        }
        public static void main(String args[])
        {
            String str = "world";
            String s;
            switch((s = str).hashCode())
            {
            default:
                break;
            case 99162322:
                if(s.equals("hello"))
                    System.out.println("hello");
                break;
            case 113318802:
                if(s.equals("world"))
                    System.out.println("world");
                break;
            }
        }
    }


看到这个代码，你知道原来字符串的switch是通过`equals()`和`hashCode()`方法来实现的。**记住，switch中只能使用整型**，比如`byte`。`short`，`char`(ackii码是整型)以及`int`。还好`hashCode()`方法返回的是`int`，而不是`long`。通过这个很容易记住`hashCode`返回的是`int`这个事实。仔细看下可以发现，进行`switch`的实际是哈希值，然后通过使用equals方法比较进行安全检查，这个检查是必要的，因为哈希可能会发生碰撞。因此它的性能是不如使用枚举进行switch或者使用纯整数常量，但这也不是很差。因为Java编译器只增加了一个`equals`方法，如果你比较的是字符串字面量的话会非常快，比如”abc” ==”abc”。如果你把`hashCode()`方法的调用也考虑进来了，那么还会再多一次的调用开销，因为字符串一旦创建了，它就会把哈希值缓存起来。因此如果这个`switch`语句是用在一个循环里的，比如逐项处理某个值，或者游戏引擎循环地渲染屏幕，这里`hashCode()`方法的调用开销其实不会很大。

好，以上就是关于switch对整型、字符型、和字符串型的支持的实现方式，总结一下我们可以发现，**其实switch只支持一种数据类型，那就是整型，其他数据类型都是转换成整型之后在使用switch的。**


## 8. 字符串池

字符串大家一定都不陌生，他是我们非常常用的一个类。
 
String作为一个Java类，可以通过以下两种方式创建一个字符串：
 
 
    String str = "Hollis";
    
    String str = new String("Hollis")；
    
 
而第一种是我们比较常用的做法，这种形式叫做"字面量"。
 
在JVM中，为了减少相同的字符串的重复创建，为了达到节省内存的目的。会单独开辟一块内存，用于保存字符串常量，这个内存区域被叫做字符串常量池。
 
当代码中出现双引号形式（字面量）创建字符串对象时，JVM 会先对这个字符串进行检查，如果字符串常量池中存在相同内容的字符串对象的引用，则将这个引用返回；否则，创建新的字符串对象，然后将这个引用放入字符串常量池，并返回该引用。
 
这种机制，就是字符串驻留或池化。
 

### 字符串常量池的位置

在JDK 7以前的版本中，字符串常量池是放在永久代中的。

因为按照计划，JDK会在后续的版本中通过元空间来代替永久代，所以首先在JDK 7中，将字符串常量池先从永久代中移出，暂时放到了堆内存中。

在JDK 8中，彻底移除了永久代，使用元空间替代了永久代，于是字符串常量池再次从堆内存移动到永久代中

## 9. Class常量池

在Java中，常量池的概念想必很多人都听说过。这也是面试中比较常考的题目之一。在Java有关的面试题中，一般习惯通过String的有关问题来考察面试者对于常量池的知识的理解，几道简单的String面试题难倒了无数的开发者。所以说，常量池是Java体系中一个非常重要的概念。

谈到常量池，在Java体系中，共用三种常量池。分别是**字符串常量池**、**Class常量池**和**运行时常量池**。

本文先来介绍一下到底什么是Class常量池。

### 什么是Class文件

在[Java代码的编译与反编译那些事儿][1]中我们介绍过Java的编译和反编译的概念。我们知道，计算机只认识0和1，所以程序员写的代码都需要经过编译成0和1构成的二进制格式才能够让计算机运行。

我们在《[深入分析Java的编译原理][2]》中提到过，为了让Java语言具有良好的跨平台能力，Java独具匠心的提供了一种可以在所有平台上都能使用的一种中间代码——字节码（ByteCode）。

有了字节码，无论是哪种平台（如Windows、Linux等），只要安装了虚拟机，都可以直接运行字节码。

同样，有了字节码，也解除了Java虚拟机和Java语言之间的耦合。这话可能很多人不理解，Java虚拟机不就是运行Java语言的么？这种解耦指的是什么？

其实，目前Java虚拟机已经可以支持很多除Java语言以外的语言了，如Groovy、JRuby、Jython、Scala等。之所以可以支持，就是因为这些语言也可以被编译成字节码。而虚拟机并不关心字节码是有哪种语言编译而来的。

Java语言中负责编译出字节码的编译器是一个命令是`javac`。

> javac是收录于JDK中的Java语言编译器。该工具可以将后缀名为.java的源文件编译为后缀名为.class的可以运行于Java虚拟机的字节码。

如，我们有以下简单的`HelloWorld.java`代码：

    public class HelloWorld {
        public static void main(String[] args) {
            String s = "Hollis";
        }
    }
    

通过javac命令生成class文件：

    javac HelloWorld.java
    

生成`HelloWorld.class`文件:

![][3]￼

> 如何使用16进制打开class文件：使用 `vim test.class` ，然后在交互模式下，输入`:%!xxd` 即可。

可以看到，上面的文件就是Class文件，Class文件中包含了Java虚拟机指令集和符号表以及若干其他辅助信息。

要想能够读懂上面的字节码，需要了解Class类文件的结构，由于这不是本文的重点，这里就不展开说明了。

> 读者可以看到，`HelloWorld.class`文件中的前八个字母是`cafe babe`，这就是Class文件的魔数（[Java中的”魔数”][4]）

我们需要知道的是，在Class文件的4个字节的魔数后面的分别是4个字节的Class文件的版本号（第5、6个字节是次版本号，第7、8个字节是主版本号，我生成的Class文件的版本号是52，这时Java 8对应的版本。也就是说，这个版本的字节码，在JDK 1.8以下的版本中无法运行）在版本号后面的，就是Class常量池入口了。

### Class常量池

Class常量池可以理解为是Class文件中的资源仓库。 Class文件中除了包含类的版本、字段、方法、接口等描述信息外，还有一项信息就是常量池(constant pool table)，用于存放编译器生成的各种字面量(Literal)和符号引用(Symbolic References)。

由于不同的Class文件中包含的常量的个数是不固定的，所以在Class文件的常量池入口处会设置两个字节的常量池容量计数器，记录了常量池中常量的个数。

![-w697][5]￼

当然，还有一种比较简单的查看Class文件中常量池的方法，那就是通过`javap`命令。对于以上的`HelloWorld.class`，可以通过

    javap -v  HelloWorld.class
    

查看常量池内容如下:

![][6]￼

> 从上图中可以看到，反编译后的class文件常量池中共有16个常量。而Class文件中常量计数器的数值是0011，将该16进制数字转换成10进制的结果是17。
> 
> 原因是与Java的语言习惯不同，常量池计数器是从0开始而不是从1开始的，常量池的个数是10进制的17，这就代表了其中有16个常量，索引值范围为1-16。

### 常量池中有什么

介绍完了什么是Class常量池以及如何查看常量池，那么接下来我们就要深入分析一下，Class常量池中都有哪些内容。

常量池中主要存放两大类常量：字面量（literal）和符号引用（symbolic references）。

### 字面量

前面说过，运行时常量池中主要保存的是字面量和符号引用，那么到底什么字面量？

> 在计算机科学中，字面量（literal）是用于表达源代码中一个固定值的表示法（notation）。几乎所有计算机编程语言都具有对基本值的字面量表示，诸如：整数、浮点数以及字符串；而有很多也对布尔类型和字符类型的值也支持字面量表示；还有一些甚至对枚举类型的元素以及像数组、记录和对象等复合类型的值也支持字面量表示法。

以上是关于计算机科学中关于字面量的解释，并不是很容易理解。说简单点，字面量就是指由字母、数字等构成的字符串或者数值。

字面量只可以右值出现，所谓右值是指等号右边的值，如：int a=123这里的a为左值，123为右值。在这个例子中123就是字面量。
```
    int a = 123;
    String s = "hollis";
```
上面的代码事例中，123和hollis都是字面量。

本文开头的HelloWorld代码中，Hollis就是一个字面量。

### 符号引用

常量池中，除了字面量以外，还有符号引用，那么到底什么是符号引用呢。

符号引用是编译原理中的概念，是相对于直接引用来说的。主要包括了以下三类常量： * 类和接口的全限定名 * 字段的名称和描述符 * 方法的名称和描述符

这也就可以印证前面的常量池中还包含一些`com/hollis/HelloWorld`、`main`、`([Ljava/lang/String;)V`等常量的原因了。

### Class常量池有什么用

前面介绍了这么多，关于Class常量池是什么，怎么查看Class常量池以及Class常量池中保存了哪些东西。有一个关键的问题没有讲，那就是Class常量池到底有什么用。

首先，可以明确的是，Class常量池是Class文件中的资源仓库，其中保存了各种常量。而这些常量都是开发者定义出来，需要在程序的运行期使用的。

在《深入理解Java虚拟》中有这样的表述：

Java代码在进行`Javac`编译的时候，并不像C和C++那样有“连接”这一步骤，而是在虚拟机加载Class文件的时候进行动态连接。也就是说，在Class文件中不会保存各个方法、字段的最终内存布局信息，因此这些字段、方法的符号引用不经过运行期转换的话无法得到真正的内存入口地址，也就无法直接被虚拟机使用。当虚拟机运行时，需要从常量池获得对应的符号引用，再在类创建时或运行时解析、翻译到具体的内存地址之中。关于类的创建和动态连接的内容，在虚拟机类加载过程时再进行详细讲解。

前面这段话，看起来很绕，不是很容易理解。其实他的意思就是： Class是用来保存常量的一个媒介场所，并且是一个中间场所。在JVM真的运行时，需要把常量池中的常量加载到内存中。

至于到底哪个阶段会做这件事情，以及Class常量池中的常量会以何种方式被加载到具体什么地方，会在本系列文章的后续内容中继续阐述。欢迎关注我的博客(http://www.hollischuang.com) 和公众号(Hollis)，即可第一时间获得最新内容。

另外，关于常量池中常量的存储形式，以及数据类型的表示方法本文中并未涉及，并不是说这部分知识点不重要，只是Class字节码的分析本就枯燥，作者不想在一篇文章中给读者灌输太多的理论上的内容。感兴趣的读者可以自行Google学习，如果真的有必要，我也可以单独写一篇文章再深入介绍。

### 参考资料

《深入理解java虚拟机》 [《Java虚拟机原理图解》 1.2.2、Class文件中的常量池详解（上）][7]

 [1]: http://www.hollischuang.com/archives/58
 [2]: http://www.hollischuang.com/archives/2322
 [3]: http://www.hollischuang.com/wp-content/uploads/2018/10/15401179593014.jpg
 [4]: http://www.hollischuang.com/archives/491
 [5]: http://www.hollischuang.com/wp-content/uploads/2018/10/15401192359009.jpg
 [6]: http://www.hollischuang.com/wp-content/uploads/2018/10/15401195127619.jpg
 [7]: https://blog.csdn.net/luanlouis/article/details/39960815

## 10. 运行时常量池

运行时常量池（ Runtime Constant Pool）是每一个类或接口的常量池（ Constant_Pool）的运行时表示形式。

它包括了若干种不同的常量：从编译期可知的数值字面量到必须运行期解析后才能获得的方法或字段引用。运行时常量池扮演了类似传统语言中符号表（ SymbolTable）的角色，不过它存储数据范围比通常意义上的符号表要更为广泛。

每一个运行时常量池都分配在 Java 虚拟机的方法区之中，在类和接口被加载到虚拟机后，对应的运行时常量池就被创建出来。

以上，是Java虚拟机规范中关于运行时常量池的定义。

### 运行时常量池在JDK各个版本中的实现

根据Java虚拟机规范约定：每一个运行时常量池都在Java虚拟机的方法区中分配，在加载类和接口到虚拟机后，就创建对应的运行时常量池。

在不同版本的JDK中，运行时常量池所处的位置也不一样。以HotSpot为例：

在JDK 1.7之前，方法区位于堆内存的永久代中，运行时常量池作为方法区的一部分，也处于永久代中。

因为使用永久代实现方法区可能导致内存泄露问题，所以，从JDK1.7开始，JVM尝试解决这一问题，在1.7中，将原本位于永久代中的运行时常量池移动到堆内存中。（永久代在JDK 1.7并没有完全移除，只是原来方法区中的运行时常量池、类的静态变量等移动到了堆内存中。）

在JDK 1.8中，彻底移除了永久代，方法区通过元空间的方式实现。随之，运行时常量池也在元空间中实现。

### 运行时常量池中常量的来源

运行时常量池中包含了若干种不同的常量：

编译期可知的字面量和符号引用（来自Class常量池）
运行期解析后可获得的常量（如String的intern方法）

所以，运行时常量池中的内容包含：Class常量池中的常量、字符串常量池中的内容

### 运行时常量池、Class常量池、字符串常量池的区别与联系


虚拟机启动过程中，会将各个Class文件中的常量池载入到运行时常量池中。

所以， Class常量池只是一个媒介场所。在JVM真的运行时，需要把常量池中的常量加载到内存中，进入到运行时常量池。

字符串常量池可以理解为运行时常量池分出来的部分。加载时，对于class的静态常量池，如果字符串会被装到字符串常量池中。


## 11. intern

 
在JVM中，为了减少相同的字符串的重复创建，为了达到节省内存的目的。会单独开辟一块内存，用于保存字符串常量，这个内存区域被叫做字符串常量池。
 

当代码中出现双引号形式（字面量）创建字符串对象时，JVM 会先对这个字符串进行检查，如果字符串常量池中存在相同内容的字符串对象的引用，则将这个引用返回；否则，创建新的字符串对象，然后将这个引用放入字符串常量池，并返回该引用。

除了以上方式之外，还有一种可以在运行期将字符串内容放置到字符串常量池的办法，那就是使用intern


intern的功能很简单：

在每次赋值的时候使用 `String` 的 `intern` 方法，如果常量池中有相同值，就会重复使用该对象，返回对象引用。

## 12. String有没有长度限制？

关于String有没有长度限制的问题，我之前单独写过一篇文章分析过，最近我又抽空回顾了一下这个问题，发现又有了一些新的认识。于是准备重新整理下这个内容。

这次在之前那篇文章的基础上除了增加了一些验证过程外，还有些错误内容的修正。我这次在分析过程中会尝试对Jdk的编译过程进行debug，并且会参考一些JVM规范等全方面的介绍下这个知识点。

因为这个问题涉及到Java的编译原理相关的知识，所以通过视频的方式讲解会更加容易理解一些，视频我上传到了B站：https://www.bilibili.com/video/BV1uK4y1t7H1/。

### String的长度限制

想要搞清楚这个问题，首先我们需要翻阅一下String的源码，看下其中是否有关于长度的限制或者定义。

String类中有很多重载的构造函数，其中有几个是支持用户传入length来执行长度的：

    public String(byte bytes[], int offset, int length) 
    

可以看到，这里面的参数length是使用int类型定义的，那么也就是说，String定义的时候，最大支持的长度就是int的最大范围值。

根据Integer类的定义，`java.lang.Integer#MAX_VALUE`的最大值是2^31 - 1;

那么，我们是不是就可以认为String能支持的最大长度就是这个值了呢？

其实并不是，这个值只是在运行期，我们构造String的时候可以支持的一个最大长度，而实际上，在编译期，定义字符串的时候也是有长度限制的。

如以下代码：

    String s = "11111...1111";//其中有10万个字符"1"
    

当我们使用如上形式定义一个字符串的时候，当我们执行javac编译时，是会抛出异常的，提示如下：

    错误: 常量字符串过长
    

那么，明明String的构造函数指定的长度是可以支持2147483647(2^31 - 1)的，为什么像以上形式定义的时候无法编译呢？

其实，形如`String s = "xxx";`定义String的时候，xxx被我们称之为字面量，这种字面量在编译之后会以常量的形式进入到Class常量池。

那么问题就来了，因为要进入常量池，就要遵守常量池的有关规定。

### 常量池限制

我们知道，javac是将Java文件编译成class文件的一个命令，那么在Class文件生成过程中，就需要遵守一定的格式。

根据《Java虚拟机规范》中第4.4章节常量池的定义，CONSTANT_String_info 用于表示 java.lang.String 类型的常量对象，格式如下：

    CONSTANT_String_info {
        u1 tag;
        u2 string_index;
    }
    

其中，string_index 项的值必须是对常量池的有效索引， 常量池在该索引处的项必须是 CONSTANT_Utf8_info 结构，表示一组 Unicode 码点序列，这组 Unicode 码点序列最终会被初始化为一个 String 对象。

CONSTANT_Utf8_info 结构用于表示字符串常量的值：

    CONSTANT_Utf8_info {
        u1 tag;
        u2 length;
        u1 bytes[length];
    }
    

其中，length则指明了 bytes[]数组的长度，其类型为u2，

通过翻阅《规范》，我们可以获悉。u2表示两个字节的无符号数，那么1个字节有8位，2个字节就有16位。

16位无符号数可表示的最大值位2^16 - 1 = 65535。

也就是说，Class文件中常量池的格式规定了，其字符串常量的长度不能超过65535。

那么，我们尝试使用以下方式定义字符串：

     String s = "11111...1111";//其中有65535个字符"1"
    

尝试使用javac编译，同样会得到"错误: 常量字符串过长"，那么原因是什么呢？

其实，这个原因在javac的代码中是可以找到的，在Gen类中有如下代码：

    private void checkStringConstant(DiagnosticPosition var1, Object var2) {
        if (this.nerrs == 0 && var2 != null && var2 instanceof String && ((String)var2).length() >= 65535) {
            this.log.error(var1, "limit.string", new Object[0]);
            ++this.nerrs;
        }
    }
    

代码中可以看出，当参数类型为String，并且长度大于等于65535的时候，就会导致编译失败。

这个地方大家可以尝试着debug一下javac的编译过程（视频中有对java的编译过程进行debug的方法），也可以发现这个地方会报错。

如果我们尝试以65534个字符定义字符串，则会发现可以正常编译。

其实，关于这个值，在《Java虚拟机规范》也有过说明：

> if the Java Virtual Machine code for a method is exactly 65535 bytes long and ends with an instruction that is 1 byte long, then that instruction cannot be protected by an exception handler. A compiler writer can work around this bug by limiting the maximum size of the generated Java Virtual Machine code for any method, instance initialization method, or static initializer (the size of any code array) to 65534 bytes

### 运行期限制

上面提到的这种String长度的限制是编译期的限制，也就是使用String s= “”;这种字面值方式定义的时候才会有的限制。

那么。String在运行期有没有限制呢，答案是有的，就是我们前文提到的那个Integer.MAX_VALUE ，这个值约等于4G，在运行期，如果String的长度超过这个范围，就可能会抛出异常。(在jdk 1.9之前）

int 是一个 32 位变量类型，取正数部分来算的话，他们最长可以有

    2^31-1 =2147483647 个 16-bit Unicodecharacter
    
    2147483647 * 16 = 34359738352 位
    34359738352 / 8 = 4294967294 (Byte)
    4294967294 / 1024 = 4194303.998046875 (KB)
    4194303.998046875 / 1024 = 4095.9999980926513671875 (MB)
    4095.9999980926513671875 / 1024 = 3.99999999813735485076904296875 (GB)
    

有近 4G 的容量。

很多人会有疑惑，编译的时候最大长度都要求小于65535了，运行期怎么会出现大于65535的情况呢。这其实很常见，如以下代码：

    String s = "";
    for (int i = 0; i <100000 ; i++) {
        s+="i";
    }
    

得到的字符串长度就有10万，另外我之前在实际应用中遇到过这个问题。

之前一次系统对接，需要传输高清图片，约定的传输方式是对方将图片转成BASE6编码，我们接收到之后再转成图片。

在将BASE64编码后的内容赋值给字符串的时候就抛了异常。

### 总结

字符串有长度限制，在编译期，要求字符串常量池中的常量不能超过65535，并且在javac执行过程中控制了最大值为65534。

在运行期，长度不能超过Int的范围，否则会抛异常。

最后，这个知识点 ，我录制了视频(https://www.bilibili.com/video/BV1uK4y1t7H1/)，其中有关于如何进行实验测试、如何查阅Java规范以及如何对javac进行deubg的技巧。欢迎进一步学习。

