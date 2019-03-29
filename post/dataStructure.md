O(1) < O(logn) < O(n) < O(nlogn) < O(n<sup>2</sup>) < O(n<sup>3</sup>) < O(2<sup>n</sup>) < O(n!) < O(n<sup>n</sup>)



### Singly Linked List

- A linked list is a linear data structure, in which the elements are not 
  stored at contiguous memory locations. The elements in a linked list are
  linked using pointers

- In simple words, a linked list consists of nodes where each node 
  contains a data field and a reference(link) to the next node in the 
  list.

  <img src="https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2013/03/Linkedlist.png" />

- Advantages over arrays
  - Dynamic size
  - Ease of insertion/deletion

- Drawbacks:
  - Random access is not allowed.  We have to access elements sequentially starting from the first node.  So we cannot do binary search with linked lists efficiently with its default implementation.
  - Extra memory space for a pointer is required with each element of the list.
  - Not cache friendly. Since array elements are contiguous locations, there is locality of reference which is not there in case of linked lists.

- Representation: A linked list is represented by a pointer to the first node of the 
  linked list.  The first node is called head.  If the linked list is 
  empty, then value of head is NULL.
  Each node in a list consists of at least two parts:

  - data
  - Pointer (Or Reference) to the next node

- Inserting a Node

  - At the front of the linked list

    <img src="https://www.geeksforgeeks.org/wp-content/uploads/gq/2013/03/Linkedlist_insert_at_start.png" />

  - After a given node

    <img src="https://www.geeksforgeeks.org/wp-content/uploads/gq/2013/03/Linkedlist_insert_middle.png" />

  - At the end of the linked list

    <img src="https://www.geeksforgeeks.org/wp-content/uploads/gq/2013/03/Linkedlist_insert_last.png">

- Deleting a given key

  <img src="https://www.geeksforgeeks.org/wp-content/uploads/gq/2014/05/Linkedlist_deletion.png">



### Circular Linked List

- Circular linked list is a linked list where all nodes are connected to form a circle. There is no NULL at the end. A circular linked list can be a singly circular linked list or doubly circular linked list.

  <img src="https://cdncontribute.geeksforgeeks.org/wp-content/uploads/CircularLinkeList.png">

- Advantages of Circular Linked Lists

  - Any node can be a starting point. We can traverse the whole list by starting from any point. We just need to stop when the first visited node is visited again.
  - Useful for implementation of queue. Unlike this implementation, we don’t need to maintain two pointers for front and rear if we use circular linked list. We can maintain a pointer to the last inserted node and front can always be obtained as next of last.
  - Circular lists are useful in applications to repeatedly go around the list. For example, when multiple applications are running on a PC, it is common for the operating system to put the running applications on a list and then to cycle through them, giving each of them a slice of time to execute, and then making them wait while the CPU is given to another application. It is convenient for the operating system to use a circular list so that when it reaches the end of the list it can cycle around to the front of the list. 













双向链表：结点都有两个指针域，一个指向直接后继，一个指向直接前驱



线性表

- 顺序存储结构
- 链式存储结构
  - 单链表
  - 静态链表
  - 循环链表
  - 双向链表



**栈**（stack）：限定仅在表尾进行插入和删除操作的线性表

栈顶（top）：允许插入和删除的一端

栈底（bottom）

后进先出（Last In First Out）

栈的插入操作/进栈/压栈/入栈

栈的删除操作/出栈/弹栈



栈的链式存储结构（链栈）



| 栈     | 时间复杂度 |                                |
| ------ | ---------- | ------------------------------ |
| 顺序栈 | O(1)       | 预先确定长度，读取方便         |
| 链栈   | O(1)       | 每个元素都有指针域，长度无限制 |



递归函数：一个直接调用自己或通过一系列的调用语句间接地调用自己的函数

每个递归定义必须至少有一个条件，满足时递归不再进行，既不再引用自身而是返回值退出



逆波兰：一种不需要括号的后缀表达法，从左到右遍历表达式的每个数字和符号，遇到数字就进栈，遇到符号就将栈顶两个数字出栈，进行运算，运算结果进栈，直到获得结果



中缀表达式转后缀表达式：



**队列**

队列（queue）：只允许在一端进行插入操作（队尾），另一端进行删除操作（队头）的线性表

先进先出（First In First Out）



串（string）：由零个或多个字符组成的有限序列，字符串

串的模式匹配：子串的定位操作



KMP模式匹配算法：



**树**

Tree：是n个(n>=0)个结点的有限集。n=0时为空树。任意一棵非空树种：(1)有且仅有一个特定的跟结点(Root);(2)当n>1时，其余结点可分为m(m>0)个互不相交的有限集，其中每一个集合本身又是一棵树，并且成为根的子树(SubTree)



结点的度(Degree)：结点拥有的子树数

叶(Leaf)/终端点：Degree=0的结点

非终端点/分支结点：Degree!=0的结点，除了根结点，分支结点也叫内部结点

树的度：树内各结点的度的最大值



结点之间的关系：Child, Parent, Sibling

结点的层次(Level)：从根开始定义起，根为第一层

树的深度(Depth)/高度：树中结点的最大层次

有序树：将树中结点的各子树看成从左至右是有次序的，不能互换



特殊二叉树：斜树、满二叉树、完全二叉树



二叉树遍历：

- 前序遍历：访问根结点，先遍历左子树，再遍历右子树
- 中序遍历
- 后序遍历
- 层序遍历

已知前序遍序列和中序遍历序列，可以唯一确定一棵二叉树

已知后序遍序列和中序遍历序列，可以唯一确定一棵二叉树



### sort

**稳定**：如果a原本在b前面，而a=b，排序之后a仍然在b的前面； 

**不稳定**：如果a原本在b的前面，而a=b，排序之后a可能会出现在b的后面；

**内排序**：所有排序操作都在内存中完成； 

**外排序**：由于数据太大，因此把数据放在磁盘中，而排序通过磁盘和内存的数据传输才能进行；

**时间复杂度**: 一个算法执行所耗费的时间。 

**空间复杂度**: 运行完一个程序所需内存的大小。

  

<img width="88%" src="https://user-gold-cdn.xitu.io/2016/11/29/4abde1748817d7f35f2bf8b6a058aa40?imageView2/0/w/1280/h/960/ignore-error/1" /> 

 n: 数据规模 k:“桶”的个数

In-place: 占用常数内存，不占用额外内存

Out-place: 占用额外内存



冒泡排序

<img width="70%" src="https://user-gold-cdn.xitu.io/2016/11/30/f427727489dff5fcb0debdd69b478ecf?imageView2/0/w/1280/h/960/ignore-error/1" />



选择排序

<img width="70%" src="https://user-gold-cdn.xitu.io/2016/11/29/138a44298f3693e3fdd1722235e72f0f?imageView2/0/w/1280/h/960/ignore-error/1" />



插入排序

<img width="70%" src="https://user-gold-cdn.xitu.io/2016/11/29/f0e1e3b7f95c3888ab2791b6abbfae41?imageView2/0/w/1280/h/960/ignore-error/1" />



归并排序

<img width="70%" src="https://user-gold-cdn.xitu.io/2016/11/29/33d105e7e7e9c60221c445f5684ccfb6?imageView2/0/w/1280/h/960/ignore-error/1" />

