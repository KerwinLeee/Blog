# 数据结构

## 数据结构分类

### 数组

> 用来存储多个数据的集合。一般在创建数组的时候，需要确定其最大容量。如果数据过大，只能扩容，较耗性能。数组相对于链表来说，**取下标值的数据更快**。但是插入和删除速度较慢，由于其在中间部位插入数据，后面数据需要逐个后移，留出一定空间才可以操作。

### 栈

> 栈是一种受限的线性表，**后进先出(LIFO)**。其限制只能在**栈的一端对栈进行插入，删除**。这一端称为`栈顶`，而末端称为`栈底`。

```ts
function Stack() {
  this.items = [];

  // 压栈
  Stack.prototype.push = function(element) {
    this.items.push(element);
  };

  // 从栈中取出
  Stack.prototype.pop = function() {
    return this.items.pop();
  };

  // 查看栈顶元素
  Stack.prototype.peek = function() {
    return this.items[this.items.length - 1];
  };

  // 判断栈是否为空
  Stack.prototype.isEmpty = function() {
    return this.items.length === 0;
  };

  // 获取栈中元素的个数
  Stack.prototype.size = function() {
    return this.items.length;
  };

  // toString方法
  Stack.prototype.toString = function() {
    var resultStr = "";
    for (let i = 0; i < this.items.length; i++) {
      resultStr += i;
    }
    return resultStr;
  };
}

const list = new Stack();
```

### 普通队列

> 队列是一种受限的线性表，**先进先出**。它只允许在**表的前端进行删除操作**，而在**表的后端进行插入操作**。

使用数组实现队列

```ts
function Queue() {
  this.items = [];
  // 将元素添加到队列中
  Queue.prototype.enqueue = function(element) {
    this.items.push(element);
  };

  // 从队列中删除元素
  Queue.prototype.dequeue = function() {
    return this.items.shift();
  };

  // 查看队列第一个的元素
  Queue.prototype.front = function() {
    return this.items[0];
  };

  // 查看队列是否为空
  Queue.prototype.isEmpty = function() {
    return this.items.length === 0;
  };

  // 查看队列中元素的个数
  Queue.prototype.size = function() {
    return this.items.length;
  };

  // toString方法
  Queue.prototype.toString = function() {
    var resultStr = "";
    for (let i = 0; i < this.items.length; i++) {
      resultStr += i;
    }
    return resultStr;
  };
}
```

### 优先级队列

> 队列中出了`数据`，还包含其数据的`优先级`

```ts
function PriorityQueue() {
  this.items = [];

  // 内部类,数据项实例（数据项 = 数据 + 优先级）
  function PriorityElement(element, priority) {
    this.element = element;
    this.priority = priority;
  }

  PriorityQueue.prototype.enqueue = function(element, priority) {
    var tempElement = new PriorityElement(element, priority);

    if (!this.items.length) {
      this.items.push(tempElement);
    } else {
      for (let i = 0; i < this.items.length; i++) {
        if (tempElement.priority > this.items[i].priority) {
          this.items.splice(i, 0, tempElement); // 指定位置i处添加数据项
          return;
        }
      }
      this.items.push(tempElement); // 当前数据项优先级最低，直接末尾插入
    }
  };
}

var priorityQueue = new PriorityQueue();
priorityQueue.enqueue(1, 1);
priorityQueue.enqueue(3, 3);
priorityQueue.enqueue(2, 2);
console.log(priorityQueue);
```

### 单向链表

> 链表中的元素在内存中**不必是连续的空间**，链表的每一个元素都是由**一个存储元素本身的节点**和**指向下一个元素的引用**组成。相对于数组来说，链表的**插入和删除操作速度更快**，但在取下标值的数据时很慢，由于其要根据引用逐个进行查找。

```ts
function LinkedList() {
  function Node(data) {
    this.data = data;
    this.next = null;
  }
  // 链表属性
  this.head = null;
  this.length = 0;

  // 末尾添加元素
  LinkedList.prototype.append = function(data) {
    var newNode = new Node(data);

    if (this.length === 0) {
      this.head = newNode;
    } else {
      var current = this.head; // 从head开始，逐个查询其节点数据是否存在
      while (current.next) {
        current = current.next;
      }
      current.next = newNode;
    }
    this.length += 1;
  };

  // 转换为字符串
  LinkedList.prototype.toString = function() {
    var tempString = "";
    var current = this.head;
    while (current && current.data) {
      tempString += ` ${current.data} `;
      current = current.next || null;
    }
    return tempString;
  };

  // 插入元素
  LinkedList.prototype.insert = function(position, data) {
    var newNode = new Node(data);
    // 越界判断
    if (position < 0 || position > this.length) {
      throw new Error("Error: out of bounds");
      return;
    }

    if (position === 0) {
      newNode.next = this.head; // 将新数据项的指针，指向头部
      this.head = newNode; // 指定新数据项为头部
    } else {
      var previous = null;
      var current = this.head;
      var index = 0;

      while (index++ < position) {
        previous = current;
        current = current.next;
      }
      newNode.next = current;
      previous.next = newNode;
      this.length += 1;
    }
  };

  // 获取指定index的数据
  LinkedList.prototype.get = function(position) {
    // 越界判断
    if (position < 0 || position >= this.length) {
      throw new Error("Error: out of bounds");
      return;
    }
    let index = 0;
    let current = this.head;

    while (index++ < position) {
      current = current.next;
    }
    return current.data;
  };

  // 获取指定数据的下标
  LinkedList.prototype.indexOf = function(data) {
    var current = this.head;

    for (var i = 0; i < this.length; i++) {
      if (current.data === data) {
        return i;
      }
      current = current.next;
    }
    return -1;
  };

  // 修改指定位置元素
  LinkedList.prototype.update = function(position, data) {
    // 越界判断
    if (position < 0 || position >= this.length) {
      throw new Error("Error: out of bounds");
      return;
    }
    let index = 0;
    let current = this.head;

    while (index++ < position) {
      current = current.next;
    }
    current.data = data;
  };

  // 移除指定位置的元素
  LinkedList.prototype.removeAt = function(position) {
    // 越界判断
    if (position < 0 || position >= this.length) {
      throw new Error("Error: out of bounds");
      return;
    }

    var current = this.head;

    if (position === 0) {
      this.head = current.next;
    } else {
      var previous = null;
      var index = 0;

      while (index++ < position) {
        previous = current;
        current = current.next;
      }
      previous.next = current.next || null;
    }
    return current.data;
  };

  // 移除指定元素
  LinkedList.prototype.remove = function(data) {
    var position = this.indexOf(data);
    this.removeAt(position);
  };

  LinkedList.prototype.isEmpty = function() {
    return !!this.length;
  };

  LinkedList.prototype.size = function() {
    return this.length;
  };
}

var linkedList = new LinkedList();
linkedList.append("a");
linkedList.append("b");
linkedList.append("c");
linkedList.insert(1, "a++");
linkedList.update(0, "aaa");
// linkedList.removeAt(3)
linkedList.remove("b");
// linkedList.removeAt(linkedList.length -1)
// console.log(linkedList.indexOf('kerwin'))
// console.log(linkedList.get(1));
console.log(linkedList.toString());
```

### 双向链表

> 链表可以**从头到尾**遍历，也可以**从尾到头**遍历

```js
function DoublyLinkedList() {
  // 属性
  this.head = null;
  this.tail = null;
  this.length = 0;

  function Node(data) {
    this.data = data;
    this.next = null;
    this.prev = null;
  }

  DoublyLinkedList.prototype.append = function(data) {
    var newNode = new Node(data);

    if (!this.length) {
      this.head = newNode;
      this.tail = newNode;
      this.length += 1;
      return;
    }
    this.tail.next = newNode;
    newNode.prev = this.tail;
    this.tail = newNode;
    this.length += 1;
  };

  DoublyLinkedList.prototype.backwardString = function() {
    var current = this.head;
    var resultStr = "";

    while (current) {
      resultStr += current.data + " ";
      current = current.next;
    }
    return resultStr;
  };

  DoublyLinkedList.prototype.forwardString = function() {
    var current = this.tail;
    var resultStr = "";

    while (current) {
      resultStr += current.data + " ";
      current = current.prev;
    }
    return resultStr;
  };

  DoublyLinkedList.prototype.insert = function(position, data) {
    // 越界判断
    if (position < 0 || position > this.length) return;

    var newNode = new Node(data);

    if (!this.length) {
      this.head = newNode;
      this.tail = newNode;
      this.length += 1;
      return;
    }

    if (position === 0) {
      this.head.prev = newNode; // 将当前head移至第二个位置，并将prev指针指向新心结
      newNode.next = this.head; // 绑定新节点的next指针
      this.head = newNode; // 生成新的头节点
    } else if (position === this.length) {
      this.tail.next = newNode; // 末尾节点next指针指向新节点
      newNode.prev = this.tail; // 新节点的prev指针指向上一个节点
      this.tail = newNode; // 生成新的尾节点
    } else {
      var current = this.head;

      for (var i = 0; i < position; i++) {
        current = current.next;
      }

      newNode.next = current; // 新节点的next指针
      newNode.prev = current.prev; // 新节点的prev指针
      current.prev.next = newNode; // 修改当前节点的上一个节点next指针
      current.prev = newNode; // 修改当前节点的prev指针
    }
    this.length += 1;
  };

  DoublyLinkedList.prototype.get = function(position) {
    let current = this._getCurrent(position);

    return current.data;
  };

  DoublyLinkedList.prototype.indexOf = function(data) {
    var current = this.head;
    for (let i = 0; i < this.length; i++) {
      if (current.data === data) {
        return i;
      }
      current = current.next;
    }
    return -1;
  };

  DoublyLinkedList.prototype._getCurrent = function(position) {
    if (position < 0 || position >= this.length) return null;

    // 判断从头 or 从尾开始遍历
    if (position < this.length / 2) {
      current = this.head;
      for (let i = 0; i < position; i++) {
        current = current.next;
      }
    } else {
      current = this.tail;
      for (let i = this.length - 1; i > position; i--) {
        current = current.prev;
      }
    }
    return current;
  };

  DoublyLinkedList.prototype.update = function(position, data) {
    let current = this._getCurrent(position);

    current.data = data;
  };

  DoublyLinkedList.prototype.removeAt = function(position) {
    if (position < 0 || position >= this.length) return;
    var current = this.head;

    if (this.length === 1) {
      this.head = null;
      this.tail = null;
      return;
    }

    if (position === 0) {
      var temp = this.head.next;
      temp.prev = null;
      this.head = temp;
    } else if (position === this.length - 1) {
      current = this.tail;
      var temp = this.tail.prev;
      temp.next = null;
      this.tail = temp;
    } else {
      current = this._getCurrent(position);
      var prev = current.prev;
      var next = current.next;
      prev.next = next;
      next.prev = prev;
    }
    this.length -= 1;
    return current.data;
  };

  DoublyLinkedList.prototype.remove = function(data) {
    var index = this.indexOf(data);
    return this.removeAt(index);
  };

  DoublyLinkedList.prototype.isEmpty = function() {
    return !!this.length;
  };

  DoublyLinkedList.prototype.size = function() {
    return this.length;
  };

  DoublyLinkedList.prototype.getHead = function() {
    return this.head.data;
  };

  DoublyLinkedList.prototype.getTail = function() {
    return this.tail.data;
  };
}

var list = new DoublyLinkedList();
list.append("kerwin");
list.append("bob");
list.insert(0, 0);
list.insert(1, 1);
list.insert(3, 3);

// console.log(list.removeAt(4));
// console.log(list.remove("kerwin"));
console.log(list.backwardString());
console.log(list.get(2));
console.log(list.indexOf("kerwin"));
list.update(1, "ddd");
console.log(list.backwardString());
```

### 集合

> 集合通常是由一组*无序的，不能重复*的元素构成

```js
function Set() {
  this.items = {};

  Set.prototype.add = function(value) {
    if (this.has(value)) {
      return false;
    }
    this.items[value] = value;
    return true;
  };

  Set.prototype.has = function(value) {
    return this.items.hasOwnProperty(value);
  };

  Set.prototype.remove = function(value) {
    if (!this.has(value)) {
      return false;
    }
    delete this.items[value];
  };

  Set.prototype.clear = function() {
    this.items = {};
  };

  Set.prototype.size = function() {
    return Object.keys(this.items).length;
  };

  Set.prototype.values = function() {
    return Object.keys(this.items);
  };

  // 并集
  Set.prototype.union = function(otherSet) {
    var tempSet = new Set();
    var values = this.values();
    var otherValues = otherSet.values();
    for (var i = 0; i < values.length; i++) {
      tempSet.add(values[i]);
    }
    for (var i = 0; i < otherValues.length; i++) {
      tempSet.add(otherValues[i]);
    }
    return tempSet;
  };

  // 交集
  Set.prototype.intersection = function(otherSet) {
    var tempSet = new Set();
    var values = otherSet.values();
    for (let i = 0; i < values.length; i++) {
      if (this.has(values[i])) {
        tempSet.add(values[i]);
      }
    }
    return tempSet;
  };

  // a集合的差集
  Set.prototype.difference = function(otherSet) {
    var tempSet = new Set();
    var values = this.values();
    for (let i = 0; i < values.length; i++) {
      if (!otherSet.has(values[i])) {
        tempSet.add(values[i]);
      }
    }
    return tempSet;
  };

  // 判断a是否为b的子集
  Set.prototype.subset = function(otherSet) {
    var values = this.values();
    for (let i = 0; i < values.length; i++) {
      if (!otherSet.has(values[i])) {
        return false;
      }
    }
    return true;
  };
}

var set = new Set();
set.add(0);
set.add(1);
var otherSet = new Set();
otherSet.add("a");
otherSet.add("b");
otherSet.add("1");
const tempSet = set.union(otherSet);
const intersectionSet = set.intersection(otherSet);
const differenceSet = set.difference(otherSet);

console.log("a集合:", set.values(), "b集合:", otherSet.values());

console.log("并集:", tempSet.values());
console.log("交集", intersectionSet.values());
console.log("a对b的差集", differenceSet.values());
console.log("a是否为b的子集", set.subset(otherSet));
```


### 哈希表

> 哈希表通常是基于`数组`实现的。它是*无序的*,_key 值也不能重复_。其优点在于提供非常快速的*插入-删除-查找*操作。但是其空间利用率不高，有很多空的单元格，来防止数据存储冲突。

- 哈希化：将大数字转换为数组范围内下标的过程（这个过程可能存在数字相同情况，可以使用`链地址法 开发地址法`来进行规避）
- 哈希函数：将单词/值转换为大数字，大数字再进行哈希化的实现函数

- 链地址法：在哈希化后，存储在数组中的值为一个`数组`或者`链表`，其包含了`哈希化前对应的大数字`
  ![链地址法](https://raw.githubusercontent.com/kerwin-ly/Blog/master/assets/imgs/link-address.png)

- 开发地址法：寻找空白的单元格来添加重复的数据(其寻找的方法有 3 种，` 线性探测``二次探索``再哈希法 `)
  ![链地址法](https://raw.githubusercontent.com/kerwin-ly/Blog/master/assets/imgs/link-address.png)

- 线性探测：根据索引，`index++`逐个查找`空的位置`进行插入。在根据索引查看该值的时候，同理。首先根据索引获取对应的值，再和自己期望的值进行对比。如果对比不相等。则继续`index++`查找。直到找到`某个index下的值为空`为止。
  注意：在删除时候，`不能将对应的值设置为null`，不然查找的时候会出错。可以设置为一个特殊值。如：-1。线性探索也有个弊端，就是`聚集`，如：填充 22,23,24,25，中间连续占用了控制。在添加 32 时候，需要连续探索很长的链才能发现空余的空间。为解决`聚集`的问题，可以使用`二次探索`方法。

- 二次探索：优化探测时的步长，如从下标值 x 开始，$x+1^2$，$x+2^2$...使连续的步长较短

- 再哈希法：把关键字用另外一个哈希函数（函数函数如下）`再做一次哈希`，用这次哈希化的结果来作为步长

```shell
# const为质数，且小于数组的容量，key为关键字
stepSize = constant - ( key % constant );
```

链地址法实现哈希表

```js
function HashTable(limit) {
  const minLimit = 7;
  this.storage = [];
  this.count = 0;
  this.limit = minLimit;

  // 哈希化函数,str => 字符串,size => hash值范围
  HashTable.prototype.hashFunc = function(str, size) {
    var hashCode = 0;
    var primeNum = 37; // 质数

    for (var i = 0; i < str.length; i++) {
      hashCode = primeNum * hashCode + str.charCodeAt(i);
    }

    return hashCode % size; // 将大数字的值映射到size范围中
  };

  // 添加/编辑操作, key => 关键字, value => 值
  HashTable.prototype.put = function(key, value) {
    var index = this.hashFunc(key, this.limit); // 通过哈希函数生成对应的index坐标值
    var bucket = this.storage[index]; // 获取存储的'桶'
    // 如果桶不存在，则放置一个空
    if (!bucket) {
      this.storage[index] = [];
      this.storage[index].push([key, value]);
      return;
    }
    // 判断bucket里面是否已存在对应数据，如果是，则修改其value值
    for (var i = 0; i < bucket.length; i++) {
      var tuple = bucket[i];
      if (tuple[0] === key) {
        tuple[1] = value;
        return;
      }
    }
    // 新增数据
    this.storage[index].push([key, value]);
    this.count++;

    // 判断是否应该扩容
    if (this.count > this.limit * 0.75) {
      this.resize(this.getPrime(this.limit * 2)); // 这里控制limit为质数，是为了让数据在哈希表上分布更为均匀
    }
  };

  HashTable.prototype.get = function(key) {
    var index = this.hashFunc(key, this.limit); // 通过哈希函数生成对应的index坐标值
    var bucket = this.storage[index]; // 获取存储的'桶'

    if (!bucket) return null;

    for (var i = 0; i < bucket.length; i++) {
      var tuple = bucket[i];
      if (tuple[0] === key) {
        return tuple[1];
      }
    }

    return null; // 循环后未找到对应key，直接返回null
  };

  HashTable.prototype.remove = function(key) {
    var index = this.hashFunc(key, this.limit); // 通过哈希函数生成对应的index坐标值
    var bucket = this.storage[index]; // 获取存储的'桶'

    if (!bucket) return null;

    for (var i = 0; i < bucket.length; i++) {
      var tuple = bucket[i];
      if (tuple[0] === key) {
        this.storage[index].splice(i, 1);
        this.count--;
        // 判断是否应该缩减容量
        if (this.limit > minLimit && this.count < this.limit * 0.25) {
          this.resize(this.getPrime(Math.floor(this.limit / 2)));
        }
        return true;
      }
    }
    return false;
  };

  // 扩容操作，limit: 限定数据量
  HashTable.prototype.resize = function(limit) {
    var oldStorage = this.storage;
    this.storage = [];
    this.count = 0;
    this.limit = limit;

    for (var i = 0; i < oldStorage.length; i++) {
      var bucket = oldStorage[i];
      if (!bucket) {
        continue;
      }
      for (var j = 0; j < bucket.length; j++) {
        this.put(bucket[j][0], bucket[j][1]);
      }
    }
  };

  HashTable.prototype.isEmpty = function() {
    return this.count === 0;
  };

  HashTable.prototype.size = function() {
    return this.count;
  };

  // 判断数字是否为质数
  HashTable.prototype.isPrime = function(num) {
    var temp = parseInt(Math.sqrt(num)); // 获取平方根后的值

    for (var i = 2; i <= temp; i++) {
      if (num % i === 0) return false;
    }
    return true;
  };

  // 获取质数
  HashTable.prototype.getPrime = function(num) {
    var tempNum = num;
    while (!this.isPrime(tempNum)) {
      tempNum++;
    }
    return tempNum;
  };
}

var hash = new HashTable();

for (var x = 0; x < 30; x++) {
  hash.put("name" + x, "kerwin" + x);
}

console.log("count", hash.count);
console.log("limit", hash.limit);
console.log("hash", JSON.stringify(hash));
```

### 树结构
>一般用于存储层级的数据结构，有明确的所属关系。

#### 基本概念

```
    	    8  （8是根节点）
    	   /  \
    	  6   10 （10是9和11的父节点）
    	 /    / \
    	5    9  11 （9是10的左子节点，11是10的右子节点）
    （5，9，11是这颗树的叶子节点）     
```

儿子兄弟表示法表示树结构
![儿子兄弟表示法](https://raw.githubusercontent.com/kerwin-ly/Blog/master/assets/imgs/tree-son-bro.png)

* 二叉树：在使用`儿子兄弟法`表示树结构后，旋转其45度，便可很明显的看到一个`二叉树`的结构（二叉树：一个节点中最多含有2个子节点）。所以**任何一个树都可以用二叉树进行表示**
![二叉树](https://raw.githubusercontent.com/kerwin-ly/Blog/master/assets/imgs/binary-tree.png)

* 二叉搜索树：它是一个二叉树，可以为空。同时需要满足几个特性。**非空左子树的所有键值小于其根节点的键值**；**非空右子树的所有键值大于其根节点的键值**；**左右子树本生也是二叉搜索树**。其可以高效的查找目标节点。（二分法查找底层实际就是二叉搜索树的构建）
![二叉搜索树](https://raw.githubusercontent.com/kerwin-ly/Blog/master/assets/imgs/binary-search-tree.png)

#### 二叉搜索树的遍历

**先序遍历**：访问根节点 => 先序遍历其左子树 => 先序遍历其右子树（实现参考下面代码）。如图
![先序遍历](https://raw.githubusercontent.com/kerwin-ly/Blog/master/assets/imgs/binary-tree-first-order.png)

**中序遍历**：中序遍历其左子树 => 访问根节点 => 中序遍历其右子树（实现参考下面代码）。如图
![中序遍历](https://raw.githubusercontent.com/kerwin-ly/Blog/master/assets/imgs/binary-tree-middle-order.png)

**后序遍历**：后序遍历其左子树 => 后序遍历其右子树 => 访问根节点（实现参考下面代码）。如图
![后序遍历](https://raw.githubusercontent.com/kerwin-ly/Blog/master/assets/imgs/binary-tree-middle-order.png)

使用链表封装二叉搜索树
```js
function BinarySearchTree() {
  // 属性
  this.root = null;

  // 内部类
  function Node(key, value) {
    this.key = key;
    this.value = value;
    this.left = null;
    this.right = null;
  }

  // 对外暴露insert方法
  BinarySearchTree.prototype.insert = function (key, value) {
    // 创建节点
    var newNode = new Node(key, value);

    // 判断根节点是否有值
    if (this.root === null) {
      this.root = newNode;
      return;
    }

    this._insertNode(this.root, newNode);
  };

  // 私有方法，二叉树匹配，插入到对应位置
  BinarySearchTree.prototype._insertNode = function (node, newNode) {
    // 新节点比原节点key值更大，对右边的树节点进行比较
    if (node.key < newNode.key) {
      if (node.right === null) {
        node.right = newNode;
        return;
      }
      this._insertNode(node.right, newNode);
    } else {
      if (node.left === null) {
        node.left = newNode;
        return;
      }
      this._insertNode(node.left, newNode);
    }
  };

  // 先序遍历
  BinarySearchTree.prototype.preOrderTraversal = function (handler) {
    _preOrderTraversalNode(this.root, handler);
  };

  function _preOrderTraversalNode(node, handler) {
    if (!node) return;

    // 这里的执行顺序，类似 洋葱模型
    handler(node.key);
    // 一直查找到最底层的左节点
    _preOrderTraversalNode(node.left, handler);
    // 根据调用栈，从最底层开始查找右节点
    _preOrderTraversalNode(node.right, handler);
  }

  // 中序遍历
  BinarySearchTree.prototype.midOrderTraversal = function (handler) {
    _midOrderTraversalNode(this.root, handler);
  };

  // 私有方法，递归调用中序遍历方法
  function _midOrderTraversalNode(node, handler) {
    if (!node) return;

    _midOrderTraversalNode(node.left, handler);
    handler(node.key);
    _midOrderTraversalNode(node.right, handler);
  }

  // 后序遍历
  BinarySearchTree.prototype.postOrderTraversal = function (handler) {
    _postOrderTraversalNode(this.root, handler);
  };

  function _postOrderTraversalNode(node, handler) {
    if (!node) return;

    _postOrderTraversalNode(node.left, handler);
    _postOrderTraversalNode(node.right, handler);
    handler(node.key);
  }

  // 最小值
  BinarySearchTree.prototype.min = function () {
    let node = this.root;
    let key = null;

    while (node) {
      key = node.key;
      node = node.left;
    }
    return key;
  };

  // 最大值
  BinarySearchTree.prototype.max = function () {
    let node = this.root;
    let key = null;

    while (node) {
      key = node.key;
      node = node.right;
    }
    return key;
  };

  // 获取特定的值
  BinarySearchTree.prototype.search = function (key) {
    return _searchNode(this.root, key);
  };

  function _searchNode(node, key) {
    if (!node) return false;

    if (key < node.key) {
      return _searchNode(node.left, key);
    } else if (key > node.key) {
      return _searchNode(node.right, key);
    } else {
      return node.key;
    }
  }

  // 删除操作
  BinarySearchTree.prototype.remove = function (key) {
    let parent = null;
    let current = this.root;

    // 获取需删除的节点 current
    while (key !== current.key) {
      parent = current;
      if (key < current.key) {
        current = current.left;
      } else if (key > current.key) {
        current = current.right;
      }
      if (!current) return false;
    }

    // 判断是否删除的是根节点
    if (this.root.key === key) {
      this.root = null;
    } else {
      // 逻辑处理，replace节点替换current节点

      // 左右节点为空
      if (!current.left && !current.right) {
        if (current.key < parent.key) {
          parent.left = null;
        } else {
          parent.right = null;
        }
      } else if (current.left && !current.right) {
        // 左节点存在，右节点为空
        if (current.key < parent.key) {
          parent.left = current.left;
        } else {
          parent.right = current.left;
        }
      } else if (!current.left && current.right) {
        // 左节点为空，右节点存在
        if (current.key < parent.key) {
          parent.left = current.right;
        } else {
          parent.right = current.right;
        }
      } else {
        // 左右子节点均存在
        const replaceNode = getSuccessor(current);

        // 判断删除节点是 在 parent的左子节点 or 右边子节点
        if (current.key < parent.key) {
          parent.left = replaceNode;
        } else {
          parent.right = replaceNode;
        }
        replaceNode.right = current.right;
      }
    }
  };

  /**
   * 获取先驱 节点
   * 前驱：比current小一点点的节点称为前驱，其是current节点左子树的最大值
   * 后继：比current大一点点的节点称为后继，其是current节点右子树的最小值
   * @param {*} delNode
   * @returns
   */
  function getSuccessor(delNode) {
    let replaceNode = delNode.left; // 先驱节点
    let successorParent = delNode; // 先驱节点的父节点

    while (replaceNode.right) {
      successorParent = replaceNode;
      replaceNode = replaceNode.right;
    }

    if (replaceNode !== delNode.left) {
      successorParent.right = replaceNode.left; // 如果“替换节点”的左节点存在，则挂载到其父节点的右节点上
      replaceNode.left = delNode.left;
    }

    return replaceNode;
  }
}

const tree = new BinarySearchTree();
let result = '';
let midResult = '';
let endResult = '';

tree.insert(9, 9);
tree.insert(8, 8);
tree.insert(4, 4);
tree.insert(13, 13);
tree.insert(12, 12);
tree.insert(15, 15);
tree.insert(5, 5);
tree.insert(6, 6);
tree.insert(10, 10);
tree.insert(11, 11);
tree.insert(14, 14);
tree.insert(16, 16);

tree.remove(15);

tree.preOrderTraversal(function (key) {
  result += `-${key}`;
});

tree.midOrderTraversal(function (key) {
  midResult += `-${key}`;
});

tree.postOrderTraversal(function (key) {
  endResult += `-${key}`;
});

console.log('先序遍历结果：', result);
console.log('中序遍历结果：', midResult);
console.log('后序遍历结果：', endResult);
console.log('最小值：', tree.min());
console.log('最大值：', tree.max());
console.log('搜索 10，结果为：', tree.search(10));
console.log('搜索 5，结果为：', tree.search(5));

```