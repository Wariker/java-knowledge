# 集合

## 集合和数组的区别：

> 集合和数组
> 
> 1.集合的长度可变，数组的长度固定
> 
> 2.数组可以是引用类型，也可以是基本类型，集合只能是引用类型
> 
> 3.数组只可以存储一种类型，集合可以存储不同类型

## Collection集合方法

```java
boolean add(E e);    在集合末尾添加元素
boolean remove(Object o);    若本集合中有与o值相同的元素，则删除该元素
void clear();    清空集合中所有的元素
boolean contains(Object o);    判断集合中是否包含某元素
boolean isEmpty();    判断集合是否为空
int size();    返回集合中的元素个数
boolean addAll(Collection c);    将一个集合中的所有元素添加到当前集合
Object[] toArray();    返回一个包含该集合中所有元素的数组
Iterator iterator();    迭代器，集合的遍历方式
```

## Collection接口的接口实现类

> List：元素按进入先后有序保存，可以重复
> 
>     -LinkedList：链表，没有同步，线程不安全
> 
>     -ArrayList：数组，没有同步，线程不安全
> 
>     -Vector：数组，同步线程安全
> 
>         -Stack：是Vector类的实现类
> 
> Set：该类集合的元素不可重复，并且做内部排序
> 
>     -HashSet：使用hash表（数组）存储元素
> 
>         -LinkedHashSet：链表维护元素的插入次序
> 
>     -TreeSet：底层实现为二叉树，元素排好序

## Map（没有实现Collection接口）

> Map：键值对集合
> 
>     -HashTable：同步，线程安全
> 
>     -HashMap：没有同步，线程不安全
> 
>         -LinkedHashMap：双向链表和哈希表实现
> 
>         -WeakHashMap
> 
>     -TreeMap：红黑树对所有的key进行排序
> 
>     -IdentifyHashMap



## List和Set集合

### List和Set的区别

> 1.List保证元素按照插入顺序排序，而Set存储和取出的顺序不一定一致
> 
> 2.List可以存储相同值的元素，而Set的元素唯一
> 
> 3.List可以直接通过索引操作元素，而Set不能根据索引获取元素



### List集合

> 整体特点比较：底层数据结构，查询效率，增删效率，线程安全问题
> 
> 1.ArrayList：底层数据结构是数组，查询较快，增删较慢，整体效率较高，
> 
>                         线程不安全，可以存储重复元素
> 
> 2.LinkedList：底层数据结构是链表，查询较慢，增删较快，整体效率较高，
> 
>                         线程不安全，可以存储重复元素
> 
> 3.Vector：底层数据结构是数组，查询较快，增删较慢，整体效率较低，
> 
>                     线程安全，可以存储重复元素



### Set集合

> 1.HashSet： 



### 一种适用场景：

> 如果元素可以重复----->List，不可以重复则使用----->Set
> 
>     List：如果需要线程安全，则使用Vector，不然一般使用ArrayList或LinkedList
> 
>                 如果增删较多使用LinkedList，如果查询较多使用ArrayList
> 
>     Set：如果需要保证插入顺序遍历则使用LinkedHashSet，不然如果需要
> 
>                 将元素排序则使用TreeSet，否则可以使用HashSet



## Map主要方法

```java
boolean containsKey(Object key) 查询Map中是否包含指定key
boolean containsValue(Object value)    查询Map中是否包含指定Value
Set entrySet()    返回Map中所包含的键值对所组成的Set集合
Object get(Object key)    返回指定key所对应的value
boolean isEmpty()
Set keySet()    返回该Map中所有key所组成的set集合
Object put(Object key,Object value)    添加一个键值对，覆盖旧值
void putAll(Map m)    将指定Map的键值对复制到当前Map中
Object remove(Object key)    删除指定key所对应的键值对，返回关联的value
int size() 返回当前Map的键值对个数
Collection values()    返回该Map里所有value组成的collection
```



## HashMap与HashTable的比较

> 从线程安全，性能，是否支持null作为键值
> 
> HashMap是异步的，并不线程安全
> 
> HashTable一些方法使用synchronized修饰，所以是线程安全的
> 
> HashMap的性能是最好的，HashTable的性能是最差的
> 
> HashMap可以支持一个null作为key，但是支持不限量的null作为value
> 
> 两者用作key的对象都必须实现hashCode()和equals()方法
> 
> 尽量不要使用可变对象作为key值
> 
> 两者不能保证键值对元素的存储顺序
