# leetcode-and-algorithms
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
### Using 2 stacks to realize a queue<br>
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

