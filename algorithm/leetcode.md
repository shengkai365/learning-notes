# 力扣刷题列表

> [图灵星球](https://turingplanet.org/2020/09/18/leetcode_planning_list/)

## 1. Basic Data Structures

### 1.1 Array

> Rotate Array - 189



> Merge Sorted Array - 88



> Shift 2D Grid - 1260



> Valid Mountain Array - 941



> Move Zeros - 283



> Two Sum II - Input array is sorted - 167



> Remove Duplicates from Sorted Array - 26



> Remove Duplicates from Sorted Array II - 80



> Merge Intervals - 56



> Container With Most Water - 11



> Next Permuation - 31



> Find All Duplicates in an Array - 442



> Rotate Image - 48



> Sort Colors - 75



> Insert Interval - 57



> Trapping Rain Water - 42



### 1.2 String

> Student Attendance Record I - 551



> Reverse String - 344
>
> Reorder Data in Log Files - 937
>
> Goat Latin - 824
>
> Add Strings - 415
>
> Longest Common Prefix - 14
>
> Implement strStr() - 28
>
> ZigZag Conversion - 6
>
> String to Integer (atoi) - 8
>
> String Compression - 443
>
> Reverse Words in a String - 151
>
> Reverse Words in a String II - 186
>
> Reverse Words in a String III - 557
>
> Expressive Words - 809
>
> Add Bold Tag in String - 616
>
> Shifting Letters - 848
>
> Text Justification - 68
>
> Break a Palindrome - 1328





### 1.3 Linked List





### 1.4 Queue

### 1.5 Stack



## 2. Advanced Data Structures

### 2.1 HashSet / HashTable

### 2.2 Tree

### 2.3 Heap

### 2.4 Graph (Breadth-FS)

### 2.5 Graph (Best-FS)

### 2.6 Graph (DFS)



## 3. Basic Algorithms

### 3.1 Binary Search

### 3.2 Breadth-First Search

### 3.3 Best-First Search

### 3.4 Depth-First Search

### 3.5 Backtracking

### 3.6 Dynamic Programming 1D

### 3.7 Dynamic Programming 1D (Multiple States)

### 3.8 Dynamic Programming 2D (2D Input)

### 3.9 Dynamic Programming 2D (Two 1D Inputs)

### 3.10 Dynamic Programming 2D (1D Inputs)

### 3.11 Dynamic Programming 2D (1D Input + K)

### 3.12 Dynamic Programming (Knapsack)



## 4. Advanced Algorithms

### 4.1 Trie

### 4.2 Union Find

### 4.3 Topological Sort

### 4.4 Boyer-Moore Voting Algorithm



## 5. Complex Problems

### 5.1 HashTable + Doubly Linked List

- **LRU Cache - 146**

请你设计并实现一个满足 [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

```
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

**提示：**

- `1 <= capacity <= 3000`
- `0 <= key <= 10000`
- `0 <= value <= 105`
- 最多调用 `2 * 105` 次 `get` 和 `put`

```python
class DoubleListNode:
    def __init__(self, key=0, val=0):
        self.key = key  # 可以根据节点找key
        self.val = val
        self.pre = None
        self.nxt = None

class LRUCache:
    def __init__(self, capacity: int):
        self.dic = {}   # key(int) : DoubleListNode
        self.cap = capacity
        self.head = DoubleListNode()
        self.tail = DoubleListNode()
        self.head.nxt = self.tail
        self.tail.pre = self.head

    def remove_from_the_list(self, node):
        node.pre.nxt = node.nxt
        node.nxt.pre = node.pre

    def push_node_to_tail(self, node):
        node.nxt = self.tail
        node.pre = self.tail.pre
        node.pre.nxt = node
        self.tail.pre = node


    def get(self, key: int) -> int:
        if key in self.dic:
            self.remove_from_the_list(self.dic[key])
            self.push_node_to_tail(self.dic[key])
            return self.dic[key].val
        else:
        	return -1


    def put(self, key: int, value: int) -> None:
        if key in self.dic:
            self.remove_from_the_list(self.dic[key])
            self.push_node_to_tail(self.dic[key])
            self.dic[key].val = value
        else:
            node = DoubleListNode(key, value)
            self.push_node_to_tail(node)

            if self.cap > 0:
                self.cap -= 1
            else:
                del self.dic[self.head.nxt.key]
                self.remove_from_the_list(self.head.nxt)
```





- LFU Cache - 460

请你为 [最不经常使用（LFU）](https://baike.baidu.com/item/缓存算法)缓存算法设计并实现数据结构。

实现 `LFUCache` 类：

- `LFUCache(int capacity)` - 用数据结构的容量 `capacity` 初始化对象
- `int get(int key)` - 如果键 `key` 存在于缓存中，则获取键的值，否则返回 `-1` 。
- `void put(int key, int value)` - 如果键 `key` 已存在，则变更其值；如果键不存在，请插入键值对。当缓存达到其容量 `capacity` 时，则应该在插入新项之前，移除最不经常使用的项。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，应该去除 **最近最久未使用** 的键。

为了确定最不常使用的键，可以为缓存中的每个键维护一个 **使用计数器** 。使用计数最小的键是最久未使用的键。

当一个键首次插入到缓存中时，它的使用计数器被设置为 `1` (由于 put 操作)。对缓存中的键执行 `get` 或 `put` 操作，使用计数器的值将会递增。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。



**示例：**

```
输入：
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
输出：
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

解释：
// cnt(x) = 键 x 的使用计数
// cache=[] 将显示最后一次使用的顺序（最左边的元素是最近的）
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // 返回 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 去除键 2 ，因为 cnt(2)=1 ，使用计数最小
                 // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // 返回 -1（未找到）
lfu.get(3);      // 返回 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // 去除键 1 ，1 和 3 的 cnt 相同，但 1 最久未使用
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // 返回 -1（未找到）
lfu.get(3);      // 返回 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // 返回 4
                 // cache=[3,4], cnt(4)=2, cnt(3)=3
```

 

**提示：**

- `0 <= capacity <= 104`
- `0 <= key <= 105`
- `0 <= value <= 109`
- 最多调用 `2 * 105` 次 `get` 和 `put` 方法



```python
```

