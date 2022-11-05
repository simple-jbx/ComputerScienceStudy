# 注释

[项目Code](https://github.com/simple-jbx/Java8Study)

[Jdk8SrcCode](https://github.com/simple-jbx/SourceCode/tree/main/JAVA/JDK8Src)

# JAVA基础

## 关键字及其作用

|                      |          |            |          |              |            |           |        |       |
| -------------------- | -------- | ---------- | -------- | ------------ | ---------- | --------- | ------ | ----- |
| 分类                 | 关键字   |            |          |              |            |           |        |       |
| 访问控制             | private  | protected  | public   |              |            |           |        |       |
| 类，方法和变量修饰符 | abstract | class      | extends  | final        | implements | interface | native |       |
|                      | new      | static     | strictfp | synchronized | transient  | volatile  |        |       |
| 过程控制             | break    | continue   | return   | do           | while      | if        | else   |       |
|                      | for      | instanceof | switch   | case         | default    |           |        |       |
| 错误处理             | try      | catch      | throw    | throws       | finally    |           |        |       |
| 包相关               | import   | package    |          |              |            |           |        |       |
| 基本类型             | boolean  | byte       | char     | double       | float      | int       | long   | short |
| 特殊值               | null     | void       | true     | false        |            |           |        |       |
| 变量引用             | super    | this       |          |              |            |           |        |       |
| 保留字               | goto     | const      |          |              |            |           |        |       |



## 拆装箱

```java
        //自动拆装箱 实质（字节码）也是调用方法拆装箱
		Integer i = 10;  //装箱
		int n = i;   //拆箱

		//调用方法拆装箱
		Integer integer = Integer.valueOf(1);
        int intValue = integer.intValue();

        Short aShort = Short.valueOf((short) 1);
        short shortValue = aShort.shortValue();

        Long aLong = Long.valueOf(1L);
        long longValue = aLong.longValue();

        Float aFloat = Float.valueOf(1.1f);//1.1 默认double 1.1f表示float
        float floatValue = aFloat.floatValue();

        Double aDouble = Double.valueOf(1.1);
        double doubleValue = aDouble.doubleValue();
        
        Boolean aBoolean = Boolean.valueOf(true);
        boolean booleanValue = aBoolean.booleanValue();

        Byte aByte = Byte.valueOf((byte) 1);
        byte byteValue = aByte.byteValue();

        Character character = Character.valueOf('1');
        char c = character.charValue();

		float f = 1.1f;//1.1编译不能通过 1.1为double 高转低有可能会有精度损失，需要强制转换，
							//但低转高不需要，不会有精度损失
		float ff = 1.1;
        double d = f;
```

package.png

<div align='center'>
    <img src='./imgs/Java/1.8/package.png' width='300px'>
	</br></br>Integer i = 10;int n = i;字节码
</div>


## 常量池

Byte, Short, Integer, Long, Character, Boolean实现了常量池技术

String实现了常量池技术

Float, Double没有实现常量池技术

```java
//常量池
public class Main {
	public static void main(String[] args) {
		Integer a = -128, b = -128;
		System.out.println(a == b); //true 当数字处于[-128,127]区间内是相等的原因是存在常量池
		System.out.println(a.equals(b)); //true
		
        
         //new String()均是在堆上创建的对象，str.intern() 会将str添加到常量池中 并返回引用
		 String s1 = "abcdefg"; //直接放在常量池
         String s2 = new String("abcdefg"); //堆上创建
         String s3 = new String(s1); //堆上创建
         String s4 = "abcdefg"; //常量池
         String s5 = "abc" + "defg"; //常量
         String s6 = "abc" + new StringBuffer("defg").toString();


         System.out.println(s1 == s2); //false
         System.out.println(s1 == s3); //false
         System.out.println(s2 == s3); //false
         System.out.println(s1 == s4); //true
         System.out.println(s1 == s5); //true 常量 编译器就确定了
         System.out.println(s1 == s6); //false

        
         System.out.println(s1.equals(s2)); //true
         System.out.println(s1.equals(s3)); //true
         System.out.println(s2.equals(s3)); //true
         System.out.println(s1.equals(s4)); //true
         System.out.println(s1 == s2.intern()); //true

	}
}
```

## 创建对象四种方式

- new

- clone

- 反射

    - ```java
        Class clazz = Class.forName("yunche.test.Hello");
        Hello hello =(Hello) clazz.newInstance();
        ```

- 序列化

## String

### 常用API

- **char charAt(int index)**

	返回给定位置的代码单元。

- int codePointAt(int index) 5.0

	返回从给定位置开始的码点。

- int offsetByCodePoints(int startIndex, int cpCount) 5.0

	返回从startIndex位置的码点开始，位移cpCount后的码点索引。

- int compareTo(String other)

	按照字典顺序，如果字符串位于other之前，返回一个负数；如果字符串位于other之后，返回一个正数；如果两个字符串相等返回0。

- IntStream codePoints() 8.0

	将这个字符串的码点作为一个流返回。调用toArray将它们放在一个数组中。

- new String(int[] codePoints, int offset, int count) 5.0

	用数组中从 offset 开始的 count 个码点构造一个字符串。

- **boolean equals(0bject other)**

	如果字符串与 other 相等， 返回 true。

- boolean equalsIgnoreCase(String other)

	如果字符串与 other 相等 （ 忽略大小写，) 返回 tme。

- **boolean startsWith(String prefix )**

- **boolean endsWith(String suffix )**

	如果字符串以 suffix 开头或结尾， 则返回 true。

- int indexOf(String str) 

- int indexOf(String str, int fromlndex ) 

- int indexOf(int cp) 

- int indexOf(int cp, int fromlndex ) 

	返回与字符串 str 或代码点 cp 匹配的第一个子串的开始位置。这个位置从索引 0 或 fromlndex 开始计算。 如果在原始串中不存在 str， 返回 -1。

- int 1astIndexOf(String str) 

- int 1astIndexOf(String str, int fromlndex ) 

- int lastindexOf(int cp) 

- int 1astindexOf(int cp, int fromlndex ) 

	返回与字符串 str 或代码点 cp 匹配的最后一个子串的开始位置。 这个位置从原始串尾 端或 fromlndex 开始计算。 

- **int 1ength( )** 

	返回字符串的长度。 

- int codePointCount(int startlndex , int endlndex ) 5.0 

	返回 startlndex 和 endludex-l之间的代码点数量。没有配成对的代用字符将计入代码点。 

-  **String replace( CharSequence oldString,CharSequence newString)** 

	返回一个新字符串。这个字符串用 newString 代替原始字符串中所有的 oldString。可 以用 String 或 StringBuilder 对象作为 CharSequence 参数。 

- **String substring(int beginlndex )** 

- **String substring(int beginlndex, int endlndex )** 

	返回一个新字符串。这个字符串包含原始字符串中从 beginlndex 到串尾或 endlndex-l 的所有代码单元。

- **String toLowerCase( )** 

-  **String toUpperCase( )** 

	返回一个新字符串。 这个字符串将原始字符串中的大写字母改为小写，或者将原始字 符串中的所有小写字母改成了大写字母。

- **String trim( )** 

	返回一个新字符串。这个字符串将删除了原始字符串头部和尾部的空格。

- String join(CharSequence delimiter, CharSequence ... elements ) 8 

	返回一个新字符串， 用给定的定界符连接所有元素。

### String、StringBuilder和StringBuffer区别

JDK1.9之前为char[]，JDK1.9之后为byte[]

|              | String | StringBuilder | StringBuffer |
| ------------ | ------ | ------------- | ------------ |
| 是否可修改   | 否     | 是            | 是           |
| 效率         | 低     | 高            | 中           |
| 是否线程安全 | 是     | 否            | 是           |

### String

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
	    private final char value[]; //底层为char数组 final修饰不可变 线程安全
}
```

### StringBuilder & StringBuffer

```java
abstract class AbstractStringBuilder implements Appendable, CharSequence {
    /**
     * The value is used for character storage.
     */
    char[] value;
}
```

```java
public final class StringBuilder
    extends AbstractStringBuilder
    implements java.io.Serializable, CharSequence
{
 	public StringBuilder() {
        super(16);
    }
    
    public StringBuilder(int capacity) {
        super(capacity);
    }
    
    public StringBuilder(String str) {
        super(str.length() + 16);
        append(str);
    }
    
    public StringBuilder(CharSequence seq) {
        this(seq.length() + 16);
        append(seq);
    }
}
```

```java
 public final class StringBuffer
    extends AbstractStringBuilder
    implements java.io.Serializable, CharSequence
{
    private transient char[] toStringCache; //防止序列化和反序列化
     
    public StringBuffer() {
        super(16);
    }
     
    public StringBuffer(int capacity) {
        super(capacity);
    }
     
    public StringBuffer(String str) {
        super(str.length() + 16);
        append(str);
    }
     
    public StringBuffer(CharSequence seq) {
        this(seq.length() + 16);
        append(seq);
    }
     
    /**
    通过对方法加锁来保证线程安全
    **/
    @Override
    public synchronized int length() {
        return count;
    }
}
```



## 重载与重写的区别

重载：发生在同一个类中，方法名相同参数列表不同（参数类型不同、个数、顺序（类型也不同）不同），与方法返回值和访问修饰符无关，即重载的方法不能根据返回类型进行区分。

重写：发生在父子类中，方法名、参数列表必须相同，返回值小于等于父类，抛出的异常小于等于父类，访问修饰符大于等于父类（里式替换原则）；如果父类方法访问修饰符为private则子类中就不是重写。

构造器（Constructor）可以被重载，不能被重写

重载的方法不能根据返回类型进行区分，函数调用时不能指定返回类型，编译器不知道要调用哪个函数。

## 面向对象三大特征

封装、继承、多态

### 封装

封装是指把一个对象的状态信息（也就是属性）隐藏在对象内部，不允许外部对象直接访问对象的内部信息。但是可以提供一些可以被外界访问的方法来操作属性。

### 继承

不同类型的对象，相互之间经常有一定数量的共同点。每一个对象还定义了额外的特性使得他们与众不同。继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承，可以快速地创建新的类，可以提高代码的重用，程序的可维护性，节省大量创建新类的时间 ，提高我们的开发效率。

- 子类拥有父类对象所有的属性和方法（包括私有属性和私有方法），但是父类中的私有属性和方法子类是无法访问，**只是拥有**。
- 子类可以拥有自己属性和方法，即子类可以对父类进行扩展。
- 子类可以用自己的方式实现父类的方法。

### 多态

表示一个对象具有多种的状态。具体表现为父类的引用指向子类的实例。

## JAVA反射

反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能成为反射机制。

优点：运行期类型的判断，动态加载类，提高代码灵活度

缺点：性能比直接的代码要慢

应用场景：Spring中的xml的配置模式、动态代理设计模式也采用了反射机制

## 异常

<div align='center'>
    <img src='./imgs/Java/1.8/JAVAThrowable.svg' width='1000px'>
    </br></br>JAVA异常
</div>

# 并发、锁

## Atomic原子类型

具有原子/原子特征的类。

<div align='center'>
    <img src='./imgs/Java/1.8/JUC.svg' width='500px'>
    </br></br>JUC包下包含的类（JDK1.8）
</div>


根据操作的类型可以将JUC包中的原子类分为4类。

### 基本类型

使用原子方式更新基本类型

#### [AtomicInteger 整型原子类]

##### 常用方法

```java
public final int get() //获取当前的值
public final int getAndSet(int newValue)//获取当前的值，并设置新的值
public final int getAndIncrement()//获取当前的值，并自增
public final int getAndDecrement() //获取当前的值，并自减
public final int getAndAdd(int delta) //获取当前的值，并加上预期的值
boolean compareAndSet(int expect, int update) //如果输入的数值等于预期值，则以原子方式将该值设置为输入值（update）
public final void lazySet(int newValue)//最终设置为newValue,使用 lazySet 设置之后可能导致其他线程在之后的一小段时间内还是可以读到旧的值。
```

```java
 // setup to use Unsafe.compareAndSwapInt for updates
private static final Unsafe unsafe = Unsafe.getUnsafe();
private static final long valueOffset;

static {
    try {
        valueOffset = unsafe.objectFieldOffset
            (AtomicInteger.class.getDeclaredField("value"));
    } catch (Exception ex) { throw new Error(ex); }
}

private volatile int value;
```

AtomicInteger 类主要利用 CAS (compare and swap) + volatile 和 native 方法来保证原子操作，从而避免 synchronized 的高开销，执行效率大为提升。

CAS的原理是拿期望的值和原本的一个值作比较，如果相同则更新成新的值。UnSafe 类的 objectFieldOffset() 方法是一个本地方法，这个方法是用来拿到“原来的值”的内存地址。另外 value 是一个volatile变量，在内存中可见，因此 JVM 可以保证任何时刻任何线程总能拿到该变量的最新值。

#### AtomicLong 长整型原子类

#### AtomicBoolean 布尔型原子类

### 数组类型

使用原子方式更新数组中的某个元素

#### AtomicIntegerArray 整型数组原子类

#### AtomicLongArray 长整型数组原子类

#### AtomicReferenceArray  引用类型数组原子类

### 引用类型

#### AtomicReference：引用类型原子类

#### AtomicMarkableReference：原子更新带有标记的引用类型。该类将 boolean 标记与引用关联起来。

#### AtomicStampedReference ：原子更新带有版本号的引用类型。该类将整数值与引用关联起来，可用于解决原子的更新数据和数据的版本号，可以解决使用 CAS 进行原子更新时可能出现的 ABA 问题。

### 对象的属性修改类型

#### AtomicIntegerFieldUpdater:原子更新整型字段的更新器

#### AtomicLongFieldUpdater：原子更新长整型字段的更新器

#### AtomicReferenceFieldUpdater：原子更新引用类型里的字段



## 锁

### [锁的级别](https://www.cnblogs.com/wuqinglong/p/9945618.html)

- 无锁状态
- 偏向锁状态
- 轻量级锁状态
- 重量级锁状态

锁可以升级，但不能降级

### 重入锁

当某个线程请求一个由其他线程持有的锁时，发出请求的线程就会阻塞。然而，由于内置锁是可重入的，因此如果某个线程视图获得一个已经由它自己所持有的锁，那么这个请求就会成功。“重入”意味着获取锁的操作粒度是“线程”，而不是“调用”。重入的一种实现方法是，为每个锁关联一个获取计数值和一个所有者线程。当计数值为0时，这个锁就认为是没有被任何线程持有。当线程请求一个未被持有的锁时，JVM将记下锁的持有者，并且将获取计数值置为1.如果同一个线程再次获取这个锁，计数值将递增，而当线程退出同步代码块时，计数器会相应递减。当计数值为0时，这个锁将被释放。

<div align='center'>
    <img src='./imgs/Java/1.8/JAVALock.svg' width='800px'>
    </br></br><a href='https://zhuanlan.zhihu.com/p/56512421'>JAVA锁</a>
</div>

# 集合

[JAVA集合源码](https://github.com/simple-jbx/SourceCode/tree/main/JAVA/JDK8Src/java/util/Collection.java)

<div align='center'>
    <img src='./imgs/Java/1.8/Collection01.png' width='1200px'>
    <img src='./imgs/Java/1.8/Collection02.png' width='1200px'>
    </br></br>JAVA集合
</div>



## List、Set、Map三者区别

- List：有序集合（存入与取出顺序相同），可存储重复元素，可存储多个null

- Set：无序集合（存入和取出顺序不一定相同），不可存储重复元素，只能存储一个null

- Map：使用键值对的方式对元素进行存储，key是无序的，且是唯一的。value值不唯一。不同的key值可以对应相同的value值。

## List

## Set

### HashSet

底层是HashMap，默认构造函数是构建一个初始容量为16，负载因子为0.75的HashMap。HashSet的值存放于HashMap的key上，HashMap的value统一为PRESENT。

HashSet如何保证数据的不可重复？

通过HashCode()和equals()两个方法

## Map

### [HashMap](https://github.com/simple-jbx/SourceCode/tree/main/JAVA/JDK8Src/java/util/HashMap.java)

```java
//初始容量 16
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;
//最大容量 1 << 30
static final int MAXIMUM_CAPACITY = 1 << 30;
//默认加载因子 0.75
static final float DEFAULT_LOAD_FACTOR = 0.75f;
//转红黑树的链表阈值
static final int TREEIFY_THRESHOLD = 8;
//转链表时候红黑树节点个数阈值
static final int UNTREEIFY_THRESHOLD = 6;
//数组最小容量
static final int MIN_TREEIFY_CAPACITY = 64;


/* ---------------- Fields -------------- */

/**
* The table, initialized on first use, and resized as
* necessary. When allocated, length is always a power of two.
* (We also tolerate length zero in some operations to allow
* bootstrapping mechanics that are currently not needed.)
*/
transient Node<K,V>[] table;

//扩容门限值 capacity * load factor 当前容量到达这个值的时候进行resize()
int threshold;

/**
* The number of key-value mappings contained in this map.
*/
transient int size;

/**
* The number of times this HashMap has been structurally modified
* Structural modifications are those that change the number of mappings in
* the HashMap or otherwise modify its internal structure (e.g.,
* rehash).  This field is used to make iterators on Collection-views of
* the HashMap fail-fast.  (See ConcurrentModificationException).
*/
transient int modCount;


//Node节点 实际上还是实现了Map.Entry接口 部分代码
static class Node<K,V> implements Map.Entry<K,V> {
    //判断两个节点值是否相等
    //先判断引用是否相等  然后判断是否为Map.Entry类型 在调用Map.Entry equals方法
    public final boolean equals(Object o) {
            if (o == this)
                return true;
            if (o instanceof Map.Entry) {
                Map.Entry<?,?> e = (Map.Entry<?,?>)o;
                if (Objects.equals(key, e.getKey()) &&
                    Objects.equals(value, e.getValue()))
                    return true;
            }
            return false;
    }
    
    //计算key的hash值 如果key为null则hash值为0放在数组索引为0的位置上
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
}
```



#### **HashMap底层实现**

1.7之前数组+链表，1.8之后数组+链表+红黑树

#### HashMap扩容

<div align='center'>
    <img src='./imgs/Java/1.8/HashMapResize.png' width='1400px'>
    <br/><br/>HashMap扩容
</div>





```java
    final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
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
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
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

#### HashMap Put

<div align='center'>
    <img src='./imgs/Java/1.8/HashMapPut.svg' width='1800px'>
    <br/><br/>HashMap Put
</div>





```java
    /**
     * Associates the specified value with the specified key in this map.
     * If the map previously contained a mapping for the key, the old
     * value is replaced.
     *
     * @param key key with which the specified value is to be associated
     * @param value value to be associated with the specified key
     * @return the previous value associated with <tt>key</tt>, or
     *         <tt>null</tt> if there was no mapping for <tt>key</tt>.
     *         (A <tt>null</tt> return can also indicate that the map
     *         previously associated <tt>null</tt> with <tt>key</tt>.)
     */
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }

    /**
     * Implements Map.put and related methods.
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
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }

    /**
     * Replaces all linked nodes in bin at index for given hash unless
     * table is too small, in which case resizes instead.
     */
    final void treeifyBin(Node<K,V>[] tab, int hash) {
        int n, index; Node<K,V> e;
        if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
            resize();
        else if ((e = tab[index = (n - 1) & hash]) != null) {
            TreeNode<K,V> hd = null, tl = null;
            do {
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
                hd.treeify(tab);
        }
    }
```

**HashMap与HashSet的区别**

| HashMap                     | HashSet                             |
| --------------------------- | ----------------------------------- |
| 实现了Map接口               | 实现了Set接口                       |
| 存储键值对                  | 存储对象                            |
| key唯一，value不唯一        | 存储对象唯一                        |
| HashMap使用键值计算Hashcode | HashSet使用成员对象来计算hashcode值 |
| 速度相对较快                | 速度相对较慢                        |

#### ConcurrentHashMap

- JDK1.7

    Segment数组+HashEntry数组方式实现，Segment实现了ReentrantLock，所以Segment有锁的性质，HashEntry用于存储键值对。

- JDK1.8

    类似HashMap，Node数组+链表/红黑树，采用CAS+synchronized来保证线程安全。synchronized只锁链表或红黑树的头结点，锁的粒度更细，竞争变小，效率提高。

<div align='center'>
    <img src='./imgs/Java/1.8/20210921163557.png' width='1200px'>
    </br></br>JDK1.7 ConcurrentHashMap
    <img src='./imgs/Java/1.8/20210921163736.png' width='1200px'>
    </br></br>JDK1.8 ConcurrentHashMap
</div>

#### HashTable

数组+链表，链表解决哈希冲突，整个数组都是synchronized修饰的，线程安全，锁的粒度太大，竞争激烈，效率低

<div align='center'>
    <img src='./imgs/Java/1.8/20210921164630.png' width='1200px'>
    </br></br>HashTable
</div>


#### HashMap、ConcurrentHashMap、HashMap的区别

|                              | HashMap（JDK1.8）                 | ConcurrentHashMap（JDK1.8）      | Hashtable                      |
| ---------------------------- | --------------------------------- | -------------------------------- | ------------------------------ |
| 底层实现                     | 数组+链表/红黑树                  | 数组+链表/红黑树                 | 数组+链表                      |
| 线程安全                     | 不安全                            | 安全（synchronized修饰Node节点） | 安全（synchronized修饰整个表） |
| 效率                         | 高                                | 较高                             | 低                             |
| 扩容                         | 初始16，每次扩容为原来的2倍       | 初始16，每次扩容为原来的2倍      | 初始                           |
| 是否支持null key和null value | 可以有一个null key 多个null value | 不支持                           | 不支持                         |

## Collections 工具类

- Collections是一个操作Set、List和Map等集合的工具类
- Collections中提供了一些列静态的方法对集合元素进行排序、查询和修改等操作，还提供了对集合对象设置不可变、同步控制等方法
- 常用操作（均为static方法）
    - **reverse(List):**反转List中元素的顺序
    - **shuffle(List):**对List集合元素进行随机排序
    - **sort(List):**根据元素的自然顺序对指定List集合元素按升序排序
    - **sort(List, Comparator):**根据指定的Comparator产生的顺序对List集合元素进行排序
    - **swap(List, int, int):**将指定list集合中的i处元素和j处元素进行交换
    - **Object max(Collection)**:根据元素自然排序，返回给定集合中的最大元素
    - **Object max(Collection, Comparator)**:根据Comparator指定的顺序，返回给定集合中的最大元素
    - **Object min(Collection)**
    - **Object min(Collection, Comparator)**
    - **int frequency(Collection, Object)**:返回指定集合中指定元素的出现次数
    -  **void copy(List dest, List src)**:将src中内容复制到dest中
    - **boolean replaceAll(List list, Object oldVal, Object newVal)**:用newVal替换所有的oldVal
    - 将指定集合包装成线程同步的集合
        - **synchronizedCollection(Collection<T> c)**
        - **synchronizedList(List<T> list)**
        - **synchronizedMap(Map<K,V> m)**
        - **synchronizedSet(Set<T> s)**
        - **synchronizedNavigableMap(NavigableMap<K,V> m)**
        - **synchronizedNavigableSet(NavigableSet<T> s)**
        - **synchronizedSortedMap(SortedMap<K,V> m)**
        - **synchronizedSortedSet(SortedSet<T> s)**



# 泛型

## 简介

### 概念

允许在定义类、接口时通过一个标识表示类中某个属性的类型或者是某个方法的返回值及参数类型。这个类型参数将在使用时（例如继承或实现这个接口，用这个类型声明变量、创建对象时）确定（传入实际的类型参数，也称为类型实参）。

### 为什么要有泛型，直接使用Object也可以存储？

- 解决元素存储的安全性问题（类似食品、药品标签）。
- 获取元素时不用类型强制转换（不用每次都辨别类型）。

## 泛型类

1. 泛型类可能有多个参数，此时应将多个参数放在一个`<>`内，例如：`<E1,E2,E3>`。
2. 泛型类构造器与普通类一样。
3. 实例化后，操作原来泛型文职的结构必须与指定的泛型类型一致。
4. 泛型不同的引用不能相互赋值。
5. 泛型如不指定，将被擦除，当做Object处理，但不等价于Object。
6. 如果泛型类是一个接口或抽象类，则不可创建泛型类的对象。
7. 泛型的指定中不能使用基本数据类型，可以使用包装类替换。
8. 在静态方法中不能使用类的泛型。
9. 异常类不能是泛型的。
10. 不能使用 new E[]。但可以：`E[] elements = (E[]) new Object[capacity];`。
11. 父类有泛型，子类可以选择保留（部分保留或全部保留）也可指定泛型（不指定，泛型擦除，认为是Object）。

## 泛型方法

`<T> T[] toArray(T[] a)`

## 通配符

`?`

```java
    public void print(List<?> list) {
        Iterator<?> iterator = list.listIterator();
        while (iterator.hasNext()) {
            Object obj = iterator.next();
            System.out.println(obj);
        }
    }
```

### 数据读取和写入要求

不允许添加（除了null），允许读，读取的数据类型为Object。

### 有限制条件的通配符

- 通配符指定上限
    - 上限extends：使用时指定的类型必须是继承某个类，或实现某个接口，即 `<=`
- 通配符指定下限
    - 下限super：使用时指定的类型不能小于操作的类，即 `>=`
- 例如
    - `<? extends Number>` (无穷小, Number]
        - 只允许泛型问Number及Number子类的引用调用
    - `<? extends Number>` [Number, 无穷大)
        - 只允许泛型为Number及Number父类的引用调用
    - <? extends Comparable>
        - 只允许泛型为实现Comparable接口的实现类的引用调用

# IO流

涉及的源码在[java.io](https://github.com/simple-jbx/SourceCode/tree/main/JAVA/JDK8Src/java/io)包下

## File

[java.io.File](https://github.com/simple-jbx/SourceCode/blob/main/JAVA/JDK8Src/java/io/File.java)

### 创建

```java
public File(String pathname);
public File(URI uri);
public File(String parent, String child);
public File(File parent, String child);  
```

### 获取

```java
//获取name
public String getName();
//获取上层文件目录路径。若无，返回null。
public String getParent();
//获取上层文件目录File，若无，则返回null。
public File getParentFile();
//获取路径（new的时候的路径）
public String getPath();
//获取绝对路径
public String getAbsolutePath();
public File getAbsoluteFile();
//获取该平台下标准绝对路径
public String getCanonicalPath();
public File getCanonicalFile();
//最后一次修改的时间，ms
public long lastModified();
//获取文件的长度（B）
public long length();
//获取指定目录下的所有文件或文件目录的名称数组
public String[] list();
public File[] listFiles();
```

### 重命名

```java
//把文件重命名到指定路径 oriFile需存在，destFile不存在
public boolean renameTo(File dest);
```

### 判断

```java
public boolean canRead();
public boolean canWrite();
public boolean exists();
public boolean isDirectory();
public boolean isFile();
public boolean isHidden();
```

### 创建

```java
public boolean createNewFile();
//创建文件目录，已存在或上层不存在则不创建
public boolean mkdir();
//上层目录不存在也一并创建
public boolean mkdirs();
//创建临时文件
public static File createTempFile(String prefix, String suffix,
                                      File directory);
public static File createTempFile(String prefix, String suffix);
```

### 删除

```java
//删除文件或文件夹，不走回收站，要删除一个文件目录，则该目录只能为空
public boolean delete();
//be deleted when the virtual machine terminates
public void deleteOnExit();
```

## 标准IO流原理及流的分类

- 用于处理设备之间的数据传输；
- java程序中对于数据的输入/输出操作以“流（Stream）”的方式进行；
- java.io包下提供了各种“流”类和接口，用以获取不同种类的数据，并通过标准的方法输入或输出数据。

### 流的分类

- 按照操作==数据单位==不同：字节流（B，8bit），字符流（2B，16bit）

- 按照数据流的==流向==不同：输入/输出流

- 按照流的==角色==不同：节点流，处理流

- | （抽象基类） | 字节流       | 字符流 |
    | ------------ | ------------ | ------ |
    | 输入流       | InputStream  | Reader |
    | 输出流       | OutputStream | Writer |

### IO流体系

| 分类       | 字节输入流              | 字节输出流               | 字符输入流            | 字符输出流             |
| ---------- | ----------------------- | ------------------------ | --------------------- | ---------------------- |
| 抽象基类   | **InputStream**         | **OutputStream**         | **Reader**            | **Writer**             |
| 访问文件   | **FileInputStream**     | **FileOutputStream**     | **FileReader**        | **FileWriter**         |
| 访问数组   | ByteArryInputStream     | ByteArryOutputStream     | CharArrayReader       | CharArrayWriter        |
| 访问管道   | PipedInputStream        | PipedOutputStream        | PipedReader           | PipedWriter            |
| 访问字符串 |                         |                          | StringReader          | StringWriter           |
| 缓冲流     | **BufferedInputStream** | **BufferedOutputStream** | **BufferedReader**    | **BufferedWriter**     |
| 转换流     |                         |                          | **InputStreamReader** | **OutputStreamWriter** |
| 对象流     | **ObjectInputStream**   | **ObjectOutputStream**   |                       |                        |
|            | FileInputStream         | FileOutputStream         | FilterReader          | FilterWriter           |
| 打印流     |                         | PrintStream              |                       | PrintWriter            |
| 推回流     | PushbackInputStream     |                          | PushbackReader        |                        |
| 特殊流     | DataInputStream         | DataOutputStream         |                       |                        |

对于文本类型的数据需要使用字符流来处理，对于图片等数据需要使用字节流来处理。

#### 转换流

**InputStreamReader：**将一个字节的输入流转换为字符的输入流

**OutputStreamWriter：**将一个字符的输出流转换为字节的输出流

作用：提供字节流与字符流之间的转换

解码：字节、字节数组 --> 字符数组、字符串

编码：字符数组、字符串 --> 字节、字节数组

#### 标准输入输出流

- System.in（InputStream）和System.out（类型是PrintStream，FilterOutputStream的子类）分别代表系统标准的输入和输出设备

- 默认输入设备：键盘，默认输出设备：显示器

- 重定向：通过System类的setIn、setOut方法对默认设备进行改变。

    - ```java
        public static void setIn(InputStream in)
        public static void setOut(PrintStream out)
        ```

#### 打印流

- 将基本数据类型的数据格式化为字符串输出
- PrintStream和PrintWriter
    - 提供了一系列print()和println()方法，用于多种数据类型的输出
    - 输出不会爆出IOException异常
    - 有自动flush功能
    - PringStream打印的所有字符都使用平台的默认字符编码转换为字节。在需要写入字符而不是写入字节的情况下，应该使用PrintWriter类。
    - System.out返回的是PringStream的实例

#### 数据流

- 方便操作Java的基本数据类型和String数据，可以使用数据流。
- DataInputStream和DataOutputStream、分别“套接”在InputStream和OutputStream子类的流上

#### 对象流

- ObjectInputStream和ObjectOutPutStream
- 用于存储和读取基本数据类型数据或对象的处理流。可以把Java中的对象写入到数据源中，也能从数据源中换源对象。
- 序列化：用ObjectOutPutStream类保存基本类型数据或对象的机制
    - serialVersionUID用来说明类的不同版本之间的兼容性。目的是对序列化对象进行版本控制。
- 反序列化：用ObjectInputStream类读取基本类型数据或对象的机制
- 不能序列化static和transient修饰的成员变量

#### 随机存取文件流(RandomAccessFile)

- RandomAccessFile（java.io）,并且实现了DataInput和DataOutput这两个接口，意味着这个类既可以读也可以写。
- 支持随机访问，程序可以直接跳到文件的任意位置来读、写文件
    - 支持只访问文件的部分内容
    - 可以像已存在的文件后追加部分内容
- 包含一个记录指针，用以标示当前读写处的位置，可自由移动记录指针
    - long getFilePointer()：获取文件记录指针的当前位置
    - void seek(long pos)：将文件记录指针定位到pos位置
- 创建类的时候需要指定mode（访问方式）：
    - r：只读（文件不存在抛异常）
    - rw：读写（文件不存在创建文件）
    - rwd：读写，同步文件内容更新（立即保存到硬盘）
    - rws：读写，同步文件内容和元数据

## BIO、NIO、AIO

[BIO、NIO和AIO的区别、三种IO的原理与用法](https://blog.csdn.net/u010541670/article/details/91890649)

[Java中BIO，NIO，AIO总结](https://zhuanlan.zhihu.com/p/159276195)

### BIO(blocking I/O)

同步阻塞，一个连接为一个线程，连接一个客户端就需要启动一个线程进行处理，如果连接未断开且未做任何事，会造成不必要的开销。可通过线程池优化。相关类在java.io包中。

适用于连接数目比较小且相对固定的架构，对服务器的要求比较高，对并发有局限性。JDK1.4之前唯一的选择。

<div align='center'>
    <img src='./imgs/Java/1.8/BIO.jpg' width='500px'>
    </br></br>BIO
</div>



**缺点**：

- 每个请求都需要创建独立的线程
- 当并发量大的时候，需要创建大量的线程，占用系统资源
- 连接建立后，如果当前线程暂时没有数据刻度，则线程就阻塞在Read操作上，造成资源浪费

### NIO(non-blocking IO)

JDK1.4开始，Java提供了一系列改进的输入/输出的新特性，统称为NIO(New IO)，同步非阻塞。

JDK7，NIO.2。

相关类在java.nio包及子包下，并对原java.io包中的很多类进行了改写。

NIO有三大核心部分：Channel，Buffer，Selector。

NIO面向缓冲区，数据读到一个它稍后处理的缓冲区中。

非阻塞模式，使一个线程从某通道发送请求或者读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用，就什么都不获取，会继续保持线程阻塞，直至数据变的可以读取之前，该线程可以继续做其他的事情。非阻塞写也是如此，一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。

NIO可以用一个线程处理多个操作，并发能力远远大于BIO。

<div align='center'>
    <img src='./imgs/Java/1.8/NIO.jpg' width='300px'>
    </br></br>NIO Selector、Channel、Buffer关系图
</div>



- 每个Channel都会对应一个Buffer
- Selector对应一个线程，一个线程对应多个Channel
- 线程切换到哪个Channel是由事件决定的
- 数据读取、写入通过Buffer，可以双向，需要flip方法切换

#### NIO.2

Path、Paths、Files

### AIO

异步非阻塞，无需线程主动切换，在相应的状态改变后，系统会同志对应的线程处理。

BIO、NIO、AIO对比



|          | BIO      | NIO                    | AIO        |
| -------- | -------- | ---------------------- | ---------- |
| IO模型   | 同步阻塞 | 同步非阻塞（多路复用） | 异步非阻塞 |
| 编程难度 | 简单     | 复杂                   | 复杂       |
| 可靠性   | 差       | 好                     | 好         |
| 吞吐量   | 低       | 高                     | 高         |

### 各自应用场景

BIO适用于连接数目比较小且固定的架构，对服务器资源要求比较高，并发局限于应用中。

NIO适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程较复杂。

AIO适用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂。

# Lambada

一个匿名函数，可以理解为一段可以传递的代码，使java语言表达能力得到了提升。

本质：作为接口的实例。

##  Lambada表达式的使用

例：`(o1, o2) -> Integer.compare(o1,o2);`

格式：

​	lambada操作符或箭头操作符

​	左边：lambada形参列表（接口中的抽象方法的形参列表）

​	右边：lambada体（重写的抽象方法的方法体）

使用：

1. 无参，无返回值

    - `Runnable r1 = () -> {System.out.println("Hello Lambad!")};`

2. 有一个参数，无返回值

    - `Consumer<String> con = (String str) -> {System.out.println(str);};`

3. 数据类型可以省略，可由编译器推断出，称为“类型推断”

    - `Consumer<String> con = (str) -> {System.out.println(str);};`

4. 只有一个参数时，参数的小括号可以省略

    - `Consumer<String> con = str -> {System.out.println(str);};`

5. 有两个或两个以上的参数，多条执行语句，并且可以有返回值

    - ```java
        Comparator<Integer> com = (x, y) -> {
        	System.out.pringln("实现函数式接口方法！");
        	return Integer.compare(x, y);
        }
        ```

6. 当只有一条语句时，return与大括号可以省略

    - `Comparator<Integer> com = (x, y) -> Integer.compare(x, y)`;

# 函数式（Functional）接口

## 简介

- 只包含一个抽象方法的接口，称为函数式接口。
- 可以通过Lambada表达式创建该接口的对象。（如果Lambada表达式抛出一个受检异常，即非运行时异常，那么该异常需要在目标接口的抽象方法上进行声明）
- 可以在一个接口上使用`@FunctionalInterface`注解，这样可以检查它是否是一个函数式接口。
- 在`java.util.function`包下定义了Java8丰富的函数式接口。

## 如何理解

- java不仅支持OOP（面向对象）还支持OOF（面向函数编程）。
- 在java中，Lambada是对象，必须依附于一类特别的对象类型--函数式接口。
- Lambada表达式就是一个函数式接口的实例。
- 以前用匿名实现类表示的均可以用Lambada表达式替换。

## Java内置四大核心函数式接口

| 函数式接口                 | 参数类型 | 返回类型  | 用途                                                         |
| -------------------------- | -------- | --------- | ------------------------------------------------------------ |
| `Consumer<T>`  消费性接口  | `T`      | `void`    | 操作`T`类型对象，无返回值，`void accept(T t)`                |
| `Supplier<T>`  供给型接口  | 无       | `T`       | 返回 `T`类型对象，`T get()`                                  |
| `Function<T,R>` 函数型接口 | `T`      | `R`       | 操作`T`类型对象，有返回值，`R apply(T t)`                    |
| `Predicate<T>` 断定型接口  | `T`      | `boolean` | 确定`T`类型对象是否满足约束，并返回boolean值，`boolean test(T t)` |

## 其他接口

| 函数式接口                                                 | 参数类型 | 返回类型            | 用途                                                         |
| ---------------------------------------------------------- | -------- | ------------------- | ------------------------------------------------------------ |
| `BiFunction<T,U,R>`                                        | `T,U`    | `R`                 | 对类型为`T,U`参数应用操作，返回`R`类型的结果，`R apply(T t, U u)` |
| `UnaryOperator<T>`(`Function`子接口)                       | `T`      | `T`                 | 对`T`类型对象进行一元运算，返回T类型结果，`T apply(T t)`     |
| `BinaryOperator<T>`(`BiFunction`子接口)                    | `T,T`    | `T`                 | 对`T`类型对象进行二元运算，并返回`T`类型结果，`T apply(T t1， T t2)` |
| `BiConsumer<T,U>`                                          | `T,U`    | `void`              | 操作`T,U`对象，无返回值，`void accept(T t, U u)`             |
| `BiPredicata<T, U>`                                        | `T,U`    | `boolean`           | `boolean test(T t, U u)`                                     |
| `ToIntFunction<T>、ToLongFunction<T>、ToDoubleFunction<T>` | `T`      | `int、long、double` | 分别计算`int、long、double`值的函数                          |
| `IntFunction<T>`                                           | `int `   | `R`                 | 参数为int类型                                                |
| `LongFunction<T>`                                          | `long`   | `R`                 | 参数为long类型                                               |
| `DoubleFunction<R>`                                        | `double` | `R`                 | 参数为double类型                                             |

# 方法引用与构造器引用

- 当要传递给Lambada体的操作，已经有实现的方法了，可以使用方法引用。
- 方法引用可以看做是Lambada表达式深层次的表达。
- 要求：实现接口的抽象方法的参数列表和返回值类型，必须与方法引用的方法的参数列表和返回值类型保持一致。
- 格式：使用操作符`::`将类（或对象）与方法名分隔开。
- 例如：
    - 对象`::`实例方法名
    - 类`::`静态方法名
    - 类`::`实例方法名

# Java Stream

## 简介

Stream是Java8中处理集合的关键抽象概念。Stream API提供了一种高效且易于使用的处理数据的方式。

为什么使用StreamAPI：实际开发中，项目有多个数据源(Mysql、Oracle、MongDB、Redis等)，这些NoSQL的数据就需要Java层面处理。

Stream（面向CPU计算）和Collection集合（面向内存存储）的区别：==Collection是一种静态的内存数据结构，而Stream是有关计算的。==

### Stream是什么

是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。

集合针对数据存储，Stream针对计算。

注意：

- Stream自己不会存储元素。
- Stream不会改变源对象。它会返回一个持有结果的新Stream。
- Stream操作是延迟执行的，这意味着会等到需要结果的时候才执行。

### 操作三个步骤

1. 创建Stream
    - 一个数据源（集合、数组），获取一个流。
2. 中间操作
    - 一个中间操作链，对数据源的数据进行处理。
3. 终止操作（终端操作）
    - 一旦执行终止操作，就==执行中间操作链==，并产生结果。之后，不会再被使用。

## Stream使用

### Stream实例化

#### 通过集合

java8中Collection接口被扩展，提供了两个获取流的方法：

- `default Stream<E> stream():返回一个顺序流`
- `default Stream<E> parallelStream():返回一个并行流`

#### 通过数组

```java
Arrays.stream()
```

#### 通过Stream of

```java
Stream.of()
```

#### 创建无限流

```java
 	//迭代
    //遍历前10个偶数
    Stream.iterate(0, t -> t + 2).limit(10).forEach(System.out::println);

    //生成
    Stream.generate(Math::random).limit(10).forEach(System.out::println);
```

### Stream的中间操作

#### 筛选与切片

```java
 int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1};
        //filter(Predicate p) 接受Lambada 过滤流中不符合条件的元素
        Arrays
            .stream(arr)
            .filter(t -> t % 2 == 0)
            .forEach(System.out::println);
        System.out.println();

        //limit(n) 截断流，使元素不超过给定数量
        Arrays
            .stream(arr)
            .limit(5)
            .forEach(System.out::println);
        System.out.println();

        //skip(n) 跳过元素，返回一个跳过了前n个元素的流。若流中元素不足n个，则返回一个空的流
        Arrays
            .stream(arr)
            .skip(2)
            .forEach(System.out::println);
        System.out.println();

        //distinct() 筛选 通过元素的hashCode()和equals()去除重复元素
        Arrays
            .stream(arr)
            .distinct()
            .forEach(System.out::println);
```

#### 映射

```java
    //映射
    @Test
    public void test2() {
        //map(Function f) 接收一个函数作为参数，将元素转换成其他形式或提取信息，该函数会被应用到每个元素上，并将其映射成一个新的元素
        List<String> strings = Arrays.asList("aa", "bb", "cc", "dd");
        strings
                .stream()
                .map(str -> str.toUpperCase(Locale.ROOT))
                .forEach(System.out::println);

        Stream<Stream<Character>> streamStream = strings.stream().map(StreamAPITest2::fromStreinToStream);
        streamStream.forEach(
                s -> {
                    s.forEach(System.out::println);
                }
        );
        System.out.println();

        //flatMap(Function f) 接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流
        Stream<Character> characterStream = strings.stream().flatMap(StreamAPITest2::fromStreinToStream);
        characterStream.forEach(System.out::println);
    }

    public static Stream<Character> fromStreinToStream(String str) {
        ArrayList<Character> list = new ArrayList<>();
        for (Character c : str.toCharArray()) {
            list.add(c);
        }
        return list.stream();
    }
```

#### 排序

```java
//排序
    @Test
    public void test4() {
        // sorted() 自然排序
        List<Integer> integers = Arrays.asList(1, 6, 15, 99, -36, -60, 50, 66, 2);
        integers.stream().sorted().forEach(System.out::println);

        // sorted(Comparator com) 定制排序
        List<User> users = new LinkedList<>();
        for (int i = 0; i < 10; i++) {
            User user = User
                    .builder()
                    .name("simple" + i)
                    .age((int) (Math.random() * 50  + i))
                    .build();
            users.add(user);
        }

        users
                .stream()
                .sorted((u1, u2) -> u2.getAge() - u1.getAge())
                .forEach(System.out::println);
    }
```



### Stream的终止操作

#### 匹配与查找

```java
 //匹配与查找
    @Test
    public void test1() {
        List<User> users = new LinkedList<>();
        for (int i = 0; i < 10; i++) {
            User user = User
                    .builder()
                    .name("simple" + i)
                    .age((int) (Math.random() * 50  + i))
                    .build();
            users.add(user);
        }

        // allMatch(Predicate p) 检查所有元素是否匹配条件
        boolean allMatch = users
                .stream()
                .allMatch(u -> u.getAge() > 20);
        System.out.println(allMatch);

        // 是否有元素匹配
        boolean anyMatch = users
                .stream()
                .anyMatch(u -> u.getAge() > 20);
        System.out.println(anyMatch);

        //所有都不匹配
        boolean noneMatch = users
                .stream()
                .noneMatch(u -> u.getAge() > 100);
        System.out.println(noneMatch);

        //返回第一个元素
        Optional<User> first = users
                .stream()
                .findFirst();
        System.out.println(first);

        //返回任意一个元素
        Optional<User> any = users
                .stream()
                .findAny();
        System.out.println(any);
    }

    @Test
    public void test2() {
        List<User> users = new LinkedList<>();
        for (int i = 0; i < 10; i++) {
            User user = User
                    .builder()
                    .name("simple" + i)
                    .age((int) (Math.random() * 50  + i))
                    .build();
            users.add(user);
        }

        // 返回流中元素的个数
        long count = users
                .stream()
                .filter(u -> u.getAge() > 20)
                .count();
        System.out.println(count);

        // max(Comparator c) 返回流中最大值
        Optional<Integer> max = users
                .stream()
                .map(User::getAge)
                .max(Integer::compareTo);
        System.out.println(max);

        // min(Comparator c) 返回流中最小值
        Optional<User> min = users
                .stream()
                .min(Comparator.comparingInt(u -> u.getAge()));
        System.out.println(min);

        // forEach(Consumer c) 内部迭代
        users
                .stream()
                .forEach(System.out::println);
    }
```

#### 规约

```java
// 规约
@Test
public void test1() {
    // reduce(T identity, BinaryOperator) 将流中元素反复结合起来，得到一个值。返回Optional<T>
    List<Integer> integerList = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
    Integer reduce = integerList
            .stream()
            .reduce(0, Integer::sum);
    System.out.println(reduce);

    // reduce(BinaryOperator) 将流中元素反复结合起来，得到一个值。返回Optional<T>
    Optional<Integer> reduce1 = integerList
            .stream()
            .reduce(Integer::sum);
    System.out.println(reduce1);
}
```

#### 收集

```java
    // 收集
    @Test
    public void test2() {
        // collect(Collect c) 将流转换为其他形式。接收一个Collector接口实现，用于给Stream中元素做汇总的方法
        List<User> users = new LinkedList<>();
        for (int i = 0; i < 10; i++) {
            User user = User
                    .builder()
                    .name("simple" + i)
                    .age((int) (Math.random() * 50  + i))
                    .build();
            users.add(user);
        }

        List<User> userList = users
                .stream()
                .filter(u -> u.getAge() > 30)
                .collect(Collectors.toList());
        System.out.println(userList);

        Set<User> userSet = users
                .stream()
                .filter(u -> u.getAge() > 30)
                .collect(Collectors.toSet());
        System.out.println(userSet);
    }
```

# [Optional类](https://github.com/simple-jbx/SourceCode/blob/main/JAVA/JDK8Src/java/util/Optional.java)

空指针异常是导致Java应用程序失败的最常见原因。

`Optional<T>`类`java.util.Optional`是一个容器类，可以保存类型T的值，代表这个值存在。或者仅仅保存null，表示这个值不存在。可以有效避免空指针异常。

Optional类的Javadoc描述：这是一个可以为null的容器对象。如果值存在则isPresent()方法会返回true，调用get()方法会返回该对象。

## 方法

### 创建Optional类对象的方法

```java
    public static <T> Optional<T> of(T value) {
        //value必须为非空 详见源码
        return new Optional<>(value);
    }
	
	//value可以为null
    public static <T> Optional<T> of(T value) {
        return new Optional<>(value);
    }

	//创建一个空的Optional实例
    public static<T> Optional<T> empty() {
        @SuppressWarnings("unchecked")
        Optional<T> t = (Optional<T>) EMPTY;
        return t;
    }	
```

### 判断Optional容器中是否包含对象

```java
    //判断对象是否为null
	public boolean isPresent() {
        return value != null;
    }
	
	//如果value！=null 则将value传给consumer，执行Consumer接口方法
    public void ifPresent(Consumer<? super T> consumer) {
        if (value != null)
            consumer.accept(value);
    }
```

### 获取Optional容器的对象

```java
	public T get() {
        if (value == null) {
            throw new NoSuchElementException("No value present");
        }
        return value;
    }

    public T orElse(T other) {
        return value != null ? value : other;
    }

    public T orElseGet(Supplier<? extends T> other) {
        return value != null ? value : other.get();
    }

    public <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) throws X {
        if (value != null) {
            return value;
        } else {
            throw exceptionSupplier.get();
        }
    }
```

# [反射](https://github.com/simple-jbx/Java8Study/tree/main/src/test/java/reflection)

## 简介

动态语言的关键，允许程序在执行期借助Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。

类加载完后，在堆内存的方法区就产生了一个Class类型的对象（一个类只有一个），这个Class对象就包含了完整的类的结构信息。这个Class对象就像一面镜子，透过镜子可以看到类的结构，即反射。

### 提供的功能

- 在运行时判断任意一个对象所属的类
- 在运行时构造任意一个类的对象
- 在运行时判断任意一类所具有的成员变量和方法
- 在运行时获取泛型信息
- 在运行时调用任意一个对象的成员变量和方法
- 在运行时处理注解
- 生成动态代理

### 主要API

- java.lang.Class
- java.lang.reflect.Method：代表类的方法
- java.lang.reflect.Field：代表类的成员变量
- java.lang.reflect.Constructor：代表类的构造器

# 线程与线程池

[JDK1.8 Thread.java](https://github.com/simple-jbx/SourceCode/tree/main/JAVA/JDK8Src/java/lang/Thread.java)

## 线程状态

- New（新创建）

    Thread thread = new Thread(task)

- Runnable（可运行）

    thread.start()

- Blocked（被阻塞）

- Waiting（等待）

- Timed waiting（计时等待）

- Terminated（被终止）

    run()方法正常退出

    没有捕获异常而异常终止

可调用getState方法获取线程当前状态

<div align='center'>
    <img src='./imgs/Java/1.8/ThreadState.jpg' width='800px'>
    </br></br>线程状态
</div>

## 线程同步

### synchronized(内置锁/互斥锁、可重入)

```java
package simple.jbx.java.com.singleton;

import java.lang.reflect.Constructor;

/**
 * @author simple.jbx
 * @description synchronized 实现懒汉式单例
 * @date 16:16 2021/10/15
 * @return
 **/
public class Lazy {
    private Lazy(){
        //System.out.println(Thread.currentThread().getName() + "start");
        synchronized (Lazy.class){
            if(lazy == null){
                throw new RuntimeException("不要试图使用反射破坏异常");
            }
        }
    }

    private volatile static Lazy lazy;

    //双重检测锁模式的懒汉式单例 DCL懒汉式
    public static Lazy getInstance() {
        if(lazy == null)
        {
            synchronized(Lazy.class){
                if(lazy == null)
                {
                    lazy = new Lazy();
                    //不是一个原子操作
                    /**
                     *1. 分配内存空间
                     *2. 执行构造方法，初始化对象
                     *3. 把这个对象指向这个空间
                     *
                     * 123 Thread A
                     * 132
                     *     Thread B //此时还没有完成构造
                     */
                }
            }
        }
        return lazy;
    }

    public static void main(String[] args) throws Exception {
        /*
        for(int i = 0; i < 10; i++)
        {
            new Thread(()->{
                Layz.getInstance();
            }).start();
        }*/

        //Lazy instance = Lazy.getInstance();
        Constructor<Lazy> declearedConstructor = Lazy.class.getDeclaredConstructor(null);
        declearedConstructor.setAccessible(true);

        //java10 这里两个对象用反射还是不能破坏单例  视频里的java8可以
        Lazy instance2 = declearedConstructor.newInstance();
        Lazy instance3 = declearedConstructor.newInstance();

        //System.out.println(instance);
        System.out.println(instance2);
        System.out.println(instance3);
    }
}

```

<div align='center'>
    <img src='./imgs/Java/1.8/synchronized001.png' width='500px'>
    </br></br>synchronized同步
</div>



synchronized同步语句块的实现使用的是monitorenter和monitorexit指令。

synchronized修饰的方法实现使用的是ACC_SYNCHRONIZED表示，该标识指明了该方法是一个同步方法。

两者的本质都是对对象监视器monitor的获取。

### voliate

稍弱的同步机制，用来确保将变量的更新操作通知到其他线程。当把变量声明为volatile类型后，编译器与运行时都会注意到这个变量是共享的，因此不会将该变量上的操作与其他内存操作一起重排序。voliate变量不会被缓存在寄存器或者对其他处理器不可见的地方，因此在读取voliate类型的变量时总会返回最新写入的值。

加锁机制既可以确保可见性又可以确保原子性，而volatile变量只能确保可见性，通常用作某个操作完成、发生中断或者状态的标志。

当且仅当满足一下所有条件时，才应该使用volatile变量：

- 对变量的写入操作不依赖变量的当前值，或者能确保只有单个线程更新变量的值。
- 该变量不会与其他状态变量一起纳入不变性条件中。
- 在访问变量时不需要加锁。

```java
volatile boolean sleep;
while(!asleep) {
	countSomeSheep();
}
```

### wait和notify

### 局部变量（ThreadLocal）



### 原子变量

```java
import java.util.concurrent.atomic
public class HelloServlet implements Servlet {
 	
    private final AtomicLong count = new AtomicLong(0);
    private final AtomicBoolean atomicBoolean = new AtomicBoolean(false);
    private final AtomicInteger atomicInteger = new AtomicInteger();
    private final AtomicIntegerArray atomicIntegerArray = new AtomicIntegerArray(0);
    private final AtomicLongArray atomicLongArray = new AtomicLongArray(0);
	
    public long getCount() {
        return count.get();
    }

    public void increCount() {
        count.incrementAndGet();
    }

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {

    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```



### 阻塞队列

## 线程封闭

避免使用同步的方式就是不共享数据。如果仅在单线程内访问数据，就不需要同步。这种技术被称为线程封闭（Thread Confinement），是实现线程安全性的最简单方式之一。当某个对象封闭在一个线程中时，这种用法将自动实现线程安全性，即使被封闭的对象本身不是线程安全的。

### Ad-hoc 线程封闭

维护线程封闭性的职责完全由程序实现来承担。非常脆弱。因此应当尽量少使用它，应该使用更强的线程封闭技术（栈封闭或ThreadLocal类）。

### 栈封闭

只有局部变量才能访问对象。局部变量的固有属性之一就是封闭在执行线程中。它们位于执行线程的栈中，其他线程无法访问这个栈。栈封闭（也被成为线程内部使用或者线程局部使用，不要与核心类库中的ThreadLocal混淆）比Ad-hoc线程封闭更容易维护，也更加健壮。

### ThreadLocal

更规范的方法。能使线程中的某个值与保存值的对象关联起来。ThreadLocal提供了get与set等访问接口或方法，这些方法为每个使用该变量的线程都存有一份独立的副本，因此get总是返回由当前执行线程在调用set时设置的最新值。

ThreadLocal对象通常用于防止对可变的单实例变量（Singleton）或全局变量进行共享。

当某个频繁执行的操作需要一个临时对象，例如一个缓冲区，而同时又希望避免在每次执行时都重新分配该临时对象，就可以使用这项技术。（JDK1.5之前，Integer.toString()方法使用ThreadLocal对象保存一个12B大小的缓冲区，用于对结果进行格式化，而不是使用共享的静态缓冲区），或每次调用的时候分配一个新的

## 线程创建和使用

### 线程创建方法

- 继承Thread类并重写run()方法
- 实现Runnable接口
- 实现Callable接口
- 线程池

### Thread

```java
/*
  线程让步
  暂停当前正在执行的线程，把执行机会让给优先级相同或者更高的线程
  若队列中没有同优先级的线程，忽略此方法
*/
public static native void yield();

/*
  使当前活动线程在指定时间内放弃对CPU控制，时间到后重新排队
  有可能会抛出异常
*/
public static native void sleep(long millis) throws InterruptedException;
public static native void sleep(long millis, int nanos) throws InterruptedException;

public synchronized void start();

/*
  当某个程序执行流中调用其他线程的join()方法时，调用线程将被阻塞，直到join()方法加入的线程执行完为止
*/
public final synchronized void join(long millis)
    throws InterruptedException
  
/*
  判断线程是否还存活
*/    
public final native boolean isAlive();    
```

### 实现 Runnable 接口和 Callable 接口的区别

**`Runnable` 接口** 不会返回结果或抛出检查异常，但是 **`Callable` 接口** 可以

1. **`execute()`方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；**
2. **`submit()`方法用于提交需要返回值的任务。线程池会返回一个 `Future` 类型的对象，通过这个 `Future` 对象可以判断任务是否执行成功**，并且可以通过 `Future` 的 `get()`方法来获取返回值，`get()`方法会阻塞当前线程直到任务完成，而使用 `get(long timeout，TimeUnit unit)`方法则会阻塞当前线程一段时间后立即返回，这时候有可能任务没有执行完。

## 线程池

Exectuors:工具类、线程池的工厂类，用于创建并返回不同类型的线程池。

```java
//1.提供指定数量的线程池
ExecutorService executorService = Executors.newFixedThreadPool(10);

//2.执行指定的线程操作。需提供实现Runnable或Callable接口实现类的对象
executorService.execute(new NumberThread());//适合使用于Runnable
executorService.submit();//适合使用于Callable
//3.关闭线程池
executorService.shutdown();

//创建线程池通用构造函数
 /**
     * Creates a new {@code ThreadPoolExecutor} with the given initial
     * parameters.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @param maximumPoolSize the maximum number of threads to allow in the
     *        pool
     * @param keepAliveTime when the number of threads is greater than
     *        the core, this is the maximum time that excess idle threads
     *        will wait for new tasks before terminating.
     * @param unit the time unit for the {@code keepAliveTime} argument
     * @param workQueue the queue to use for holding tasks before they are
     *        executed.  This queue will hold only the {@code Runnable}
     *        tasks submitted by the {@code execute} method.
     * @param threadFactory the factory to use when the executor
     *        creates a new thread
     * @param handler the handler to use when execution is blocked
     *        because the thread bounds and queue capacities are reached
     * @throws IllegalArgumentException if one of the following holds:<br>
     *         {@code corePoolSize < 0}<br>
     *         {@code keepAliveTime < 0}<br>
     *         {@code maximumPoolSize <= 0}<br>
     *         {@code maximumPoolSize < corePoolSize}
     * @throws NullPointerException if {@code workQueue}
     *         or {@code threadFactory} or {@code handler} is null
     */

/*
corePoolSize 核心线程数 线程池线程基本数量（即使没有任务，线程池所有线程是空闲状态，线程数也保持这个数）
maximumPoolSize 线程池最大线程数
keepAliveTime 当线程数大于corePoolSize时多余的线程处于空闲状态时最大存活时间
unit keepAliveTime的单位
workQueue 存放已提交但未执行的任务的工作队列
threadFactory 使用哪种工厂模式创建线程池
handler 饱和策略
*/
public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler) {···}
```

优点：

- 提高响应速度（减少了创建新线程的时间）
- 降低资源消耗（重复利用线程池中的线程，不需要每次都创建）
- 便于线程管理
    - corePoolSize：核心池大小
    - maximumPoolSize：最大线程数
    - keepAliveTime：线程没有任务时最多保持多长时间后会终止

### 创建线程池

使用Executor静态工厂

| 方法                             | 描述                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| newCachedThreadPool              | 返回一个带缓存的线程池，必要时创建新线程；空闲线程会被保留60秒 |
| newFixedThreadPool               | 包含固定数量的线程；空闲线程会一直被保留；线程数由参数决定   |
| newSingleThreadExecutor          | 只有一个线程的“池”，该线程顺序执行每一个提交的任务           |
| newScheduledThreadPool           | 用于预定执行而构建的固定线程池，替代java.util.Timer          |
| newSingleThreadScheduledExecutor | 用于预定执行而构建的单线程“池”                               |

### 线程池常用方法

- int getLargestPoolSize() 返回线程池在该执行器生命周期中的最大尺寸

### 生命周期

ExecutorService的声明周期有三种状态：运行、关闭、已停止。

初始创建时处于运行状态。shutdown方法将执行平缓的关闭过程：不在接受新的任务，同时等待已经提交的任务执行完成--包括那些还未开始执行的任务。shutdownNow方法将执行粗暴的关闭过程：它将尝试取消所有运行中的任务，并且不在启动队列中尚未开始执行的任务。

在ExecutorService关闭后提交的任务将由“拒绝执行处理器（Rejected Execution Handle）来处理”，它会抛弃任务，或者使得execute方法抛出一个未检查的Rejected-ExecutionException。等所有任务完成后，ExecutorService到达终止状态。可以调用awaitTermination来等待ExecutorService到达终止状态，或者通过调用isTerminated来轮询ExecutorService是否已经终止。通常在调用awaitTermination之后会立即调用shutdown，从而产生同步地关闭ExecutorService的效果。

### 管理线程池队列任务

ThreadPoolExcutor允许提供一个BlockingQueue（阻塞队列）来保存等待执行的任务。基本的任务排队方法有3种：无解队列、有届队列和同步移交（Synchronous Handoff）。队列的选择与其他配置参数有关，例如线程池大小等。

newFixedThreadPool和newSingleThreadExecutor在默认情况会使用一个无界的LinkedBlockingQueue。如果任务到达速度大于处理速度，队列将无限制增加。

比较稳妥的策略是使用有界队列，例如ArrayBlockingQueue、有界的LinkedBlockingQueue、PriorityBlockingQueue。会引发新的问题，队列满了怎么办，需要用到下面的各种饱和策略。在使用有界队列的时候，队列的大小与线程池的大小必须一起调节。如果线程池较小而队列较大，那么有助于减少内存使用量，降低CPU使用率，还能减少上下文切换，但可能会限制吞吐量。

对于非常大的或者无界的线程池，可以通过使用SynchronousQueue来避免任务排队，以及直接将任务从生产者转移交给工作线程。SynchronousQueue不是真正意义上的队列，而是一种移交机制。要将一个任务放到SynchronousQueue中，必须有另一个空闲线程在等待接收任务。如果没有，并且线程池当前大小小于最大值，则创建一个新的线程，否则根据饱和策略，这个任务会被拒绝。只有当线程池是无界的或者可以拒绝任务的时候，SynchronousQueue才有实际价值。newCachedTHreadPool工厂方法中就使用了SynchronousQueue。

### 饱和策略

#### AbortPolicy

Abort（中止）策略是默认的饱和策略，该策略将抛出未检查的RejectedExecutionException。调用者可以捕获这个异常，并根据需求编写处理代码。

#### CallerRunsPolicy

调用者运行策略实现了一种调节机制，该策略既不会抛弃任务，也不会抛出异常，而是将某些任务回退到调用者，从而降低新任务的流量。它不会在线程池中的某个线程执行新提交的任务，而是在一个调用了execute的主线程中执行该任务。由于执行该任务需要一定时间，因此在这段时间内主线程不会调用accept，从而到达的请求被保存在TCP的缓存队列而不是在应用程序。

#### DiscardPolicy

当新提交的任务无法保存到队列等待执行时，抛弃（Discard）策略会抛弃该任务。

#### DiscardOldestPolicy

抛弃最旧的策略会抛弃下一个将被执行的任务，然后尝试重新提交新的任务。如果工作队列是一个优先队列，则该策略会抛弃优先级最高的任务，因此最好不要将该策略与优先级队列放在一起使用。

# 多线程



# 生产力

## Java命令行调试

### 查看字节码

```java
javap -c xxx.class
```

## IDEA

### 快捷键

### 插件

#### Alibaba Java Coding Guidelines

Alibaba 编码规范

#### LeetCode Editor

LeetCode刷题

#### Rainbow Brackets

彩虹括号

#### Background Image Plus

更改IDEA背景

#### Maven

#### Lombok

注解 web开发

#### Bytecode Editor

查看字节码

选中要查看的.class文件然后点上方菜单view->Show ByteCode

#### AiXcoder Code Completer

 
