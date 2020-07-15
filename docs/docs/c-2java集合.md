![Java面试集锦：集合思维导图与30道集合面试题.png](https://upload-images.jianshu.io/upload_images/7326374-d0d95aacb79fe020.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![Collection集合.png](https://upload-images.jianshu.io/upload_images/7326374-713401355dd1edd1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![List集合.png](https://upload-images.jianshu.io/upload_images/7326374-827d52005b88f6c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![ArrayListandVectorandLinkedList.png](https://upload-images.jianshu.io/upload_images/7326374-1fac794b96f88f61.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![hashmapandtreemapandhashtable.png](https://upload-images.jianshu.io/upload_images/7326374-887b6324f047ec1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![map.png](https://upload-images.jianshu.io/upload_images/7326374-638e2081063a08b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![set集合.png](https://upload-images.jianshu.io/upload_images/7326374-80d9060200b9f713.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![其他.png](https://upload-images.jianshu.io/upload_images/7326374-265e4e16038d08e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Java集合框架为Java编程语言的基础，也是Java面试中很重要的一个知识点。这里，我列出了一些关于Java集合的重要问题和答案。

### 1.Java集合框架是什么？说出一些集合框架的优点？

每种编程语言中都有集合，最初的Java版本包含几种集合类：`Vector`、`Stack`、`HashTable`和`Array`。

随着集合的广泛使用，`Java1.2`提出了囊括所有集合接口、实现和算法的集合框架。在保证线程安全的情况下使用泛型和并发集合类，Java已经经历了很久。它还包括在Java并发包中，阻塞接口以及它们的实现。

集合框架的部分优点如下：

（1）使用核心集合类降低开发成本，而非实现我们自己的集合类。

（2）随着使用经过严格测试的集合框架类，代码质量会得到提高。

（3）通过使用JDK附带的集合类，可以降低代码维护成本。

（4）复用性和可操作性。

### 2.集合框架中的泛型有什么优点？

1.Java1.5引入了泛型，所有的集合接口和实现都大量地使用它。

2.泛型允许我们为集合提供一个可以容纳的对象类型，因此，如果你添加其它类型的任何元素，它会在编译时报错。

3.这避免了在运行时出现ClassCastException，因为你将会在编译时得到报错信息。

4.泛型也使得代码整洁，我们不需要使用显式转换和instanceOf操作符。

5.它也给运行时带来好处，因为不会产生类型检查的字节码指令。

### 3.Java集合框架的基础接口有哪些？

`Collection`为集合层级的根接口。一个集合代表一组对象，这些对象即为它的元素。Java平台不提供这个接口任何直接的实现。

`Set`是一个不能包含重复元素的集合。这个接口对数学集合抽象进行建模，被用来代表集合，就如一副牌。

`List`是一个有序集合，可以包含重复元素。你可以通过它的索引来访问任何元素。List更像长度动态变换的数组。

`Map`是一个将key映射到value的对象.一个Map不能包含重复的key：每个key最多只能映射一个value。

一些其它的接口有Queue、Dequeue、SortedSet、SortedMap和ListIterator。



### 4.为何Map接口不继承Collection接口？

尽管`Map`接口和它的实现也是集合框架的一部分，但`Map`不是集合，集合也不是`Map`。因此，`Map`继承`Collection`毫无意义，反之亦然。

如果`Map`继承`Collection`接口，那么元素去哪儿？Map包含key-value对，它提供抽取key或value列表集合的方法，但是它不适合“一组对象”规范。



### 5.ArrayList和Vector有何异同点？

ArrayList和Vector在很多时候都很类似。

（1）两者都是基于索引的，内部由一个数组支持。

（2）两者维护插入的顺序，我们可以根据插入顺序来获取元素。

（3）ArrayList和Vector的迭代器实现都是fail-fast的。

（4）ArrayList和Vector两者允许null值，也可以使用索引值对元素进行随机访问。

以下是ArrayList和Vector的不同点。

（1）Vector是同步的，而ArrayList不是。然而，如果你寻求在迭代的时候对列表进行改变，你应该使用CopyOnWriteArrayList。

（2）ArrayList比Vector快，它因为有同步，不会过载。

（3）ArrayList更加通用，因为我们可以使用Collections工具类轻易地获取同步列表和只读列表。

### 6.Array和ArrayList有何区别？什么时候更适合用Array？

Array可以容纳基本类型和对象，而ArrayList只能容纳对象。

Array是指定大小的，而ArrayList大小是固定的。

Array没有提供ArrayList那么多功能，比如addAll、removeAll和iterator等。尽管ArrayList明显是更好的选择，但也有些时候Array比较好用。

（1）如果列表的大小已经指定，大部分情况下是存储和遍历它们。

（2）对于遍历基本数据类型，尽管Collections使用自动装箱来减轻编码任务，在指定大小的基本类型的列表上工作也会变得很慢。

（3）如果你要使用多维数组，使用[][]比List>更容易。


### 7.ArrayList和LinkedList有何区别？

ArrayList和LinkedList两者都实现了List接口，但是它们之间有些不同。

1）ArrayList是由Array所支持的基于一个索引的数据结构，所以它提供对元素的随机访问，复杂度为O(1)，但LinkedList存储一系列的节点数据，每个节点都与前一个和下一个节点相连接。所以，尽管有使用索引获取元素的方法，内部实现是从起始点开始遍历，遍历到索引的节点然后返回元素，时间复杂度为O(n)，比ArrayList要慢。
2）与ArrayList相比，在LinkedList中插入、添加和删除一个元素会更快，因为在一个元素被插入到中间的时候，不会涉及改变数组的大小，或更新索引。

3）LinkedList比ArrayList消耗更多的内存，因为LinkedList中的每个节点存储了前后节点的引用。

### 8.哪些集合类提供对元素的随机访问？

ArrayList、HashMap、TreeMap和HashTable类提供对元素的随机访问。

### 9.哪些集合类是线程安全的？

Vector、HashTable、Properties和Stack是同步类，所以它们是线程安全的，可以在多线程环境下使用。Java1.5并发API包括一些集合类，允许迭代时修改，因为它们都工作在集合的克隆上，所以它们在多线程环境中是安全的。点击这里一文搞懂问什么线程不安全。

### 10.并发集合类是什么？

Java1.5并发包（java.util.concurrent）包含线程安全集合类，允许在迭代时修改集合。迭代器被设计为fail-fast的，会抛出ConcurrentModificationException。一部分类为：CopyOnWriteArrayList、 ConcurrentHashMap、CopyOnWriteArraySet。

### 11.队列和栈是什么，列出它们的区别？

栈和队列两者都被用来预存储数据。java.util.Queue是一个接口，它的实现类在Java并发包中。队列允许先进先出（FIFO）检索元素，但并非总是这样。Deque接口允许从两端检索元素。栈与队列很相似，但它允许对元素进行后进先出（LIFO）进行检索。Stack是一个扩展自Vector的类，而Queue是一个接口。

### 12.Collections类是什么？

Java.util.Collections是一个工具类仅包含静态方法，它们操作或返回集合。它包含操作集合的多态算法，返回一个由指定集合支持的新集合和其它一些内容。这个类包含集合框架算法的方法，比如折半搜索、排序、混编和逆序等。

### 13.Comparable和Comparator接口有何区别？

Comparable和Comparator接口被用来对对象集合或者数组进行排序。Comparable接口被用来提供对象的自然排序，我们可以使用它来提供基于单个逻辑的排序。Comparator接口被用来提供不同的排序算法，我们可以选择需要使用的Comparator来对给定的对象集合进行排序。

### 14.Iterator是什么？

Iterator接口提供遍历任何Collection的接口。我们可以从一个Collection中使用迭代器方法来获取迭代器实例。迭代器取代了Java集合框架中的Enumeration。迭代器允许调用者在迭代过程中移除元素。

### 15.Enumeration和Iterator接口的区别？

Enumeration的速度是Iterator的两倍，也使用更少的内存。Enumeration是非常基础的，也满足了基础的需要。但是，与Enumeration相比，Iterator更加安全，因为当一个集合正在被遍历的时候，它会阻止其它线程去修改集合。迭代器取代了Java集合框架中的Enumeration。迭代器允许调用者从集合中移除元素，而Enumeration不能做到。为了使它的功能更加清晰，迭代器方法名已经经过改善。


### 16.Iterater和ListIterator之间有什么区别？

（1）我们可以使用Iterator来遍历Set和List集合，而ListIterator只能遍历List。
（2）Iterator只可以向前遍历，而LIstIterator可以双向遍历。
（3）ListIterator从Iterator接口继承，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置。

### 17.通过迭代器fail-fast属性，你明白了什么？

每次我们尝试获取下一个元素的时候，Iterator fail-fast属性检查当前集合结构里的任何改动。如果发现任何改动，它抛出ConcurrentModificationException。Collection中所有Iterator的实现都是按fail-fast来设计的（ConcurrentHashMap和CopyOnWriteArrayList这类并发集合类除外）。

### 18.fail-fast与fail-safe有什么区别？

Iterator的fail-fast属性与当前的集合共同起作用，因此它不会受到集合中任何改动的影响。Java.util包中的所有集合类都被设计为fail-fast的，而java.util.concurrent中的集合类都为fail-safe的。Fall—fast迭代器抛出ConcurrentModificationException，fall—safe迭代器从不抛出ConcurrentModificationException。

### 19.在迭代一个集合的时候，如何避免？

ConcurrentModificationException？在遍历一个集合的时候我们可以使用并发集合类来避免ConcurrentModificationException，比如使用CopyOnWriteArrayList，而不是ArrayList。


### 20.HashMap和HashTable有何不同？

（1）HashMap允许key和value为null，而HashTable不允许。
（2）HashTable是同步的，而HashMap不是。所以HashMap适合单线程环境，HashTable适合多线程环境。
（3）在Java1.4中引入了LinkedHashMap，HashMap的一个子类，假如你想要遍历顺序，你很容易从HashMap转向LinkedHashMap，但是HashTable不是这样的，它的顺序是不可预知的。
（4）HashMap提供对key的Set进行遍历，因此它是fail-fast的，但HashTable提供对key的Enumeration进行遍历，它不支持fail-fast。
（5）HashTable被认为是个遗留的类，如果你寻求在迭代的时候修改Map，你应该使用CocurrentHashMap。

### 21.如何决定选用HashMap还是TreeMap？

对于在Map中插入、删除和定位元素这类操作，HashMap是最好的选择。然而，假如你需要对一个有序的key集合进行遍历，TreeMap是更好的选择。基于你的collection的大小，也许向HashMap中添加元素会更快，将map换为TreeMap进行有序key的遍历。


### 22.我们如何对一组对象进行排序？

如果我们需要对一个对象数组进行排序，我们可以使用Arrays.sort()方法。如果我们需要排序一个对象列表，我们可以使用Collection.sort()方法。两个类都有用于自然排序（使用Comparable）或基于标准的排序（使用Comparator）的重载方法sort()。Collections内部使用数组排序方法，所有它们两者都有相同的性能，只是Collections需要花时间将列表转换为数组。


### 23.有哪些关于 Java 集合框架的最佳实践？

- 基于应用的需求来选择使用正确类型的集合，这对性能来说是非常重要的。例如，如果元素的大小是固定的，并且知道优先级，我们将会使用一个 `Array` ，而不是 `ArrayList` 。
- 一些集合类允许我们指定他们的初始容量。因此，如果我们知道存储数据的大概数值，就可以避免重散列或者大小的调整。
- 总是使用泛型来保证类型安全，可靠性和健壮性。同时，使用泛型还可以避免运行时的 `ClassCastException` 异常。
- 在 `Map` 中使用 `JDK` 提供的不可变类作为一个 `key`，这样可以避免 `hashcode` 的实现和我们自定义类的 `equals` 方法。
- 应该依照接口而不是实现来编程。
- 返回零长度的集合或者数组，而不是返回一个 `null` ，这样可以防止底层集合是空的。


### 24. List 和 Set 区别？

`List`，`Set` 都是继承自 `Collection` 接口。

- List 特点：元素有放入顺序，元素可重复。
- Set 特点：元素无放入顺序，元素不可重复，重复元素会覆盖掉。
> 注意：元素虽然无放入顺序，但是元素在 Set 中的位置是有该元素的 hashcode 决定的，其位置其实是固定的。

另外 `List` 支持 `for` 循环，也就是通过下标来遍历，也可以用迭代器，但是 `Set `只能用迭代，因为他无序，无法用下标来取得想要的值。

`Set` 和 `List` 对比：

- Set：检索元素效率高，删除和插入效率低，插入和删除不会引起元素位置改变。
- List：和数组类似，List 可以动态增长，查找元素效率低，插入删除元素效率，因为可能会引起其他元素位置改变。



### 25. HashSet 和 TreeSet 的区别？

`HashSet` 是用一个` hash` 表来实现的，因此，它的元素是无序的。添加，删除和 `HashSet` 包括的方法的持续时间复杂度是`O(1)` 。
`TreeSet` 是用一个树形结构实现的，因此，它是有序的。添加，删除和 `TreeSet` 包含的方法的持续时间复杂度是 `O(logn)` 。

如何决定选用 `HashMap` 还是 `TreeMap`？

对于在` Map `中插入、删除和定位元素这类操作，`HashMap` 是最好的选择。
然而，假如你需要对一个有序的 key 集合进行遍历， TreeMap 是更好的选择。
基于你的 `collection` 的大小，也许向 `HashMap` 中添加元素会更快，再将 `HashMap` 换为 `TreeMap` 进行有序 `key` 的遍历。


### 26. HashMap 和 ConcurrentHashMap 的区别？

`ConcurrentHashMap` 是线程安全的 `HashMap` 的实现。主要区别如下：

1、ConcurrentHashMap 对整个桶数组进行了分割分段(Segment)，然后在每一个分段上都用 lock 锁进行保护，相对 于Hashtable 的 syn 关键字锁的粒度更精细了一些，并发性能更好。而 HashMap 没有锁机制，不是线程安全的。JDK8 之后，ConcurrentHashMap 启用了一种全新的方式实现,利用 CAS 算法。
2、HashMap 的键值对允许有 null ，但是 ConCurrentHashMap 都不允许。


### 27. 什么是迭代器(Iterator)？

迭代器是一种设计模式，它是一个对象，它可以遍历并选择序列中的对象，而开发人员不需要了解该序列的底层结构。迭代器通常被称为“轻量级”对象，因为创建它的代价小。Java中的Iterator功能比较简单，并且只能单向移动：对已集合类中的任何一个实现类，都可以返回这样一个Iterator对象。跟循环差不多。

好处是可以适合用于任何一个类，而且java也对它进行了优化，比直接用index访问快一点。迭代器是一种模式，它可以使得对于序列类型的数据结构的遍历行为与被遍历的对象分离，迭代器相当于有个指针,每次调用hasNext()方法如果返回值是true,说明有下一个元素,再执行next()方法获得该元素的值.

在迭代器Iteartor接口中，有以下3个方法：
1.hasNext() 该方法英语判断集合对象是否还有下一个元素，如果已经是最后一个元素则返回false
2.next() 把迭代器的指向移到下一个位置，同时，该方法返回下一个元素的引用
3.remove()  从迭代器指向的Collection中移除迭代器返回的最后一个元素，该操作使用的比较少。

### 28. 什么是Java优先级队列(Priority Queue)？

PriorityQueue是一个基于优先级堆的无界有序队列，它的元素是按照自然顺序(natural order)排序的。在创建的时候，我们可以给它提供一个负责给元素排序的比较器。PriorityQueue不允许null值，因为他们没有自然顺序，或者说他们没有任何的相关联的比较器。最后，PriorityQueue不是线程安全的，入队和出队的时间复杂度是O(log(n))。

### 29.你了解大O符号(big-O notation)么？你能给出不同数据结构的例子么？

大O：描述了当数据结构里面的元素增加的时候，算法的规模或者是性能在最坏的场景下有多么好。
大O符号也可用来描述其他的行为，比如：内存消耗。因为集合类实际上是数据结构，我们一般使用大O符号基于时间，内存和性能来选择最好的实现。大O符号可以对大量数据的性能给出一个很好的说明。

### 30.如何权衡是使用无序的数组还是有序的数组？

有序数组最大的好处在于查找的时间复杂度是O(log n)，而无序数组是O(n)。有序数组的缺点是插入操作的时间复杂度是O(n)，因为值大的元素需要往后移动来给新元素腾位置。相反，无序数组的插入时间复杂度是常量O(1)。
所以，查找操作多的时候，使用有序；增删操作多的使用无序的即可。


## 推荐
[大厂笔试内容集合（内有详细解析）](https://mp.weixin.qq.com/mp/homepage?__biz=MzIwMTg3NzYyOA==&hid=10&sn=34bfdf3ba4ff1ef1852e392195616f4e) 持续更新中....

[ProcessOn是一个在线作图工具的聚合平台~](https://www.processon.com/i/5cd53c2fe4b01941c8cf1c21)

## 文末

>欢迎关注个人微信公众号：**Coder编程**
欢迎关注**Coder编程**公众号，主要分享数据结构与算法、Java相关知识体系、框架知识及原理、Spring全家桶、微服务项目实战、DevOps实践之路、每日一篇互联网大厂面试或笔试题以及PMP项目管理知识等。更多精彩内容正在路上~
新建了一个qq群：315211365，欢迎大家进群交流一起学习。谢谢了！也可以介绍给身边有需要的朋友。

>文章收录至
Github: https://github.com/CoderMerlin/coder-programming
Gitee: https://gitee.com/573059382/coder-programming
欢迎**关注**并star~
![微信公众号](https://upload-images.jianshu.io/upload_images/7326374-4a93eb93c3882ff2?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




















