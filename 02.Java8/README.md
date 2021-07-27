## Java8版





#### [41. 缺失的第一个正数](https://leetcode-cn.com/problems/first-missing-positive/)

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        HashMap<Integer,Integer> mp=new HashMap<>();

        for(int i=0; i<nums.length; ++i)
        {
            mp.put( nums[i],1 );
        }

        int cur=1;
        while( true )
        {
            if( null==mp.get(cur) )
            {
                break;
            }

            ++cur;
        }

        return cur;
    }
}
```

