## 一、什么是STL?

1. STL（Standard Template Library），即标准模板库，是一个高效的C程序库，包含了诸多常用的基本数据结构和基本算法。为广大C程序员们提供了一个可扩展的应用框架，高度体现了软件的可复用性。
2. 从逻辑层次来看，在STL中体现了泛型化程序设计的思想（generic programming）。在这种思想里，大部分基本算法被抽象，被泛化，独立于与之对应的数据结构，用于以相同或相近的方式处理各种不同情形。
3. 从实现层次看，整个STL是以一种类型参数化（type parameterized）的方式实现的，基于模板（template）。

STL有六大组件，但主要包含容器、迭代器和算法三个部分。

- 容器（Containers）：用来管理某类对象的集合。每一种容器都有其优点和缺点，所以，为了应付程序中的不同需求，STL 准备了七种基本容器类型。
- 迭代器（Iterators）：用来在一个对象集合的元素上进行遍历动作。这个对象集合或许是个容器，或许是容器的一部分。每一种容器都提供了自己的迭代器，而这些迭代器了解该种容器的内部结构。
- 算法（Algorithms）：用来处理对象集合中的元素，比如 Sort，Search，Copy，Erase 那些元素。通过迭代器的协助，我们只需撰写一次算法，就可以将它应用于任意容器之上，这是因为所有容器的迭代器都提供一致的接口。

STL 的基本观念就是将数据和操作分离。数据由容器进行管理，操作则由算法进行，而迭代器在两者之间充当粘合剂，使任何算法都可以和任何容器交互运作。

## 二、容器（Containers）

容器用来管理某类对象。为了应付程序中的不同需求，STL 准备了两类共七种基本容器类型：

1. 序列式容器（Sequence containers），此为可序群集，其中每个元素均有固定位置—取决于插入时机和地点，和元素值无关。如果你以追加方式对一个群集插入六个元素，它们的排列次序将和插入次序一致。STL提供了三个序列式容器：

- 向量（vector）
- 双端队列（deque）
- 列表（list）

2. 关联式容器（Associative containers），此为已序群集，元素位置取决于特定的排序准则以及元素值，和插入次序无关。如果你将六个元素置入这样的群集中，它们的位置取决于元素值，和插入次序无关。STL提供了四个关联式容器：

- 集合（set）
- 多重集合（multiset）
- 映射（map）
- 多重映射（multimap）。

### vector

[[vector]]（向量）: 是一种序列式容器，事实上和数组差不多，但它比数组更优越。一般来说数组不能动态拓展，因此在程序运行的时候不是浪费内存，就是造成越界。而 vector 正好弥补了这个缺陷，它的特征是相当于可拓展的数组（动态数组），它的随机访问快，在中间插入和删除慢，但在末端插入和删除快。

**特点**

- 拥有一段连续的内存空间，并且起始地址不变，因此它能非常好的支持随机存取，即 [] 操作符，但由于它的内存空间是连续的，所以在中间进行插入和删除会造成内存块的拷贝，另外，当该数组后的内存空间不够时，需要重新申请一块足够大的内存并进行内存的拷贝。这些都大大影响了 vector 的效率。
- 对头部和中间进行插入删除元素操作需要移动内存，如果你的元素是结构或类，那么移动的同时还会进行构造和析构操作，所以性能不高。
- 对最后元素操作最快（在后面插入删除元素最快），此时一般不需要移动内存，只有保留内存不够时才需要。

**优缺点和适用场景**

优点：支持随机访问，即 [] 操作和 .at()，所以查询效率高。

缺点：当向其头部或中部插入或删除元素时，为了保持原本的相对次序，插入或删除点之后的所有元素都必须移动，所以插入的效率比较低。

适用场景：适用于对象简单，变化较小，并且频繁随机访问的场景。

**例子**

以下例子针对整型定义了一个 vector，插入 6 个元素，然后打印所有元素：

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main(int argc, char* argv[])
{
    vector<int> vecTemp;

    for (int i = 0; i<6; i++)
        vecTemp.push_back(i);

    for (int i = 0; i<vecTemp.size(); i++)
        cout << vecTemp[i] <<" "; // 输出：0 1 2 3 4 5

    return 0;
}
```

### deque

==deque（double-ended queue）是由一段一段的定量连续空间构成。一旦要在 deque 的前端和尾端增加新空间，便配置一段定量连续空间，串在整个 deque 的头端或尾端。== 因此不论在尾部或头部安插元素都十分迅速。 在中间部分安插元素则比较费时，因为必须移动其它元素。deque 的最大任务就是在这些分段的连续空间上，维护其整体连续的假象，并提供随机存取的接口。

**特点**

- 按页或块来分配存储器的，每页包含固定数目的元素。
- deque 是 list 和 vector 的折中方案。兼有 list 的优点，也有vector 随机线性访问效率高的优点。

**优缺点和适用场景**

优点：支持随机访问，即 [] 操作和 .at()，所以查询效率高；可在双端进行 pop，push。

缺点：不适合中间插入删除操作；占用内存多。

适用场景：适用于既要频繁随机存取，又要关心两端数据的插入与删除的场景。

**例子**

以下例子声明了一个浮点类型的 deque，并在容器尾部插入 6 个元素，最后打印出所有元素。

```cpp
#include <iostream>
#include <deque>

using namespace std;

int main(int argc, char* argv[])
{
    deque<float> dequeTemp;

    for (int i = 0; i<6; i++)
        dequeTemp.push_back(i);

    for (int i = 0; i<dequeTemp.size(); i++)
        cout << dequeTemp[i] << " "; // 输出：0 1 2 3 4 5

    return 0;
}
```

### list

==List 由双向链表（doubly linked list）实现而成，元素也存放在堆中，每个元素都是放在一块内存中，他的内存空间可以是不连续的，通过指针来进行数据的访问，这个特点使得它的随机存取变得非常没有效率，== 因此它没有提供 [] 操作符的重载。但是由于链表的特点，它可以很有效率的支持任意地方的插入和删除操作。

**特点**

- 没有空间预留习惯，所以每分配一个元素都会从内存中分配，每删除一个元素都会释放它占用的内存。
- 在哪里添加删除元素性能都很高，不需要移动内存，当然也不需要对每个元素都进行构造与析构了，所以常用来做随机插入和删除操作容器。
- 访问开始和最后两个元素最快，其他元素的访问时间一样。

**优缺点和适用场景**

优点：内存不连续，动态操作，可在任意位置插入或删除且效率高。

缺点：不支持随机访问。

适用场景：适用于经常进行插入和删除操作并且不经常随机访问的场景。

**例子**

以下例子产生一个空 list，准备放置字符，然后将 'a' 至 'z' 的所有字符插入其中，利用循环每次打印并移除集合的第一个元素，从而打印出所有元素：

```cpp
#include <iostream>
#include <list>

using namespace std;

int main(int argc, char* argv[])
{
    list<char> listTemp;

    for (char c = 'a'; c <= 'z'; ++c)
        listTemp.push_back(c);

    while (!listTemp.empty())
    {
        cout <<listTemp.front() << " ";
        listTemp.pop_front();
    }

    return 0;
}
```

成员函数empty()的返回值告诉我们容器中是否还有元素，只要这个函数返回 false，循环就继续进行。循环之内，成员函数front()会返回第一个元素，pop_front()函数会删除第一个元素。

注意：list<指针> 完全是性能最低的做法，还不如直接使用 list<对象> 或使用 vector<指针> 好，因为指针没有构造与析构，也不占用很大内存。

### set

[[set]]（集合）由红黑树实现，其内部元素依据其值自动排序，每个元素值只能出现一次，不允许重复。

**特点**

- set 中的元素都是排好序的，集合中没有重复的元素;
- map 和 set 的插入删除效率比用其他序列容器高，因为对于关联容器来说，不需要做内存拷贝和内存移动。

**优缺点和适用场景**

优点：使用平衡二叉树实现，便于元素查找，且保持了元素的唯一性，以及能自动排序。

缺点：每次插入值的时候，都需要调整红黑树，效率有一定影响。

适用场景：适用于经常查找一个元素是否在某群集中且需要排序的场景。

**例子**

下面的例子演示 set（集合）的两个特点：

```cpp
#include <iostream>
#include <set>

using namespace std;

int main(int argc, char* argv[])
{
    set<int> setTemp;

    setTemp.insert(3);
    setTemp.insert(1);
    setTemp.insert(2);
    setTemp.insert(1);

    set<int>::iterator it;
    for (it = setTemp.begin(); it != setTemp.end(); it++)
    {
        cout << *it << " ";
    }

    return 0;
}
```

```
输出结果：1 2 3
```

一共插入了 4 个数，但是集合中只有 3 个数并且是有序的，可见之前说过的 set 集合的两个特点，有序和不重复。

当 set 集合中的元素为结构体时，该结构体必须实现运算符 ‘<’ 的重载：

```cpp
#include <iostream>
#include <set>
#include <string>

using namespace std;

struct People
{
    string name;
    int age;

    bool operator <(const People p) const
    {
        return age < p.age;
    }
};

int main(int argc, char* argv[])
{
    set<People> setTemp;

    setTemp.insert({"张三",14});
    setTemp.insert({ "李四", 16 });
    setTemp.insert({ "隔壁老王", 10 });

    set<People>::iterator it;
    for (it = setTemp.begin(); it != setTemp.end(); it++)
    {
        printf("姓名：%s 年龄：%d\n", (*it).name.c_str(), (*it).age);
    }

    return 0;
}
```

```
/*
输出结果
姓名：王二麻子 年龄：10
姓名：张三 年龄：14
姓名：李四 年龄：16 
*/
```

可以看到结果是按照年龄由小到大的顺序排列。另外 string 要使用c_str()转换一下，否则打印出的是乱码。

另外 Multiset 和 set 相同，只不过它允许重复元素，也就是说 multiset 可包括多个数值相同的元素。

### map

[[map]] 由红黑树实现，其元素都是 “键值/实值” 所形成的一个对组（key/value pairs)。每个元素有一个键，是排序准则的基础。每一个键只能出现一次，不允许重复。

map 主要用于资料一对一映射的情况，map 内部自建一颗红黑树，这颗树具有对数据自动排序的功能，所以在 map 内部所有的数据都是有序的。比如一个班级中，每个学生的学号跟他的姓名就存在着一对一映射的关系。

**特点**

自动建立 Key - value 的对应。key 和 value 可以是任意你需要的类型。

根据 key 值快速查找记录，查找的复杂度基本是 O(logN)，如果有 1000 个记录，二分查找最多查找 10次(1024)。

增加和删除节点对迭代器的影响很小，除了那个操作节点，对其他的节点都没有什么影响。

对于迭代器来说，可以修改实值，而不能修改 key。

**优缺点和适用场景**

优点：使用平衡二叉树实现，便于元素查找，且能把一个值映射成另一个值，可以创建字典。

缺点：每次插入值的时候，都需要调整红黑树，效率有一定影响。

适用场景：适用于需要存储一个数据字典，并要求方便地根据key找value的场景。

**例子**

```cpp
#include "stdafx.h"
#include <iostream>
#include <map>
#include <string>

using namespace std;

int main(int argc, char* argv[])
{
    map<int, string> mapTemp;

    mapTemp.insert({ 5,"张三" });
    mapTemp.insert({ 3, "李四"});
    mapTemp.insert({ 4, "隔壁老王" });

    map<int, string>::iterator it;
    for (it = mapTemp.begin(); it != mapTemp.end(); it++)
    {
        printf("学号：%d 姓名：%s\n", (*it).first, (*it).second.c_str());
    }

    return 0;
}
```

```
/*
输出结果：
学号：3 姓名：李四
学号：4 姓名：隔壁老王
学号：5 姓名：张三
*/
```

multimap 和 map 相同，但允许重复元素，也就是说 multimap 可包含多个键值（key）相同的元素。

### 容器配接器

除了以上七个基本容器类别，为满足特殊需求，STL还提供了一些特别的（并且预先定义好的）容器配接器，根据基本容器类别实现而成。包括：

**1. stack**名字说明了一切，stack 容器对元素采取 LIFO（后进先出）的管理策略。

**2. queue**queue 容器对元素采取 FIFO（先进先出）的管理策略。也就是说，它是个普通的缓冲区（buffer）。

**3. priority_queue**priority_queue 容器中的元素可以拥有不同的优先权。所谓优先权，乃是基于程序员提供的排序准则（缺省使用 operators）而定义。Priority queue 的效果相当于这样一个 buffer：“下一元素永远是queue中优先级最高的元素”。如果同时有多个元素具备最髙优先权，则其次序无明确定义。

## 三、总结

### 各容器的特点总结

- vector 头部与中间插入和删除效率较低，在尾部插入和删除效率高，支持随机访问。
- deque 是在头部和尾部插入和删除效率较高，支持随机访问，但效率没有 vector 高。
- list 在任意位置的插入和删除效率都较高，但不支持随机访问。
- set 由红黑树实现，其内部元素依据其值自动排序，每个元素值只能出现一次，不允许重复，且插入和删除效率比用其他序列容器高。
- map 可以自动建立 Key - value 的对应，key 和 value 可以是任意你需要的类型，根据 key 快速查找记录。

在实际使用过程中，到底选择这几种容器中的哪一个，应该根据遵循以下原则：

> 1. 如果需要高效的随机存取，不在乎插入和删除的效率，使用 vector。
> 2. 如果需要大量的插入和删除元素，不关心随机存取的效率，使用 list。
> 3. 如果需要随机存取，并且关心两端数据的插入和删除效率，使用 deque。
> 4. 如果打算存储数据字典，并且要求方便地根据 key 找到 value，一对一的情况使用 map，一对多的情况使用 multimap。
> 5. 如果打算查找一个元素是否存在于某集合中，唯一存在的情况使用 set，不唯一存在的情况使用 multiset。


### 各容器的时间复杂度分析

- vector 在头部和中间位置插入和删除的时间复杂度为 O(N)，在尾部插入和删除的时间复杂度为 O(1)，查找的时间复杂度为 O(1)；
- deque 在中间位置插入和删除的时间复杂度为 O(N)，在头部和尾部插入和删除的时间复杂度为 O(1)，查找的时间复杂度为 O(1)；
- list 在任意位置插入和删除的时间复杂度都为 O(1)，查找的时间复杂度为 O(N)；
- set 和 map 都是通过红黑树实现，因此插入、删除和查找操作的时间复杂度都是 O(log N)。

### 各容器的共性

各容器一般来说都有下列函数：默认构造函数、复制构造函数、析构函数、empty()、max_size()、size()、operator=、operator<、operator<=、operator>、operator>=、operator==、operator!=、swap()。

顺序容器和关联容器都共有下列函数：

- begin() ：返回容器第一个元素的迭代器指针；
- end()：返回容器最后一个元素后面一位的迭代器指针；
- rbegin()：返回一个逆向迭代器指针，指向容器最后一个元素；
- rend()：返回一个逆向迭代器指针，指向容器首个元素前面一位；
- clear()：删除容器中的所有的元素；
- erase(it)：删除迭代器指针it处元素。
