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



### Doubly Linked List

- A Doubly Linked List (DLL) contains an extra pointer, typically called previous pointer, together with next pointer and data which are there in singly linked list.

  <img src="https://www.geeksforgeeks.org/wp-content/uploads/gq/2014/03/DLL1.png">

- Advantages over singly linked list

  - A DLL can be traversed in both forward and backward direction.
  - The delete operation in DLL is more efficient if pointer to the node to be deleted is given.
  - We can quickly insert a new node before a given node.
    In singly linked list, to delete a node, pointer to the previous node is needed. To get this previous node, sometimes the list is traversed. In DLL, we can get the previous node using previous pointer.

- Disadvantages over singly linked list

  - Every node of DLL Require extra space for an previous pointer.
  - All operations require an extra pointer previous to be maintained. 

- Insertion

  - At the front of the DLL

    <img src="https://www.geeksforgeeks.org/wp-content/uploads/gq/2014/03/DLL_add_front1.png">

  - After a given node

    <img src="https://www.geeksforgeeks.org/wp-content/uploads/gq/2014/03/DLL_add_middle1.png">

  - At the end of the DLL

    <img src="https://www.geeksforgeeks.org/wp-content/uploads/gq/2014/03/DLL_add_end1.png">

  - Before a given node

    <img src="https://cdncontribute.geeksforgeeks.org/wp-content/uploads/5-55-300x100.png">



### Stack

- Stack is a linear data structure which follows a particular order in 
  which the operations are performed. The order may be LIFO(Last In First 
  Out) or FILO(First In Last Out).

  <img src="https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2013/03/stack.png" />

- Mainly the following three basic operations are performed in the stack:

  - **Push:**  Adds an item in the stack. If the stack is full, then it is said to be an Overflow condition.
  - **Pop:** Removes an item from the stack. The items are  popped in the reversed order in which they are pushed. If the stack is  empty, then it is said to be an Underflow condition.
  - **Peek or Top:** Returns top element of stack. 
  - **isEmpty:**  Returns true if stack is empty, else false.

 

### Queue				

- A Queue is a linear structure which follows a particular order in which 
  the operations are performed. The order is First In First Out (FIFO).

  <img src="https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2014/02/Queue.png">

- Mainly the following four basic operations are performed on queue:

  - Enqueue: Adds an item to the queue. If the queue is full, then it is said to be an Overflow condition.
  - Dequeue: Removes an item from the queue. The items are popped in the same order in which they are pushed. If the queue is empty, then it is said to be an Underflow condition.
  - Front: Get the front item from queue.
  - Rear: Get the last item from queue. 



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

