# leetcode-and-algorithms


-> 递归函数的空间复杂度： 递归层数 * 每层需要的空间 （因为每层都需要压栈存储local variables 和 地址） 
-> Amortized 复杂度： 经常很cheap，但是偶尔很expensive的复杂度（经常和stack有关）：O(2n + n-1) / n = O(1)
> <a href="#head1">Binary Search 二分搜索和变种</a><br>
> <a href="#head2">Queue, Stack and LinkedList</a><br>
> None<br>
> None<br>
## <a id = "head1">Binary Search 二分搜索和变种</a>
### 变种1：find the index of the `closet` number to target in a list<br>
仅仅需要在Binary Search里做几个改变<br> 
1. 提前停止，在left + 1 == right 的时候停止
2. 当nums[middle] < target: left = middle(而不是middle + 1,因为不能确定middle不是结果)
3. 当nums[middle] > target: right = middle(同理)
4. 对left和right两个值对比较
### 变种2： find the index of the leftmost int whose value is equal to the target 
`提前停止，它有很多好处，可以保证不会死循环的问题`, 当num[middle] == target时，right = middle<br>
```
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if len(nums) == 0:
            return [-1,-1]
        result = [None, None]
        left, right = 0, len(nums) - 1
        while left < right - 1:#提前停止，可以保证不会死循环
            middle = (left + right) // 2
            if nums[middle] == target:
                right = middle
            elif nums[middle] < target:
                left = middle + 1
            else:
                right = middle - 1
        if nums[left] == target:
            result[0] = left
        elif nums[right] == target:
            result[0] = right
        else:
            result[0] = -1
```
### 变种3： find the index of the rightmost ...
`提前停止，它有很多好处，可以保证不会死循环的问题`, 当num[middle] == target时，left = middle<br>
```
        left, right = 0, len(nums) - 1
        while left < right - 1:#提前停止，可以保证不会死循环
            middle = (left + right) // 2
            if nums[middle] == target:
                left = middle
            elif nums[middle] < target:
                left = middle + 1
            else:
                right = middle - 1
        if nums[right] == target:
            result[1] = right
        elif nums[left] == target:
            result[1] = left
        else:
            result[1] = -1
        return result
```
### 变种3 ： how to find k elements that is closest to the target number
step 1. Binary search to find the closest number<br>
step 2. 中心开花 双指针向左右找<br>
`Follow up: 能不能left + 7 = right时停止？`<br>
No!!!: 1, 2, 3, 4, `5, 20, 21, 22, 23`
## <a id = "head2">Queue, stack and LinkedList</a><br>
### Using 2 stacks to implement a queue<br>
Stack1:1,2,3,4,5<br>
Stack2:<br>
Queue push: 直接push到stack1(To buffer all the elements)<br>
Queue pop: 1. 如果stack2 is empty, push all elements from stack1 to stack2, then pop top of stack2<br>
2.if stack2 is not empty, pop top of stack 2<br>
`the complexity of push()`: O(1)<br>
`the complexity of pop()`:<br> 
1st element pop: n(pop all elements from stack1) + n(push all elements to stack2) + 1(pop top)<br>
2ed element pop: 1<br>
3rd element pop: 1<br>
...<br>
nth element pop: 1<br>
`amertized complexity is 3n / n = 3 = O(1)`  
<br>
### implement the min() function using stack with o(1):
  STACK1: 1 2 4 -1  
  STACK2: 1 1 1 -1  
  同步加同步减
### FOLLOW UP: HOW TO OPTIMIZE THE SPACE USAGE OF STACK2 ASSUMING THAT THERE ARE A LOT OF DUPLICATE ELEMENTS IN STACK1
 ![image](https://user-images.githubusercontent.com/25994245/149253764-819b6983-1489-46c7-b27c-6ec599cc7e85.png)

### SORT NUMBERS WITH 3/2 STACKS(SELECTION SORT):
    用一个stack来存所有未排序的数， 用一个stack来存所有排好序的
    
### TWO STACKS TO SIMULATE DEQUE AMERTIZEDO(1)
    两个stack各存一半，然后用另一个stack来倒腾

## LinkedList
    reverse linkedlist
    merge linkedlist
    linkedlist has circle
    find mid of linkedlist

## Binary Tree
    1. What is balanced BT? Each node, (the deepth of left child) - (deepth of right child) <= 1
    2. What is complete BT? 除了最后一层，其他都满。 最后一层往左边挤
    3. What is binary search tree: 每一个节点，左子树的值都小于当前节点，右子树的值都大于当前节点
    -> 如何求一个递归函数里面调用另一个递归函数 的 时间复杂度？？？   画递归树！！！逐层累加
![image](https://user-images.githubusercontent.com/25994245/149644924-d3be75a0-5bc3-4a60-a251-90418291a981.png)
    4. 判断是否是balanced tree？
    5.  判断一个树是不是中心轴对称
    ![image](https://user-images.githubusercontent.com/25994245/149645082-4781cbcf-633f-43cb-b66d-dbfe67cd5e2e.png)
    
    def is_symmetric(left, right):
    if not left and not right:
        return True

    if left and right and left.val == right.val:
        return is_symmetric(left.left, right.right) and is_symmetric(left.right, right.left)
        
    时间复杂度 o(n/2)
    空间复杂度 o(n/2) if it is balanced o(log(n))
## Heap(priority queue)
    存在一个list里， 从第一个元素开始计数： 
        左子树 = 2 * i; 右子树= 2* i + 1; parent = i // 2;
    总是一颗完全树
    1. insert: log(n) insert at tail and 上浮.
    2. delete: log(n) 删除堆顶，然后讲最后一个元素放入root，然后下浮(和小的换)
    3. update: log(n) 上浮 或 下浮
    4. heapify: o(n)
    
    Example: Fing the smallest k numbers from an unsorted array of size n:
        1. sort 
        2. use minheap: build a min-heap, keep popping k elements
        3. use maxheap: build a max-heap of size k, then compare n-k elements 
        4. quick select O(n)  : n + n/2 + n /4 + n /8 + .... worst case o(n** 2)
    
## Graph
    1. Adjacent matrix: 不sparse, waste too much space.  
![image](https://user-images.githubusercontent.com/25994245/150649672-e94f7638-0372-42f6-80d4-8b6c7cac035d.png)
    2. Adjacent list: save space, waste time.  
    3. Use a hashtable: 
    {v0:{v2:distance, v3: distance}}

### BFS（BFS-1）:找所有点的最短距离
    1. 分层打印tree.  
    2. Bipartite.  
![image](https://user-images.githubusercontent.com/25994245/150650875-39b612c6-3102-4d41-8ee6-c86ce45f8313.png)
    
    对每一个没有遇到过的点进行bfs, 第一层把 1 放入 u， 然后把 它的下一层放入 v, 然后 expand 2, 准备把1， 3 放入u,发现 3 已经在v里了，所以不能bipartite.    
    3. Determine whether a binary tree is a complete binary tree?   
        不能的情况是： 当一个node已经不能generate两个孩子了，那之后的node都不能有孩子！！！ 
    4. 如果需要讨论同一层的逻辑关系， 用bfs.  
    
### Dijikstra(BFS-2): 
    1. Usage: Find the shortest path cost from a single node to any other nodes in a graph.
    2. Data structure: `min_heap`
    3.1: init state(start node)
    3.2: Generation Rule
    3.3: Termination condition
     -》 每次弹出最小distance的边
     -》 弹出后更新`它能到达`的点的距离dis[]
     -》 将还没有visit过的点入heap
     -》 直到栈空
    3.4: `All the cost of nodes that are expanded are monolically increasing.`
    3.5: O(nlogn)
    3.6: if a node is poped, then the value is fixed.
![image](https://user-images.githubusercontent.com/25994245/150651784-074e5905-87ad-49f0-8d2c-5cc5c3680d8e.png)
    思路： 一层一层的筛选， 每层从小到大的访问
## DFS
Recall "Using pre-order to traverse tree"  
`Back-tracking is just a behavior` same thing  
为什么permutation问题不用bfs，因为FIFO的queue空间不够  
###基本方法：每层代表什么意义，每层有多少个状态要try  
1. print all the subset of a set S= {"a". "b", "c"}

2. find all the valid permutation using the parenthesis provided ()()()

3. print all combinations of coins that can sum up to a total value k.  eg: coins: 1, 2, 15, 25. target:99
    3.1: solution1 99 层， 每一层考虑四个硬币， 复杂度 4 ** 99
    3.2: solution2 4 层， 每层考虑加多少个对应的硬币
![image](https://user-images.githubusercontent.com/25994245/150658368-77f6bb12-4fa1-42c4-8bd1-51eff55e1ab2.png)
4. list all the permutations of list.
    trick: 
![image](https://user-images.githubusercontent.com/25994245/150660985-9b8db4a4-3272-409a-991d-0d1a23b341b4.png)
    通过i 与 ii的位置来避免list的remove和insert操作

## Hash table
    Q1: If there are one missing number from 1 to n in an unsorted array. How to find it in O(n) time?
    XOR bit operation 
![image](https://user-images.githubusercontent.com/25994245/150903265-92883de2-af2d-4032-8024-2a351fae46a6.png)

## String
ASCII编码 英文编码
unicode 所有国家文字编码
    Q1: char removal: 删除特定char，如何只用O(1)复杂度：(两个挡板，三个区域)
![image](https://user-images.githubusercontent.com/25994245/150904693-14b275a8-a2a5-4df0-9279-5e203b00aae7.png)
    Q2: Remove all leading and duplicate empty spaces from an input string:
    "___abc__ed__ef__" -> "abc_ed_ef"<br>
    Q3:Char De_duplication
![image](https://user-images.githubusercontent.com/25994245/150906652-1bccedb8-0a2f-47ad-b046-a163a6005225.png)
     fast 和 slow比较是否相等  
    Q4:  char de-duplication adjaccent letters repeatedly) abbbbbaz -> aaz-> z  O(n):
     use a stack: while ptr[j] == stack[-1], stack.pop(), stack.push 然后额外pop一次
![image](https://user-images.githubusercontent.com/25994245/150908075-c184db33-47ed-487d-bb90-d920654c2750.png)
    Q5: Sub-string finding 暴力一个一个对比：o(m * n) Rabin-karp: idea: If we can have an efficient hash function that can use O(1) to get a hash of a substring, we can use O(n) time to finish the compare: The hash function must be based on current hash and next value.
![image](https://user-images.githubusercontent.com/25994245/150917304-212884f4-514e-465b-b2ea-a3b1adb493b1.png)
    Q6: String reversal  
        I love yahoo -> yahoo love I  
        Primitive way: 根据空格区分，然后反向组合 CONS:不是in-place，需要o（n）额外空间
        solution2: step1: reverse each word. step2: reverse each sentense. Pros: inplace reverse.
    Q7: ab | cdef -> ef | abcd shift 2 chars to the right: ( love yahoo 变种)
    Q8: string replace   predentdent dent -> en
        快慢指针， 如果size是增加，则从右往左看

## String advanced topic:
    Q1: String shuffling: "A1B2C3D4" -> "ABCD1234" merge sort, 自定义比较  
    follow up:  "ABCD1234" -> "A1B2C3D4"
![image](https://user-images.githubusercontent.com/25994245/151266985-5dc6a7bd-fb8a-4790-97ee-e6034d565cfd.png)
    也是iloveyahoo的变种  
    Q2: String permutation with duplicate chars: "abbbc".  
![image](https://user-images.githubusercontent.com/25994245/151267584-5aec02b6-1737-4a0f-a297-e82d931ad45d.png)
    For each level, same word cannot to picked twice. Use memo  
    Q3: String En/Decoding "aaaaaabccaaaaa" -> "a4b1c2a5"  Restriction: in-place  
    类似于string replacement, 先resize，然后从右往左快慢指针。  
    Q4: Longest substring that contains only unique char. Given a string, return the length of the longest substring without duplicate charactrers. "BDEFGADE" is "BDEFGA"
    (follow up: without duplicate k times)  
    双指针 + hashmap  
    Q5: Find all anagrams of a substring s2 in a long string s1:   s2 = "aabc" s1="zzz cdeb caab cyw" 找同行异构体:  
    还是sliding window和hash.  
    Q6: Given a 0-1 array, you can flip at most k 0 to 1, find the longest subarray that consists of all 1s:  
    010101010100000000  
    Solution: find a sliding window that contains 4 zeros  

## DP DP的核心思想类似于数学归纳法：
    1. 如何用size = N-1的方法来构建SIZE = N 的solution.
### DP的常用解题方法: 一维的data，求max or min
    Q1: cut ropes with maximum product.  
    solution1:   
![image](https://user-images.githubusercontent.com/25994245/152440338-e59fd1e9-8df7-4586-8532-4b427464893b.png)
    solution2: 左大段， 右小段 is more preferrable to solve a problem. 大段的意思是， 我们需要通过读取表格，获得dp[left]的值，小段的意思是，我们不需要读表格，给是不做任何cut，自成一段。
![image](https://user-images.githubusercontent.com/25994245/152442512-f292b4c9-ae07-4bb7-a92f-590ebacc4da5.png)
    solution3: Non-dp recursion
    Q2: Given a word, can it be composed by concatenating words from a given dictionary? Example: bob, cat, rob?
    `我一开始的想法： 对一个bcoabt, 再所有可能的地方切一刀，然后看左大段和右大段是否可以被组成， dp[i][j] = dp[i][k] && dp[k+1][j] 但是这样就需要对进行二维dp，需要的存储空间更大。`
    ###套用`左大段右小段`， 每次切一刀，看左边是否存在dp[k]中，看右边是否存在dict中，这样只需要一维的存储空间。 证明完备性： 假如一个string可以被组成，那它必定由一个s放在最右边,左边的由剩下的s组成。
    dp[n]: if first substring with length n can be composed.
    bobb = dp[1] && str[2,n] || dp[2] && str[3, n] || dp[3]
    
    Q3: Edit Distance
![image](https://user-images.githubusercontent.com/25994245/152627371-71eeaa12-8d32-4b57-9e28-3213592a5c7f.png)

    solution1: 递归：
![image](https://user-images.githubusercontent.com/25994245/152627379-b49a5c01-06db-4d50-994f-11e58f65f275.png)

    solution2: 讲递归改成dp:
![image](https://user-images.githubusercontent.com/25994245/152627570-0d065fed-a2e5-42c6-8fe9-dc9b100a7394.png)

    


    
## cheatsheet  
-> Binary Search: 双指针，while循环，每次减少一半， 时间复杂度O(n), 空间复杂度O(1)



