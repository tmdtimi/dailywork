## HashMap

数组+链表/红黑树

map.put("A","B");

首先计算 A 的hashcode()

public native int hashCode()

不同对象hashCode()可能相等

两个对象equals返回true,hashCode肯定相等

两个对象equals返回false,hashcode也可能相等




数组+链表/红黑树


HashMap 静态内部类 Node<k,v>

final int hash

final k key

V value

Node<k,v> next


Node数组结构 初始长度16

发生冲突形成链表

链表长度超过8之时 链表升级为红黑树


**map.put("apple"，0)**

index=Hash("apple")=2

![Alt pic](../pics/map1.png)

如果index发生冲突 ----头插法

![Alt pic](../pics/map2.png)



**map.get("apple")**

index=Hash("apple")

有hash冲突时，一个一个向下找
![Alt pic](../pics/map3.png)

头插法---后插入的元素被查找的可能性更大



**HashMap初始容量16**

每次自动扩展或者手动初始化时，长度必须是2的幂

之所以选择16，是为了满足从key映射到index的算法

index = Hash（“apple”）

index = HashCode（Key） & （Length - 1）

length是2的幂，计算只用关注HashCode(key)的后4位 ， 提升了效率 并且可以均匀分布


**Resize**

影响Resize的两个因素

1:Capacity : HashMap的当前容量

2:LoadFactor : 负载因子 ， 默认值为0.75f


当 hashMap.size() > Capacity*LoadFactor时

*一： 扩容： 创建一个新的Entry空数组，长度是原数组的两倍*

*二：ReHash：*
	
遍历原Entry数组，把所有的Entry重新Hash到新数组 ----- 因为长度改变了  元素的Hash值也会改变


index = HashCode（Key） & （Length - 1）


**Rehash可能带来的线程不安全问题**

![Alt pic](../pics/map4.png)

![Alt pic](../pics/map5.png)



假设一个HashMap已到达了Resize的临界点  此时有线程A和B,在同一时刻对HashMap进行put操作 

此时两个线程各自进行Resize的第一步  扩容

Resize可能出现环形链表  进入死循环


![Alt pic](../pics/map6.png)




二叉查找树：

1：左/右子树所有节点的值小于或等于根节点值

2：左/右子树皆为二叉查找树

查找最大次数为树的高度

但是 1 2 3 4 5 6 这种将退化成链表


**红黑树**

自平衡的二叉查找树

1：节点是红色或者黑色

2：根节点是黑色

3：每个叶子节点都是黑色的空节点

4：每个红色节点的两个子节点都是黑色

5：从任一节点到每个叶子节点的所有路径都有相同的黑色节点数	

![Alt pic](../pics/Tree1.png)


当红黑树插入节点，破坏相应规则时，可以采取两种措施维持树的平衡

若父节点时黑色节点，就直接加入即可

若父节点时红色节点，则需要考虑两种措施


1：变色
  
2：左/右旋转    

### CuncurrentHashMap

想避免 多个线程操作HashMap出现的Resize环形链表--死循环问题

可以使用 HashTable  Collections.sychronizedMap()

但是性能----无论是读还是写 ， 都会给集合加上锁，导致同一时间其他操作阻塞


![Alt pic](../pics/cm1.png)


CucurrentHashMap提出segment概念

segment相当于一个 HashMap对象

segment也是一个 HashEntry数组， 每个HashEntry即是键值对，也是链表头节点

![Alt pic](../pics/segment.png)

CuncurrentHashMap在链表中有2的N次方个segment


![Alt pic](../pics/CuncurrentHashMap.png)


优势在于，采用了锁分段技术，每一格segment之间互补影响


**1:不同Segment中可以并发写入**

![Alt pic](../pics/w1.png)

**2:同一个Segment中可以并发读写**
![Alt pic](../pics/w2.png)


**3:同一个Segment中并发写入**

会被阻塞

![Alt pic](../pics/w3.png)


对同一个segment中的并发写入是需要加锁的


**GET**

1.为输入的Key做Hash运算，得到hash值。


2.通过hash值，定位到对应的Segment对象


3.再次通过hash值，定位到Segment当中数组的具体位置。


**Put**

1.为输入的Key做Hash运算，得到hash值。


2.通过hash值，定位到对应的Segment对象


3.获取**可重入锁**(可以并发读、读写、不能并发写)


4.再次通过hash值，定位到Segment当中数组的具体位置。


5.插入或覆盖HashEntry对象。


6.释放锁。


**Size**

ConcurrentHashMap的Size方法是一个嵌套循环，大体逻辑如下：



1.遍历所有的Segment。


2.把Segment的元素数量累加起来。


3.把Segment的修改次数累加起来。


4.判断所有Segment的总修改次数是否大于上一次的总修改次数。

如果大于，说明统计过程中有修改，重新统计，尝试次数+1；如果不是。说明没有修改，统计结束。


5.如果尝试次数超过阈值，则对每一个Segment加锁，再重新统计。


6.再次判断所有Segment的总修改次数是否大于上一次的总修改次数。由于已经加锁，次数一定和上次相等。


7.释放锁，统计结束。


先乐观锁 后 悲观锁

为了尽量不锁住所有Segment，首先乐观地假设Size过程中不会有修改。当尝试一定次数，才无奈转为悲观锁，锁住所有Segment保证强一致性

