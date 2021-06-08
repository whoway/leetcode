## Python3版



## 01.数组

### 1.1.数组的遍历



####  [485. 最大连续 1 的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)



简单遍历，内存开销大

```python3
class Solution(object):
    def findMaxConsecutiveOnes(self, nums):
        n=len(nums)
        count=0
        countmax=0
        for i in range(n):
            if (nums[i]==1):
                count=count+1
            else:
                count=0
            if(count>=countmax):
                countmax=count
        return countmax
```
