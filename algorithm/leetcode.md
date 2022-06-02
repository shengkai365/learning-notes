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

> **LRU Cache - 146**

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





> LFU Cache - 460