## C++11版



## 01.数组

### 1.1.数组的遍历

#### [485. 最大连续 1 的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

1、STL的堆

```cpp
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {

        int len=nums.size();
        if( len<=0 )
        {
            return 0;
        }

        if( 1==len )
        {
            return nums[0];//正好0和1直接表达了几个1
        }

        vector<int> preNums(len);
        preNums[0]=nums[0];
        for(int i=1; i<len; ++i)
        {
            preNums[i]= (preNums[i-1]+nums[i]) *nums[i];   
        }

        //建立最大堆
        make_heap( preNums.begin(), preNums.end() );
        return preNums[0];
    }
};
```





#### [495. 提莫攻击](https://leetcode-cn.com/problems/teemo-attacking/)

- 思考，在线段上的数学

```cpp
class Solution {
public:
    int findPoisonedDuration(vector<int>& timeSeries, int duration) {
        if( timeSeries.size()<1 )
        {
            return 0;
        }

        if( 1==timeSeries.size() )
        {
            return duration;
        }

        int res=0;

        //当前中毒，持续到某秒
        int LastTime=timeSeries[0];
        for( auto cur : timeSeries )
        {
            //假设当前中毒，持续到的时间，
            //显然，newLastTime>=LastTime;
            int newLastTime=cur+duration;

            //这才是真正的增加的中毒时间
            int temp=newLastTime-max( LastTime, cur );
            res+=temp;

            //更新时间
            LastTime=newLastTime;
        }

        return res;
    }
};
```









#### [414. 第三大的数](https://leetcode-cn.com/problems/third-maximum-number/)



```cpp
class Solution {
public:
    int thirdMax(vector<int>& nums) {

        int len=nums.size();
        if( len<=0 )
        {
            return 0;
        }

        if( 1==len )
        {
            return nums[0];
        }

        if( 2==len )
        {
            return max( nums[0], nums[1] );
        }


        set<int> temp;
        for(int i=0; i<len; ++i)
        {
            temp.insert( nums[i] );
        }

        int num=0;
        vector<int> solve;
        for( set<int>::iterator it=temp.begin(); it!=temp.end(); ++it)
        {
            solve.push_back( *it );
            ++num;
        }


        if( num<3 )
        {
            return solve[num-1];
        }
        else
        {
            return solve[num-3];
        }
        
    }
};
```







#### [628. 三个数的最大乘积](https://leetcode-cn.com/problems/maximum-product-of-three-numbers/)



#### 1.排序的解法

- 思路如下

```cpp
测试样例排序后为
情况一、
1 2 3 4 5
//最右边为，全正数
        select[0]= nums[Len-1]*nums[Len-2]*nums[Len-3];

情况二、
-2 -1 2 3 4
-2 -1 -1 2 3
//最右边为负数，正数，正数  或者 负数，负数，正数
        select[1]= nums[0]*nums[1]*nums[Len-1];

情况三、
-5 -4 -3 -2 -1
//最右边为，全负数
        select[2]= nums[0]*nums[1]*nums[2];

```

- 排序AC的代码如下

```cpp
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        int Len=nums.size();
        if( Len<3 )
        {
            return 0;
        }

        sort( nums.begin(), nums.end() );
        int select[3];
        //最右边为，全正数
        select[0]= nums[Len-1]*nums[Len-2]*nums[Len-3];

        //最右边为负数，正数，正数  或者 负数，负数，正数
        select[1]= nums[0]*nums[1]*nums[Len-1];
        
        //最右边为，全负数
        select[2]= nums[0]*nums[1]*nums[2];

        int res=INT_MIN;
        for(int i=0; i<3; ++i)
        {
            res=max( res, select[i] );
        }

        return res;
    }
};
```



#### 2.超时的解法

- 『超时的解法，但是思路可以』

```cpp
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        int Len=nums.size();
        if( Len<3 )
        {
            return 0;
        }
        vector<int> select(Len);
        //下面不需要，因为默认初始化了
        //fill( select.begin(), select.end(), 0);
        select[ Len-1 ]=1;
        select[ Len-2 ]=1;
        select[ Len-3 ]=1;

        int res=INT_MIN;
        do
        {
            int temp=1;
            for(int i=0; i<Len; ++i)
            {
                //超时的地方！！ 样例过了71/91
                if( select[i] )
                {
                    temp*=nums[i];
                }
            }
            res=max( res,temp );
        } while( next_permutation( select.begin(), select.end() ) );
        
        return res;
    }
};
```





### 1.2.统计数组中的元素



#### [645. 错误的集合](https://leetcode-cn.com/problems/set-mismatch/)

- 哈希+数列求和公式

```cpp
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        int sum=0;
        unordered_map<int,int> mp;
        int select=INT_MIN;
        for( auto cur : nums )
        {
            sum+=cur;
            mp[cur]++;
            if( 2==mp[cur] )
            {
                select=cur;
            }
        }

        int Len=nums.size();
        int trueSum=(1+Len)*Len/2;
        int distance= trueSum-sum;

        vector<int> ret;
        ret.push_back( select );
        ret.push_back( select+distance );
        return ret;
    }
};
```





## 02.字符串



### 2.1.字符

- 💦[520. 检测大写字母](https://leetcode-cn.com/problems/detect-capital/)

```cpp
class Solution {
public:
    bool detectCapitalUse(string word) {
        int CountUpper=0;
        for( auto c: word )
        {
            if( isupper(c) )
            {
                ++CountUpper;
            }
        }

        if( 0==CountUpper || word.size()==CountUpper )
        {
            return true;
        }

        if( 1==CountUpper && isupper( word[0] ) )
        {
            return true;
        }

        return false;
    }
};
```



### 2.2.回文串的定义

- [125. 验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)





### 2.3.公共前缀



### 2.4.单词



### 2.5.字符串的反转

- [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        
        int Left=0,Right=s.size()-1;
        while( Left<Right )
        {
            swap( s[Left], s[Right] );
            ++Left;
            --Right;
        }
        return ;
        
    }
};
```







## 05.双指针法

#### 1、头尾指针

[345. 反转字符串中的元音字母](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)

- (1)难以复现的bug

```cpp

map<char,int> mp;
void init()
{
    mp['a']=1;
    mp['e']=1;
    mp['i']=1;
    mp['o']=1;
    mp['u']=1;

    mp['A']=1;
    mp['E']=1;
    mp['I']=1;
    mp['O']=1;
    mp['U']=1;
}

class Solution {
public:
    string reverseVowels(string s) {
        init();
        if( s.size()<=1 )
        {
            return s;
        }

        int L=0;
        int R=s.size()-1;
        //出差的地方：会导致"qq"这样的样例出错！
        while( 0==mp[ s[L] ] && L<s.size() )
        {
            ++L;
        }

        while( 0==mp[ s[R]] && R>=0 )
        {
            --R;
        }

        while( L<R )
        {
            swap( s[L], s[R] );
            ++L;
            --R;

            while( 0==mp[ s[L] ] && L<s.size() )
            {
                ++L;
            }

            while( 0==mp[ s[R]] && R>=0 )
            {
                --R;
            }
        }

        return s;

    }
};
```



- （2）AC的代码

```cpp
map<char,int> mp;
void init()
{
    mp['a']=1;
    mp['e']=1;
    mp['i']=1;
    mp['o']=1;
    mp['u']=1;

    mp['A']=1;
    mp['E']=1;
    mp['I']=1;
    mp['O']=1;
    mp['U']=1;
}

class Solution {
public:
    string reverseVowels(string s) {
        mp.clear();
        init();
        if( s.size()<=1 )
        {
            return s;
        }

        int L=0;
        int R=s.size()-1;
        // L<s.size() && 0==mp[ s[L] ] 
        //这2个一定不能互换前后！！！
        while( L<s.size() && 0==mp[ s[L] ]   )
        {
            ++L;
        }

        while( R>=0 && 0==mp[ s[R]]   )
        {
            --R;
        }

        while( L<R )
        {
            swap( s[L], s[R] );
            ++L;
            --R;
            while( L<s.size() && 0==mp[ s[L] ]   )
            {
                ++L;
            }

            while( R>=0 && 0==mp[ s[R]]   )
            {
                --R;
            }
        }

        return s;

    }
};
```





