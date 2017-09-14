# Collection

Collection是集合类的一个顶级接口，其直接继承接口有 List 与 Set 。声明了适用于JAVA集合（只包括 Set 和 List ）的通用方法。

继承关系：

![collection](../imgs/collection-diagram.png)

上述所有的集合类，都实现了Iterator接口。

- Iterator 接口包含以下以下方法: 
    1. hasNext()    // 是否还有下一个元素。
    2. next()   // 返回下一个元素。
    3. remove() 删除当前元素。


- 集合类: 

名称 |  | 是否有序 | 是否允许元素重复  
---|---|---|---  
Collection |  | 无序 | 允许元素重复  
List |  | 有序 | 允许元素重复  
Set | AbstractSet | 无序 | 不允许元素重复  
set | HashSet | 无序 | 不允许元素重复  
Set | TreeSet | 有序 | 不允许元素重复  
Map | AbstractMap | 无序 |使用 key-value 来映射和存储数据，key 必须唯一，value 可以重复
Map | HashMap | 无序 |  
Map | TreeMap | 有序 |  

- 输出方式  

1. Iterator: 迭代输出。  
2. foreach: JDK1.5 之后提供的新功能，可以输出数组或集合。
3. for 循环。  
4. ListIterator: 是 Iterator 的子接口, 专门用于输出 List 中的内容。

## Set

Set 是最简单的一种集合。集合中的对象不按特定的方式排序，并且没有重复对象。主要有两个实现类：

    1. HashSet
    2. TreeSet

- 功能： 

存入Set的每个元素都必须是唯一的.

    1. HashSet：为快速查找设计的 Set 。存入 HashSet 的对象必须定义 hashCode()。 
    2. TreeSet：有顺序的 Set，底层为树结构。可以从 Set 中提取有序的序列
    3. LinkedHashSet：具有 HashSet 的查询速度，且内部使用链表维护元素的顺序(插入的次序)。于是在使用迭代器遍历 Set 时，结果会按元素插入的次序显示。

## List

List的特征是其元素以线性方式存储，集合中可以存放重复对象。  

List 接口的实现类有：  

    1. ArrayList
    2. LinkedList
    3. Vector

- List 的功能方法

    1. ArrayList：由数组实现的 List。允许对元素进行快速随机访问，但是向List中间插入与移除元素的速度很慢。ListIterator 只应该用来由后向前遍历 ArrayList ,而不是用来插入和移除元素。因为那比 LinkedList 开销要大很多。 
    2. LinkedList：对顺序访问进行了优化，向 List 中间插入与删除的开销并不大。随机访问则相对较慢。具有 addFirst(), addLast(), getFirst(), getLast(), removeFirst() 和 removeLast() 等方法，使得LinkedList可以当作堆栈、队列和双向队列使用。

## Map 和 Collection

Map 是一种把键对象和值对象映射的集合, 每一个元素都包含一对键对象和值对象。Map 没有继承于 Collection 接口 从 Map 集合中检索元素时，只要给出键对象，就会返回对应的值对象。 

Map 接口的实现类有：
    1. HashTable
    2. HashMap
    3. WeakHashMap
    4. Tip












