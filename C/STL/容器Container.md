### 主要容器分类

C++标准库提供了丰富的容器,主要容器有:

1. 序列式容器:按线性顺序存储元素

- vector:动态数组,可随机访问元素。
- deque:双端队列,效率比 vector 好,但无法随机访问。
- list:双向链表,适用于频繁的插入和删除。
- array:普通数组,固定容量。

2. 关联式容器:通过键来访问元素

- set/multiset:各元素唯一/不唯一,按键值排序。
- map/multimap:键值对应值,键唯一/不唯一,按键值排序。

3. 容器适配器:提供额外功能的容器

- stack:运算栈,实现后进先出。
- queue:队列,先进先出。
- priority_queue:优先级队列。

这些容器分为序列式、关联式以及容器适配器。

序列式容器存储元素顺序排列,元素直接按索引访问;

关联式容器通过键来访问元素,元素之间有排序关系;

容器适配器可以把其他容器转换为栈、队列等形式,提供额外功能。

除此之外,还有一些标准容器:

- bitset:位集合,高效操作并存储一个位的集合。
- forward_list:单链表,实现了 list 的子集。
- unordered_set/unordered_map:无序集合,依靠哈希表实现。

<br>

### vector

<br>

#### vector 通用特性

vector 是 STL 中最常用的容器之一,与 C 语言中的数组非常相似,但提供更多功能。

主要特点:

- 动态数组:可以在运行时自由增长,不需要预先指定长度。
- 随机访问:支持通过[]运算符快速访问任意元素。
- 自动扩容:当元素超出容量时,vector 会自动重新分配内存,大小一般翻倍。
- 顺序存储:元素依次连续存储在内存中。

常用 API:

- push_back():在末尾插入一个元素。
- pop_back():删除末尾的元素。
- size():返回容器中元素的个数。
- capacity():返回当前分配的存储容量。
- max_size():返回允许的最大容量。
- empty():判断容器是否为空。
- array accessor:[] 用于索引访问。
- begin()/end():返回迭代器。
- insert():在任意位置插入元素。
- erase():删除任意位置的元素。
- resize():调整大小,可增加或删除元素。

优缺点:

- 优点:随机访问效率高;自动扩容,使用方便。
- 缺点:扩容和删除效率低,平均复杂度 O(n);占用额外内存。

初始化:

- 直接初始化:

```cpp
vector<int> v1;
vector<int> v2(10); // 长度为10,元素默认初始值为0
```

- 列表初始化:

```cpp
vector<int> v3 = {1,2,3};
vector<int> v4(10, 8); //长度为10,元素默认初始值为8
```

<br>

#### vector 实现原理

1.  vector 的三个关键成员:

- data:指向元素的首地址。
- first:指向第一个元素。
- last:指向最后一个元素的下一个位置。

其中 first 和 last 用于判断可用元素范围。

2.  分配内存空间:

- vector 初始化时会分配一个指定容量的内存空间。
- 当元素数量超过已有容量时,需重新分配两倍容量的内存空间。

3.  重新分配内存:

- 检查 capacity。当 size() > capacity() 时需要重新分配。
- 分配两倍 capacity 大小的新内存。
- 将原有元素拷贝到新内存。
- 释放原有内存。
- 更新 first、last、data 指针。

4. 增删元素:

- 插入:需要移动后续元素,时间复杂度 O(n)
- 删除:需要移动后续元素填补 deleted 位置,时间复杂度 O(n)

5. 随机访问:

利用 data 指针和下标 [] 实现。

6. 迭代器:

- begin():返回指向 first 的迭代器。

- end():返回指向 last 的迭代器。

迭代器实际上是一个左闭右开区间，即 `[begin,end)` ，所以 end 是不指向一个有效的元素的

7. 释放内存

- vector 内存的回收只能靠 vector 调用析构函数的时候才被系统收回
- 也可以使用 swap 释放内存，如`v1.swap(v1)`

<br>

### stack

<br>

#### stack 特性

stack 是容器适配器中的一种,它采用底层容器(通常是 vector 或 deque)实现的堆栈数据结构。

主要特点:

- 遵循 LIFO(后进先出)原理。
- 提供常用的栈操作,如入栈(push)、出栈(pop)、查看堆栈顶(top)等。

常用函数:

- push():向栈顶插入元素。
- pop():从栈顶移除第一个元素。
- top():返回栈顶元素。
- empty():判断栈是否为空。
- size():返回栈中元素个数。

底层容器:

stack 默认采用 deque 作为底层容器,deque 有效支持栈上两端的操作。
但也可以使用 vector 作为底层容器。

堆栈实现:

stack 的底层实际上是一个容器(通常是 deque 或 vector),并提供了面向栈的接口。
利用容器的插入和删除操作就可以实现栈功能。

初始化:

```cpp
stack<int> s;  // 使用deque作为默认容器

stack<int, vector<int>> s;  // 使用 vector 作为容器
```

<br>

#### stack 底层实现

stack 可以使用两种底层容器:

- vector: 作为底层容器的默认实现。
- deque: 相比 vector,deque 支持更高效的在两端插入和删除操作,更适合作为底层容器。

stack 可以直接利用 vector 和 deque 中的以下三个函数实现

- push(): 调用 push_back() 插入元素
- pop(): 调用 pop_back() 删除元素
- top(): 调用 back() 访问栈顶元素

stack 本质上是一个适配器,类似以下形式

```cpp
template<class T, class Container = deque<T> >
class stack {
public:
    void push(const T&);
    void pop();
    // ...
private:
    Container c;  // 底层容器
};
```

<br>

### queue

queue 实现和 stack 类似,也是采用底层容器(deque 或 list)来实现面向队列的接口。

底层容器:

- 默认采用 deque 作为底层容器。
- deque 支持高效的从两端插入和删除。
- 也可以使用 list 作为底层容器。

时间复杂度:

- 使用 deque 作底层容器:
  - push: O(1)
  - pop: O(1)
- 使用 list 作底层容器:
  - push: O(1)
  - pop: O(1)

实现队列接口:

- push():调用底层容器的 push_back()实现
- pop(): 调用底层容器的 pop_front()实现
- front():调用底层容器的 front()实现
- back():调用底层容器的 back()实现

队列类:

大致结构如下:

```cpp
template <class T, class Container = deque<T>>
class queue {
public:
    void push(const T& x) { c.push_back(x); }
    void pop() { c.pop_front(); }
    // ...
private:
    Container c;  //底层容器
};
```

使用 queue:

```cpp
queue<int> q;
q.push(1);
q.push(2);
q.pop();
```

<br>

### deque

<br>

#### deque 简介

deque 是 STL 容器之一,双端数组,分为严格意义上的双端队列和段双端队列两种。

主要特点:

- 支持高效的在两端插入和删除操作。时间复杂度为 O(1)。
- 支持随机访问,但效率不如 vector。
- 不必在插入删除时发生实际的数据搬移。
- 内部数据是以一系列段数组的形式分配,在需要时再分配新段。

常用函数:

- push_back():在末尾插入元素
- push_front():在队首插入元素
- pop_back():删除末尾元素
- pop_front():删除队首元素
- front():返回队首元素
- back():返回队尾元素
- [] :支持随机访问
- begin()/end():返回迭代器
- size():返回元素个数

优缺点:

- 优点:两端插入删除效率高;不需要重新分配内存
- 缺点:随机访问效率低于 vector

底层实现:

deque 内部使用一系列连续的内存块来存储元素。
每次需要时再分配新内存块。

初始化:

- 直接初始化:

```cpp
deque<int> d;
deque<int> d(10, 1); //长度10,默认元素1
```

- 列表初始化:

```cpp
deque<int> d = {1,2,3};
```

<br>

#### deque 底层实现原理

deque（双端队列）是 C++标准库中的一个容器，它是一种双端开口的序列容器，可以在两端进行高效的插入和删除操作。

deque 的底层实现基于一种叫做“分段连续空间”的数据结构。  
它将一个大的连续空间按照一定的大小分为多个小的连续空间，每个小的连续空间叫做一个“缓冲区”，每个缓冲区内部元素是连续存储的。  
deque 维护了一个双向链表，每个节点指向一个缓冲区，同时记录了缓冲区的起始地址、结束地址以及下一个缓冲区的指针。

头部插入元素，deque 会在链表头部添加一个新的节点，并将新的元素插入到头部缓冲区的前面；  
尾部插入元素，deque 会在链表尾部添加一个新的节点，并将新的元素插入到尾部缓冲区的后面。  
在头部或尾部删除元素，deque 也会根据需要调整其内部结构，以保证在常数时间内完成操作。

在实现上，deque 使用了一个名为 map 的中央控制器，它是一个指针数组，每个指针指向一个缓冲区。  
map 的大小是固定的，每个缓冲区的大小也是固定的，这些大小都是在编译时指定的。  
当我们需要在 deque 的头部或尾部插入元素时，deque 会先检查是否需要添加新的缓冲区，如果需要，就会在 map 中分配一个新的指针并指向一个新的缓冲区，以保证能够容纳新的元素。

<br>

#### deque 底层实现对性能的影响

1. 两端插入删除性能极高。deque 通过分配一系列连续的内存块实现,两端插入删除时不需要移动已有元素。所以时间复杂度是 O(1)。

2. 随机访问性能不如 vector。deque 内部元素不是顺序存储,为了随机访问一个元素,需要先定位到对应内存块,再在内存块中查找。所以效率不如 vector。

3. 内存开销较大。deque 需要维护内存块数组,每次分配新内存块也需要额外开销。总的内存开销比 vector 多。

4. 扩容方式不同。deque 每次只分配一个固定容量的内存块,而不是像 vector 那样两倍扩容。

5. 迭代器 invalid。deque 的迭代器不能跨越内存块。一旦 deque 重新分配内存块,所有迭代器都会失效。

基于这些特点,我们可以得到以下性能指标:

- 插入删除性能: deque >> vector
- 随机访问性能:vector >> deque
- 内存开销:vector < deque
- 迭代器稳定性:vector > deque

所以:

- 如果需要频繁两端插入删除,应选择 deque。
- 如果需要高效的随机访问,应选择 vector。

<br>

#### deque 的应用场景

1. 实现命令历史(命令行界面)
2. 基于双端队列实现的事件循环
3. 作为 stack 和 queue 的底层容器
4. 层次结构(如 XML 文件)遍历时使用
5. 需要高效插入删除的集合

<br>

#### deque 线程安全问题

C++标准库中的 deque 容器是一个线程不安全的容器，它的底层实现不支持多线程操作

若于多线程环境下使用，可以尝试以下几种方式

1. 使用同步机制（如互斥锁、读写锁等）来保证线程安全
2. 调用 boost 库中的线程安全的 deque 实现

C++11 标准引入了一些原子操作和同步机制（如 `std::atomic、std::mutex` 等），可以用于实现线程安全的数据结构

<br>

### list

<br>

#### list 简述

list 是 STL 中的序列式容器之一,它实现了双向链表。

主要特点:

- list 的元素不要求连续内存存储。
- 节点是分立的,用指针链接起来的。
- 支持高效的插入与删除操作。

常用函数:

- push_back():在尾部插入元素
- push_front():在头部插入元素
- pop_back():删除尾部元素
- pop_front():删除头部元素
- erase():删除指定元素
- insert():在指定位置插入元素
- sort():排序
- merge():合并
- remove():删除指定值元素

优缺点:

- 优点:对插入和删除操作效率高。
- 缺点:不支持随机访问,效率较低。

分配方式:

list 的节点分散分配,相邻节点通过指针链接在一起。

初始化:

```cpp
list<int> lst;
list<int> lst = {1, 2, 3};
```

时间复杂度:

- 插入和删除:O(1)
- 查找:O(n)
- 访问:O(n)

<br>

#### list 实现

List 的底层实现是一个双向链表，每个节点包含三个指针：一个指向前一个节点、一个指向后一个节点，以及一个指向节点中存储的元素

List 维护了两个指针，一个指向链表的头节点，一个指向链表的尾节点。当我们需要在链表的头部或尾部执行插入或删除操作时，在对应首尾使用 new 添加新节点，或者使用 delete 删除节点

对于基本类型（如 int、char 等），List 会将其直接存储在节点中；对于复杂类型（如自定义类、结构体等），List 会存储其指针或引用

List 的插入和删除操作非常高效，因为它只需要对节点的指针进行修改

<br>

#### list 多线程

同样的，他也不是一个线程安全的容器，若要保证线程安全，可以参考 deque 的对应方法

<br>

### priority_queue

<br>

#### 优先队列简介

优先队列(`Priority Queue`)是 STL 中的一个重要数据结构。它与标准的队列不同,具有以下特点:

- 队列中的元素有优先级,可以根据优先级高低对元素进行排序。
- 出队操作选择优先级最高的元素。
- 优先队列的插入和删除操作基于堆数据结构实现。

常用接口:

- push():向队列中插入一个元素
- pop():移除优先级最高的元素
- top():获取优先级最高的元素
- empty():判断队列是否为空
- size():返回队列中的元素个数

底层实现:

STL 的优先队列默认采用二叉堆实现。
堆也可以是其他类型,包括牛栏堆、斐波那契堆等。

二叉堆:

- 完全二叉树。所有父节点的元素都小于或大于子节点。
- 提供高效插入和删除操作。时间复杂度为 O(logn)。

初始化:

```cpp
priority_queue<int> max_pq;  // 最大堆,默认
priority_queue<int,vector<int>,greater<int>> min_pq; // 最小堆
```

应用:

优先队列主要用于需要高效排序的场景,例如:

- 哈夫曼编码
- Dijkstra 最短路算法
- 单源最短路径
- 高频元素统计

<br>

#### 优先队列原理

优先队列的底层实现是使用堆（Heap）数据结构。  
堆是一种完全二叉树，可以分为两种类型：最大堆和最小堆。  
在最大堆中，每个节点的值都大于或等于其子节点的值；在最小堆中，每个节点的值都小于或等于其子节点的值。  
在优先队列中，通常使用最大堆来维护元素的优先级关系

当有新元素加入优先队列时，它会被插入到堆的末尾，然后通过上滤（percolate up）操作将其移动到正确的位置，以保证堆的性质不变。  
当需要提取队列中优先级最高的元素时，堆的根节点就是优先级最高的元素，它会被弹出并返回。

优先队列通常使用 vector 或 deque 容器来实现堆

优先队列依然是线程不安全的

<br>

### set

<br>

#### set 简介

set 是 STL 中的一个重要的关联式容器,具有以下特点:

特点:

- set 中不允许包含重复元素。
- 元素会自动根据其值被排序。
- 可以快速找到元素,但是不支持随机访问。

常用接口:

- insert():插入元素
- erase():删除元素
- find():查找元素
- count():统计元素个数
- begin()/rend():返回迭代器
- size():返回元素个数
- empty():判断是否为空

底层实现:

set 默认采用红黑树来实现。

时间复杂度:

- 插入和删除:O(logN)
- 查找:O(logN)
- 遍历: O(N)

遍历:

set 提供了迭代器遍历方式和内置迭代器遍历方式:

```cpp
// 迭代器遍历
for (auto it = s.begin(); it != s.end(); ++it){
    cout << *it << endl;
}

// 内置遍历
for (int x : s) {
    cout << x << endl;
}
```

初始化:

```cpp
set<int> s;
set<int> s = {1, 2, 3};
set<string> s2;
```

<br>

#### set 底层实现

set 容器的底层实现通常使用`红黑树（Red-Black Tree）`数据结构，红黑树是一种自平衡的二叉搜索树，能够保证在最坏情况下的查找、插入和删除操作的时间复杂度为 `O(log n)`

set 容器中的元素是唯一的，因此在插入元素时，红黑树会自动去重，保证每个元素只出现一次

对于 Set 容器，STL 实现的红黑树是一个左偏树（Leftist Tree）的变种，被称为 RB-tree（Red-Black Tree）

set 容器还提供了其他一些操作，如 lower_bound()、upper_bound()、equal_range()等，这些操作都是基于 RB-tree 实现的

<br>

### map

<br>

#### map 简介

map 是 STL 中的关联式容器之一,它支持动态初始大小,允许使用用户定义的键类型。

主要特点:

- 存储的是键值对(key-value 对)。
- 可以通过键快速定位数据,同一个键只能出现一次。
- 内部使用红黑树自平衡。

常用函数:

- insert(): 插入元素
- erase(): 删除元素
- find(): 查找元素
- at(): 通过键获取元素
- begin()/end(): 返回迭代器
- size(): 返回元素个数
- empty(): 判断容器是否为空

时间复杂度:

- 插入和删除: O(logn)
- 查找: O(logn)
- 迭代: O(n)

初始化:

```cpp
map<string, int> m;
map<string, int> m = {
   {"apple", 1},
   {"banana", 2}
};
```

使用:

```cpp
m["apple"] = 100;  // 通过键设置值
int appleValue = m["apple"];
```

<br>

#### map 底层实现

map 容器的底层实现通常使用红黑树（Red-Black Tree）数据结构

在 map 容器中，元素由键值对组成，按照键的大小关系进行排序。在插入元素时，map 容器会将元素插入到红黑树中的合适位置，并保持红黑树的性质不变

<br>
