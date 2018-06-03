JAVA面试题(四)——集合

===

1. Collection接口：定义了存取一组对象的方法。
   - Set（->集合）中的数据对象没有顺序且不可以重复
   - List中的数据对象有顺序且可以重复（equals)
     - ArrayList
     - Vector
     - LinkedList
     - LinkedList和ArrayList的区别：
       1. LinkedList经常用在增删操作较多而查询操作很少的情况下，ArrayList则相反。
       2. ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构。
     - Vector和ArrayList的区别
       1. vector是线程同步的，所以它也是线程安全的，而arraylist是线程异步的，是不安全的。如果不考虑到线程的安全因素，一般用arraylist效率比较高。
       2. 如果集合中的元素的数目大于目前集合数组的长度时，vector增长率为目前数组长度的100%，而arraylist增长率为目前数组长度的50%。如果在集合中使用**数据量比较大**的数据，用**vector**有一定的优势。
   - Queue保持一个队列(先进先出)的顺序
2. Map接口：定义了存储“键（key,名字）-值（value）映射对”的方法。
   - HashMap
   - HashTable
     - LinkedHashMap
   - SortedMap
     - TreeMap
   - WeakHashMap
   - IdentityHashMap
   - EnumMap
   - ConcurrentHashMap：线程安全、锁分离。内部使用段来表示这些不同的部分，每个段是小小的HashTable,有自己的锁。只要多个修改发生在不同的段上，就可以并发进行。
   - HashMap和HashTable的区别
     1. HashMap是线程不安全的，HashTable是线程安全的。
     2. HashMap允许一个为null的key，多个为null的value。HashTable的key和value都不允许为null。
3. conllections和conllection区别
   - conllection是接口，是集合框架的顶级接口。下面有子接口以及实现类。
   - conllections是工具类，是集合框架的工具类，对集合进行操作。里面有很多静态方法。