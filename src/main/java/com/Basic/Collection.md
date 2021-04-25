##Java Collection
###1.ArrayList(基于动态数组实现，支持随机访问，线程不安全)
原型：
```java
public class ArrayList<E> extends AbstractList<E> implements List<E>, RandomAccess, Cloneable, Serializable{
    ...
    private static final int DEFAULT_CAPACITY = 10;//默认容器长度
    private static final Object[] EMPTY_ELEMENTDATA = new Object[0];//空数组异常处理节约空间
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = new Object[0];//默认构造
    transient Object[] elementData;//存放数据
    private int size;//长度
    ...
}
```
构造方法：
```text
ArrayList()：构造一个初始容量为 10 的空列表
ArrayList(initialCapacity)：构造一个初始容量为 initialCapacity 的空列表
ArrayList(Collection<?extends E>c)：构造一个包含指定 Collection 元素的列表，这些元素是按照该 Collection 的迭代器返回它们的顺序排列的
```
扩容：
ensureCapacityInternal() 方法来检查容量足够，调用 grow() 实现 size+1，newCapacity = oldCapacity + (oldCapacity >> 1)，copyOf原数组
因此最好在创建 ArrayList 对象时就指定大概的容量大小，减少扩容操作的次数。

删除：
调用 System.arraycopy() 将 index+1 后面的元素都复制到 index 位置上，该操作的时间复杂度为 O(N)， ArrayList 删除元素的代价是非常高的。

序列化：
transient防止自动序列化
writeObject() 和 readObject() 来控制只序列化数组中有元素填充那部分内容。


###2.HashMap(基于哈希表实现，线程不安全)
原型：
```java
public class HashMap<K, V> extends AbstractMap<K, V> implements Map<K, V>, Cloneable, Serializable
{
    ...
    static final int DEFAULT_INITIAL_CAPACITY = 16;//初始容量
    static final int MAXIMUM_CAPACITY = 1073741824;//最大容量
    static final float DEFAULT_LOAD_FACTOR = 0.75F;//负载因子
    static final int TREEIFY_THRESHOLD = 8;//链表树化阙值
    static final int UNTREEIFY_THRESHOLD = 6;//红黑树链表化阙值
    static final int MIN_TREEIFY_CAPACITY = 64;//最小树化阙值
    transient HashMap.Node<K, V>[] table;//存放键值对
    transient Set<Entry<K, V>> entrySet;
    transient int size;
    transient int modCount;
    int threshold;
    final float loadFactor;
    ...
}
```
hash：
```java
static final int hash(Object key) {
        int h;
        return key == null ? 0 : (h = key.hashCode()) ^ h >>> 16;
    }
    int index = (table.size - 1) & hash;
```
右移16位:让高位也参与运算  ^:更加散列减少碰撞次数

put：
```text
1.判断键值对数组table[i]是否为空或为null，否则执行resize()进行扩容；
2.根据键值key计算hash值得到插入的数组索引i，如果table[i]==null，直接新建节点添加，转向⑥，如果table[i]不为空，转向③；
3.判断table[i]的首个元素是否和key一样，如果相同直接覆盖value，否则转向④，这里的相同指的是hashCode以及equals；
4.判断table[i] 是否为treeNode，即table[i] 是否是红黑树，如果是红黑树，则直接在树中插入键值对，否则转向⑤；
5.遍历table[i]，判断链表长度是否大于8，大于8的话把链表转换为红黑树，在红黑树中执行插入操作，否则进行链表的插入操作；遍历过程中若发现key已经存在直接覆盖value即可；
6.插入成功后，判断实际存在的键值对数量size是否超多了最大容量threshold，如果超过，进行扩容。
```

resize：
```java
//链表重构检验
e.hash & oldCap
```
构建新数组，然后转移旧数组元素的方式来实现

