# HashMap
基于键的HashCode值唯一标识一条数据
基于键的HashCode值进行数据的存取
其每次遍历的顺序无法保证相同
key和value允许为null
非线程安全
可以用Collections的synchronizedMap方法使HashMap具有线程安全的能力，或者使用ConcurrentHashMap


其内部是一个数组，数组中的每个元素都是一个单向链表
![[IMG-Map-2025_02_20_23_01_50.png]]
查找数据时,根据Hash值快速定位数组下标,但是之后需要对链表进行顺序遍历直到找到需要的数据
时间复杂度为O(n)
Java8之后,将数据结构修改为数组+链表或红黑树。
在链表中的元素超过8个以后，HashMap会将链表结构转换为红黑树结构以提高查询效率，因此其时间复杂度为O(log N)
![[IMG-Map-2025_02_20_23_05_97.png]]

