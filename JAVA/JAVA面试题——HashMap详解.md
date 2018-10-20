HashMap详解
===

1. 默认初始容量：一定要是2的倍数，默认是16。
```java
	static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
```
2. 最大容量：2的30次方。
```java
	/**
     * The maximum capacity, used if a higher value is implicitly specified
     * by either of the constructors with arguments.
     * MUST be a power of two <= 1<<30.
     */
    static final int MAXIMUM_CAPACITY = 1 << 30;
```
3. 负载因子：默认是0.75。
```java
	/**
     * The load factor used when none specified in constructor.
     */
    static final float DEFAULT_LOAD_FACTOR = 0.75f;
```
4. 树形化阈值：hashmap的存储结构由数组+链表改变成数组+红黑树的桶数界限。该值必须大于2并应该至少是8。
```java
    static final int TREEIFY_THRESHOLD = 8;
```
5. 非树形化阈值：在重构期间，桶数不大于这个值时，hashmap的存储结构仍是数组+链表。应小于树形化阈值（TREEIFY_THRESHOLD ）。
```java
    static final int UNTREEIFY_THRESHOLD = 6;
```
6.  可被树形化的最小表容量：至少为树形化阈值的四倍。
```java
    static final int MIN_TREEIFY_CAPACITY = 64;
```
7. 哈希函数：用以定位数组索引位置。
	- key为null的都在表头。
	- key不为null的： 取hashCode()的高16位 异或 低16位 所得的值作为索引位置。图解如下：
![hash()图解](images/hash()%E5%9B%BE%E8%A7%A3.PNG)
```java
static final int hash(Object key) {
        int h;
		//^为异或符号（异或：如果a、b两个值不相同，则异或结果为1。如果a、b两个值相同，异或结果为0。）
		//>>>：无符号右移,高位补0
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```
8. 数组（表）：
```java
transient Node<K,V>[] table;
```
9. 链表（桶）：
```java
transient Set<Map.Entry<K,V>> entrySet;
```
10. 键值对数量：
```java
transient int size;
```