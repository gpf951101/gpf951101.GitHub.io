---
layout: article
title: Java集合之HashMap
key: May20200804_1
date: 2020-8-4
tags: [Java, 面试]
sharing: true
show_author_profile: true
lang: zh
comment: true
pageview: true
---

HashMap的简单理解与分析。

<!--more-->

# 1. HashMap的创建

当创建HashMap集合对象的时候，在jdk8之前，构造方法中创建一个长度是**16**的`Entry[] table`用来存储键值对数据。在jdk8与以后，不是在HashMap的构造方法底层创建数组，是在第一次调用put方法的时候创建`Node[] table`用来存储键值对数据的。



# 2. HashMap的存储

1. 假设想哈希表中存放“数据结构-21”的时候，根据“数据结构”调用String类中重写的hashCode()方法计算出hash值，然后结构数组长度采用某种算法计算出向Node数组中存储数据的索引值。如果计算出的索引值所在空间没有数据，则直接将“数据结构-21”存储到数组中，如计算出索引值为3.

   1.1 哈希表底层采用何种算法计算hash值？还有哪些算法可以计算出hash值？

   - 底层采用key的hashCode()方法的值结合数组长度进行**无符号右移(>>>)**、**按位异或(^)**、**按位与(&)**计算出索引值。
   - 还可采用：平方取中法、取余数、伪随机数。
   - 其他计算方法与位运算相比，相率较低。

2. 向hash表中插入“算法-43”的时候，假设“算法“计算出的hashCode()方法结合数组长度计算出的索引值与”数据结构“计算出的索引值相同，也是3，此时数组空间不为null。

   - 底层比较"数据结构"与”算法“的hash值是否一致，如果不一致，则在此空间上画出一个节点来存储键值对数据”算法-43“，这种方法称为**拉链法**。

3. 向hash表中插入“数据结构-23”时，首先调用“数据结构”的hashCode()方法结合数组长度计算出索引，肯定为3，然后和步骤2一样，比较hashCode是否相等，此时HashCode值相等。

   - hashCode值相等，发生哈希碰撞。
   - 底层调用“数据结构”所属类String的equals方法比较内容是否相等。
     - 如果内容相等：则将后添加的数据的value值覆盖之前的value值。
     - 如果内容不相等：则继续向下和其他数据的key进行比较，如果都不相等，则划出一个节点存储数据。

**如果节点长度即链表长度大于阈值8且数组长度大于64则将，链表转化为红黑树。**

![image-20200804110332457](/images/image-20200804110332457.png)

```java
Map<String, Integer> map = new HashMap<>();
map.put("数据结构", 21);
map.put("算法", 43);
map.put("数据结构", 23);
```

# 3. HashMap的继承关系

![image-20200804112033724](/images/image-20200804112033724.png)

```java
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable
```

继承了抽象类`AbstractMap`，又实现了`Map`接口，`AbstractMap`也实现了`Map`接口。

# 4. HashMap的集合成员

## 4.1 成员变量

![image-20200804112427738](\images\image-20200804112427738.png)

```java
// 序列化版本号
private static final long serialVersionUID = 362498820763181265L;
// 必须是2的n次幂 默认容量是14 1<<4 相当于1*2^4
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
// 集合最大容量上限
static final int MAXIMUM_CAPACITY = 1 << 30;
// 默认负载因子(加载因子) 默认是0.75
static final float DEFAULT_LOAD_FACTOR = 0.75f;
// 普通链表转红黑树阈值
static final int TREEIFY_THRESHOLD = 8;
// 红黑树转普通链表阈值
static final int UNTREEIFY_THRESHOLD = 6;
// 当节点链表超过8且数组长度大于此值，转化为红黑树，如果小于此值，进行扩容
static final int MIN_TREEIFY_CAPACITY = 64;
// 存储元素的数组 JDK8之前叫做Entry，仅仅改名
transient Node<K,V>[] table;
// 存放具体元素的集合   
transient Set<Map.Entry<K,V>> entrySet;
// 存放元素的个数，不等于数组长度。默认为0
transient int size;
// 用来记录HashMap修改次数(扩容和更改结构)
transient int modCount;
// 用来调整大小的阈值 等于容量*负载因子 jdk8为容量，在put中再与负载因子相乘
int threshold;
// 加载因子
final float loadFactor;
```

**问题1**：`DEFAULT_INITIAL_CAPACITY`为什么必须是2的n次幂？

**答：**当向HashMap中添加元素的时候，需要根据key的hash值去确定在数组中的文职。为了减少碰撞，需要将数据尽量均匀分配，每个链表长度大致相同，这个是在就在把数据存到那个链表中的算法。这个算法实际就是取模，但是效率低于位运算，所以源码中使用hash&(length-1)，而实际上hash%length等于hash&(length-1)的前提是length是2的n次幂。

![image-20200804223526507](\images\image-20200804223526507.png)

如果长度为5,此时不等于取模运算，容易发生hash碰撞，如2、3、4、5等结果都是0.

![image-20200804223941113](\images\image-20200804223941113.png)

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        ...
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
    	...
}
```

总结：

- 根据key的hash确定其在数组中的位置时，如果n为2的幂，可保证数据的均匀插入，如果不是，可能会加大hash冲突。
- 如果采用取余运算，效率要低于位运算。
- HashMap的大小为2次幂可以使得数据均匀分布，减少hash冲突。如果hash冲突越大，代表数组中一个链的长度越大，使得性能降低。

**问题2：**如果初始化大小输入不是2的幂结果会怎样？

**答：**如果初始化长度不是2的幂，则通过位移运算和或运算得到的肯定是2的幂次数，且是最接近初始化且大于初始化值的数。

```java
static final int MAXIMUM_CAPACITY = 1 << 30
public HashMap(int initialCapacity, float loadFactor) {
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " +
                                           initialCapacity);
    // int最大为2^31-1，所以如果初始化的值大于1<<30，则赋值为1<<30. 后结果最大为1<<30.
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
    if (loadFactor <= 0 || Float.isNaN(loadFactor))
        throw new IllegalArgumentException("Illegal load factor: " +
                                           loadFactor);
    this.loadFactor = loadFactor;
    this.threshold = tableSizeFor(initialCapacity);
}
static final int tableSizeFor(int cap) {
    // 为了防止结果已经是2的幂，如果没有减一操作，经过后续命令，则结果为n的2倍。
    int n = cap - 1;
    // 如果此时n为0，则结果为n+1=0.
    n |= n >>> 1;
    n |= n >>> 2;
    n |= n >>> 4;
    n |= n >>> 8;
    n |= n >>> 16;
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```

当cap-1不为0，分析无符号右移操作。

如cap=10.

```tex
int n = 10 - 1 = 9; // 00000000 00000000 00000000 00001001
n |= n >>> 1;
// n>>>1 -------> 00000000 00000000 00000000 00000100
// n -----------> 00000000 00000000 00000000 00001001
// n | n>>>1----> 00000000 00000000 00000000 00001101
// 规律：最左边1的右边一位变为1.
n |= n>>>2;
// n>>>2 -------> 00000000 00000000 00000000 00000011
// n -----------> 00000000 00000000 00000000 00001101
// n | n>>>2 ---> 00000000 00000000 00000000 00001111
// 后续操作不改变n的值 最终返回n+1=16.
```

在执行tableSizeFor操作的时候，已经确保cap不大于2^30, 最多为30个1，30个1再加1则为2^30.(2^0=1为第一位)。

考虑最大值，即30个1， 00111111 11111111 11111111 11111111. 无符号右移16位为：00000000 00000000 00111111 11111111。做与运算后，结果为n，最终返回为n+1，即cap。

**问题3**：为什么Map桶中节点个数超过8才转为红黑树？

答：因为树节点的大小大约是普通节点的两倍，所以我们只在桶包含足够的节点时才使用树节点。当他们变得太小(6)时，就会转换为普通的桶，由参数`UNTREEIFY_THRESHOLD`决定。在使用分布良好的用户hashCode时，很少使用红黑树，亮相情况下，在随机哈希码下，桶中节点的频率服从泊松分布，8时值特别小。

TreeNodes占用空间时普通Nodes的两倍，当只有桶中包含足够多的节点才会转为TreeNodes。是否足够多是由参数`TREEIFY_THRESHOLD`决定的。TreeNodes和Nodes的转化其实是为了时间和空间的权衡。

红黑树平均查找长度log(n)，链表为n/2，当n为8，前者为3，后者为4，前者效率更好。

**注：当链表中节点超过8，且数组长度大于等于64，才会转化为红黑树。如果仅仅节点超过8，则进行扩容。**

由参数`MIN_TREEIFY_CAPACITY`决定，这个值不能小于4*`TREEIFY_THRESHOLD`.

**问题4：**为什么加载因子默认设置为0.75？

答：加载因子是用来衡量HashMap满的程度，表示HashMap的疏密程度，影响hash操作到同一个数组位置的概率。

架子啊因子太大，导致查找元素效率低，太小导致数组利用率低。当size/capacity大于0.75，则需要进行扩容。扩容涉及到rehash、复制数据等操作，非常消耗性能，所以开发过程中要尽量减少扩容的次数，如确定大小，尽量初始化指定大小。

## 4.2 构造方法

1. 无参。默认容量为16，负载因子为0.75.

   ```java
   // jdk8之后，初始容量不在此处，在第一次put的时候
   public HashMap() {
       this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
   }
   ```

2. 指定初始容量，默认负载因子0.75。

   ```java
   // 调用了第3种方法
   public HashMap(int initialCapacity) {
       this(initialCapacity, DEFAULT_LOAD_FACTOR);
   }
   ```

3. 指定初始容量和负载因子

   ```java
   public HashMap(int initialCapacity, float loadFactor) {
       if (initialCapacity < 0)
           throw new IllegalArgumentException("Illegal initial capacity: " +
                                              initialCapacity);
       if (initialCapacity > MAXIMUM_CAPACITY)
           initialCapacity = MAXIMUM_CAPACITY;
       if (loadFactor <= 0 || Float.isNaN(loadFactor))
           throw new IllegalArgumentException("Illegal load factor: " +
                                              loadFactor);
       this.loadFactor = loadFactor;
       this.threshold = tableSizeFor(initialCapacity);
   }
   ```

4. 包含另外一个'Map'的构造函数

   ```java
   public HashMap(Map<? extends K, ? extends V> m) {
       this.loadFactor = DEFAULT_LOAD_FACTOR;
       putMapEntries(m, false);
   }
   final void putMapEntries(Map<? extends K, ? extends V> m, boolean evict) {
       int s = m.size();
       if (s > 0) {
           if (table == null) { // pre-size
               // +1 ---》 尽量减少扩容
               float ft = ((float)s / loadFactor) + 1.0F;
               int t = ((ft < (float)MAXIMUM_CAPACITY) ?
                        (int)ft : MAXIMUM_CAPACITY);
               if (t > threshold)
                   threshold = tableSizeFor(t);
           }
           else if (s > threshold)
               resize();
           for (Map.Entry<? extends K, ? extends V> e : m.entrySet()) {
               K key = e.getKey();
               V value = e.getValue();
               putVal(hash(key), key, value, false, evict);
           }
       }
   }
   ```

## 4.3 成员方法

### 4.3.1 增加方法

大致步骤：

1. 先通过hash值，计算出key的索引。
2. 如果没有冲突，直接插入。
3. 出现碰撞：进行处理
   - 使用红黑树，调用红黑树的方法插入。
   - 普通方法插入，达到转化条件，转为红黑树。
4. 存在重复key，替换为新值。
5. size超过阈值，进行扩容。

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```

此处查看hash方法。

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

hash方法解析：

1. 如果key为空，此时hash直接赋值为0.(hashMap的key可以为null，但只能有一个)

   不同于HashTable，HashTable不可存放key为null的键值对。

   ```java
   // HashTable的put方法
   public synchronized V put(K key, V value) {
       // Make sure the value is not null
       if (value == null) {
           throw new NullPointerException();
       }
   
       // Makes sure the key is not already in the hashtable.
       // 此处由取余计算出索引 然后比较索引位置上的节点的hashCode与val是否与插入值一样，一样则进行覆盖，不一样则使用addEntry方法插入
       Entry<?,?> tab[] = table;
       int hash = key.hashCode();
       int index = (hash & 0x7FFFFFFF) % tab.length; // 把负数可转化为正数
       @SuppressWarnings("unchecked")
       Entry<K,V> entry = (Entry<K,V>)tab[index];
       for(; entry != null ; entry = entry.next) {
           if ((entry.hash == hash) && entry.key.equals(key)) {
               V old = entry.value;
               entry.value = value;
               return old;
           }
       }
   
       addEntry(hash, key, value, index);
       return null;
   }
   ```

2. hash值无符号右移16位再与hash值做按位异或操作。

   可以同时考虑低16位和高16位，使得低16位与高16位进行按位与操作。

**问题1：** 为什么不直接使用hashCode的值？

如果n即数组长度很小，假设为16，则n-1的二进制为：1111. 与hashCode直接进行按位与操作，实际上只使用了后四位，当高位变化很大，低位没变化，很容易造成哈希冲突，高位与低位做与运算后可以解决这个问题。

put方法使用了putVal方法。

```java
	/**
     * Implements Map.put and related methods
     *
     * @param hash hash for key
     * @param key the key
     * @param value the value to put
     * @param onlyIfAbsent if true, don't change existing value
     * @param evict if false, the table is in creation mode.
     * @return previous value, or null if none
     */
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        Node<K,V> e; K k;
        // 是否相等
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        // 是否为红黑树
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            // 遍历链表
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st 是否转为红黑树
                        treeifyBin(tab, hash);
                    break;
                }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        // 替换新值
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;
    // 是否扩容
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}
```

转为红黑树：

```java
final void treeifyBin(Node<K,V>[] tab, int hash) {
    int n, index; Node<K,V> e;
    // 如果数组长度小于MIN_TREEIFY_CAPACITY，则进行扩容
    if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
        resize();
    // 如果为null，不执行操作，putVal中已有判断，所以多线程不安全
    else if ((e = tab[index = (n - 1) & hash]) != null) {
        
        TreeNode<K,V> hd = null, tl = null;
        do {
            // 普通链表节点替换为红黑树节点
            TreeNode<K,V> p = replacementTreeNode(e, null);
            if (tl == null)
                hd = p;
            else {
                p.prev = tl;
                tl.next = p;
            }
            tl = p;
        } while ((e = e.next) != null);
        if ((tab[index] = hd) != null)
            // 红黑树进行旋转
            hd.treeify(tab);
    }
}
```

### 4.3.2 扩容方法

#### 1. 什么时候扩容

- 当HashMap中的元素个数超过数组长度*负载因子时，就会进行扩容。默认数组长度为16，负载因子0.75，所以默认超过12进行扩容，扩容后为原来的2倍，再重新计算每个元素再新数组中的位置，非常耗性能。
- 当其中一个链表的个数达到8个，但是数组长度没有达到64，HashMap会优先使用扩容解决；如果数组长度达到64，则转化为红黑树。

#### 2. 扩容机制

- 进行扩容会伴随着一次重新hash分配，会遍历hash表中的所有元素，非常耗时。
- 扩容时使用的rehash方法非常巧妙，每次都是翻倍，与原来计算的索引值相比，只增加了一个bit位，所以节点要么再原位置，要么位原位置+旧容量。正式由于此设计，省去了重新计算hash的时间。同时可以减少原链表中的产生hash冲突的节点，均匀分散在新空间。

```java
final Node<K,V>[] resize() {
    Node<K,V>[] oldTab = table;
    int oldCap = (oldTab == null) ? 0 : oldTab.length; // 旧容量
    // 旧扩容阈值，如果指定初始容量，则构造函数中初始化默认16；如果是无参构造器，则为0
    // 直接打断点，因为启动器加载其他类，使用到了HashMap，所以此时不是自己使用的，需要再自己项目打断点，然后进入方法再打断点
    int oldThr = threshold;
    int newCap, newThr = 0;
    // 非初始化
    if (oldCap > 0) {
        // 调整阈值最大，无法扩容
        if (oldCap >= MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        // 旧容量扩大一倍还小于最大容量 并且久容量大于等于默认最小容量16
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            // 扩新容量和新阈值
            newThr = oldThr << 1; // double threshold
    }
    // 初始化有参构造器
    else if (oldThr > 0) // initial capacity was placed in threshold
        // 初始化容量为旧阈值 即为16
        newCap = oldThr;
    // 初始化无参构造器
    else {               // zero initial threshold signifies using defaults
        // 默认容量和阈值
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    // 初始化新阈值 有参构造器
    if (newThr == 0) {
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    threshold = newThr;
    @SuppressWarnings({"rawtypes","unchecked"})
    Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    table = newTab;
    if (oldTab != null) {
        // 遍历旧table
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {
                // 取出索引值，置空旧table
                oldTab[j] = null;
                if (e.next == null)
                    // 此处没有链表，直接计算索引插入
                    newTab[e.hash & (newCap - 1)] = e;
                else if (e instanceof TreeNode)
                    // 切分红黑树
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                else { // preserve order
                    // 此处为链表，则遍历
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    do {
                        next = e.next;
                        // 高位为0 在旧位置
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        // 高位为1 在新位置
                        else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```

### 4.3.3 删除操作

```java
public boolean remove(Object key, Object value) {
    return removeNode(hash(key), key, value, true, true) != null;
}
final Node<K,V> removeNode(int hash, Object key, Object value,
                               boolean matchValue, boolean movable) {
    Node<K,V>[] tab; Node<K,V> p; int n, index;
    // 要删除的节点不为null
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (p = tab[index = (n - 1) & hash]) != null) {
        Node<K,V> node = null, e; K k; V v;
        // 找具体节点 第一个就是
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            node = p;
        // 此处有链表或者红黑树
        else if ((e = p.next) != null) {
            // 红黑树
            if (p instanceof TreeNode)
                node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
            // 链表
            else {
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key ||
                         (key != null && key.equals(k)))) {
                        node = e;
                        break;
                    }
                    p = e;
                } while ((e = e.next) != null);
            }
        }
        // 删除节点
        if (node != null && (!matchValue || (v = node.value) == value ||
                             (value != null && value.equals(v)))) {
            // 红黑树
            if (node instanceof TreeNode)
                ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
            // 数组删除
            else if (node == p)
                tab[index] = node.next;
            // 链表删除 node = p.next
            else
                p.next = node.next;
            ++modCount;
            --size;
            afterNodeRemoval(node);
            return node;
        }
    }
    return null;
}
```

### 4.3.4 get方法

删除操作先做get操作。逻辑类似

```java
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}
final Node<K,V> getNode(int hash, Object key) {
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & hash]) != null) {
        // 第一个值就是
        if (first.hash == hash && // always check first node
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        // 链表或者红黑树
        if ((e = first.next) != null) {
            if (first instanceof TreeNode)
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            do {
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}
```

### 4.3.4 HashMap的遍历

1. 分别遍历key和value

   ```java
   // 遍历key
   Set<Object> keys = map.keySet();
   for(Object key: keys){}
   // 遍历values
   Collection<Object> values = map.values();
   for(Object v: values){}
   ```

2. 使用iterator迭代器

   ```java
   Set<Map.Entry<Object, Object>> entries = map.entrySet();
   for(Iterator<Map.Entry<Object, Object>> it=entries.iterator();it.hasNext();){
       Map.Entry<Object, Object> entry = it.next();
       Object key = entry.getKey();
       Object value = entry.getValue();
   }
   ```

3. 通过get方式 不建议使用

   ```java
   Set<Object> keys = map.keySet();
   for(Object key: keys) {
       Object value = map.get(key);
   }
   ```

4. 使用Map接口的默认方法

   ```java
   map.forEach((key, value)->{
       // lamba表达式
   });
   ```

# 其他

HashMap初始化尽量指定大小，为：需要存储的元素个数/负载因子

负载因子尽量使用0.75.