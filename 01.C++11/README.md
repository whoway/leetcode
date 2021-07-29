## C++11版



## ✅01.数组

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

### 1.3.数组的改变、移动

| 题号                                                         | C++11 | Java8 |
| ------------------------------------------------------------ | ----- | ----- |
| [453. 最小移动次数使数组元素相等](https://leetcode-cn.com/problems/minimum-moves-to-equal-array-elements/) |       |       |
| [665. 非递减数列](https://leetcode-cn.com/problems/non-decreasing-array/) |       |       |
| ✅[283. 移动零](                                              |       |       |



### 1.4.二维数组及滚动数组

| 题号                                                         | 描述 | 状态 |
| ------------------------------------------------------------ | ---- | ---- |
| ✅[118. 杨辉三角](https://leetcode-cn.com/problems/pascals-triangle/) |      |      |
| ✅[119. 杨辉三角 II](https://leetcode-cn.com/problems/pascals-triangle-ii/) |      |      |
| [661. 图片平滑器](https://leetcode-cn.com/problems/image-smoother/) |      |      |
| [598. 范围求和 II](https://leetcode-cn.com/problems/range-addition-ii/) |      |      |
| [419. 甲板上的战舰](https://leetcode-cn.com/problems/battleships-in-a-board/) |      |      |



### 1.5.数组的旋转

| 题号                                                         | C++11 | 完成状态 |
| ------------------------------------------------------------ | ----- | -------- |
| ⭐️[189. 旋转数组](https://leetcode-cn.com/problems/rotate-array/) | 1     |          |
| [396. 旋转函数](                                             |       |          |



### 1.6.特定顺序遍历二维数组

#### ⭐️[54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> res;
        if( 0==matrix.size() ) 
        {
            return res;
        }
        
        //左上和右下2个点确定一个矩形
        int LeftUpRow=0,LeftUpCol=0;
        int RightDownRow=matrix.size()-1,RightDownCol=matrix[0].size()-1;

        //开始『减而治之』的遍历
        while( LeftUpRow<=RightDownRow && LeftUpCol<=RightDownCol )
        {
            for(int loop=LeftUpCol; loop<=RightDownCol; ++loop)
            {
                res.push_back( matrix[LeftUpRow][loop] );
            }
            ++LeftUpRow;//遍历完一行之后，将左上角的点移动位置『减而治之』
            
            //易错点，注意这样的样例：[ [2,4] ]
            if( LeftUpRow>RightDownRow || LeftUpCol>RightDownCol ) break;
            for(int loop=LeftUpRow; loop<=RightDownRow; ++loop)
            {
                res.push_back( matrix[loop][RightDownCol] );
            }
            --RightDownCol;
             
            if( LeftUpRow>RightDownRow || LeftUpCol>RightDownCol ) break;
            for(int loop=RightDownCol; loop>=LeftUpCol; --loop)
            {
                res.push_back( matrix[RightDownRow][loop] );
            }
            --RightDownRow;

            if( LeftUpRow>RightDownRow || LeftUpCol>RightDownCol ) break;
            for(int loop=RightDownRow; loop>=LeftUpRow; --loop)
            {
                res.push_back( matrix[loop][LeftUpCol] );
            }
            ++LeftUpCol;
        }

        return res;
    }
};
```



#### ⭐️[59. 螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/)

- 是上一道题的影子题，上一道是读取，这一道是写入
- 记忆技巧：`vector< vector<int> >  solve( 3 , vector<int>(4) );  //初始化solve，行为3，列为4`

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        
        vector< vector<int> > matrix( n, vector<int>(n) );
        if( 0==n )
        {
            return matrix;
        }

        int curNum=1;
        //左上和右下2个点确定一个矩形
        int LeftUpRow=0,LeftUpCol=0;
        int RightDownRow=n-1,RightDownCol=n-1;

        //开始『减而治之』的遍历
        while( LeftUpRow<=RightDownRow && LeftUpCol<=RightDownCol )
        {
            for(int loop=LeftUpCol; loop<=RightDownCol; ++loop)
            {
                matrix[LeftUpRow][loop]=curNum;
                ++curNum;
            }
            ++LeftUpRow;//遍历完一行之后，将左上角的点移动位置『减而治之』
             
            if( LeftUpRow>RightDownRow || LeftUpCol>RightDownCol ) break;
            for(int loop=LeftUpRow; loop<=RightDownRow; ++loop)
            {
                matrix[loop][RightDownCol]=curNum;
                ++curNum;
            }
            --RightDownCol;
             
            if( LeftUpRow>RightDownRow || LeftUpCol>RightDownCol ) break;
            for(int loop=RightDownCol; loop>=LeftUpCol; --loop)
            {
                matrix[RightDownRow][loop]=curNum;
                ++curNum;
            }
            --RightDownRow;

            if( LeftUpRow>RightDownRow || LeftUpCol>RightDownCol ) break;
            for(int loop=RightDownRow; loop>=LeftUpRow; --loop)
            {
                matrix[loop][LeftUpCol]=curNum;
                ++curNum;
            }
            ++LeftUpCol;
        }

        return matrix;
    }
};
```





## ✅02.链表

### 2.1.链表的删除

- Ⓜ️[203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        //哑结点
        ListNode * dummy=new ListNode(0x3f3f3f);
        dummy->next=head;
        ListNode * temp=dummy;

        while( nullptr!=temp->next )
        {
            if( val==temp->next->val )
            {
                ListNode * del=temp->next;
                temp->next=del->next;
                delete del;
                continue;
            }
            else 
            {
                temp=temp->next;
            }
        }

        ListNode * del=dummy;
        dummy=dummy->next;
        delete del;
        return dummy;
    }
};
```



- ⭐️[237. 删除链表中的节点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    void deleteNode(ListNode* node) {
        node->val=node->next->val;
        node->next=node->next->next;
        //易容成那样，然后取而代之
        //狸猫换太子
        return ;
    }
};
```



### 2.3.链表的旋转与反转  

#### ✅[61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        
        if(!head || !head->next || k==0) return head;
        ListNode *pos = head;
        int size = 1;
        while(pos && pos->next)
        {
            pos = pos -> next;
            size ++;
        }
        int move = k % size;
        if(move == 0) return head;
        ListNode *cut = head;
        for(int i=0; i<size-move-1; ++i) cut = cut -> next;
        ListNode *result = cut -> next;
        cut -> next = nullptr;
        pos -> next = head;
        return result;

        

    }
};
```



#### ✅[24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {

        ListNode * odd=new ListNode;
        ListNode * even=new ListNode;
        ListNode * one=odd;
        ListNode * two=even;

        int tag=1;
        while( nullptr!=head )
        {
            if( tag&1 )
            {
                one->next=head;
                one=one->next;
            }
            else
            {
                two->next=head;
                two=two->next;
            }
            ++tag;
            head=head->next;
        }

        one->next=nullptr;
        two->next=nullptr;

        ListNode * ret=new ListNode;
        ListNode * temp=ret;
        odd=odd->next;
        even=even->next;

        while( nullptr!=odd && nullptr!=even )
        {
            ListNode * evenTemp=even;
            even=even->next;
            temp->next=evenTemp;
            temp=temp->next;

            ListNode * oddTemp=odd;
            odd=odd->next;
            temp->next=oddTemp;
            temp=temp->next;

            
        }

        if( nullptr!=odd )
        {
            temp->next=odd;
        }
        
        if( nullptr!=even )
        {
            temp->next=even;
        }

        return ret->next;


    }   
};
```





#### Ⓜ️[206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode * sentry=nullptr;	//哨兵
        while( nullptr!=head )
        {
            ListNode * temp=head->next;
            head->next=sentry;
            sentry=head;
            head=temp;
        }
        return sentry;
    }
};
```





#### Ⓜ️[92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        if( left<1 || right<1 || left>right )
        {
            throw "errorInput";
        }
        //-----------------------主题部分----------------------------
        if( nullptr==head || nullptr==head->next )
        {
            return head;
        }
        ListNode * dummy=new ListNode( 0x3f3f3f );
        dummy->next=head;
        
        //表示已经到了第1个节点
        ListNode * pre=dummy;
        ListNode * cur=head;
        int beginLoop=left-1;
        int endLoop=right;
        while( beginLoop-- )
        {
            cur=cur->next;
            pre=pre->next;
            --endLoop;
        }
        
        ListNode * sentry=nullptr;
        while( endLoop-- )
        {
            ListNode * temp=cur->next;
            cur->next=sentry;
            sentry=cur;
            cur=temp;
        }
        pre->next->next=cur;
        pre->next=sentry;
        //-----------------------主题部分----------------------------
        ListNode * del=dummy;
        dummy=dummy->next;
        delete del;
        return dummy;
    }
};
```





#### [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)







### 2.4.链表高精度加法 

- Ⓜ️[2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {

        //头结点
        ListNode *rt=new ListNode(0);
        

        ListNode * pre=rt;//表示当前节点
        int tag=1;//头结点
        int carry=0;//进位

        while(NULL!=l1&&NULL!=l2)
        {
            if(tag)
            {
                rt->val=((l1->val+l2->val+carry)%10);
                carry=(l1->val+l2->val+carry)/10;
                tag=0;
                l1=l1->next;
                l2=l2->next;
            }
            else
            {
                pre->next=new ListNode(((l1->val+l2->val+carry)%10));
                carry=(l1->val+l2->val+carry)/10;
                pre=pre->next;

                l1=l1->next;
                l2=l2->next;
            }
        }

        if(NULL==l1&&NULL!=l2)
        {
            while(NULL!=l2)
            {
                pre->next=new ListNode((l2->val+carry)%10);
                carry=(l2->val+carry)/10;
                l2=l2->next;
                pre=pre->next;
            }
        }
        

        if(NULL!=l1&&NULL==l2)
        {
            while(NULL!=l1)
            {
                pre->next=new ListNode((l1->val+carry)%10);
                carry=(l1->val+carry)/10;
                l1=l1->next;
                pre=pre->next;
            }
        }

        if(0!=carry)
        {
            pre->next=new ListNode(carry);
        }

        return rt;

    }
};
```





- Ⓜ️[445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

ListNode * reverseList( ListNode * root )
{
    ListNode * sentry=nullptr;
    while( nullptr!=root )
    {
        ListNode * temp=root->next;
        root->next=sentry;
        sentry=root;
        root=temp;
    }

    return sentry;
}

class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        if( nullptr==l1 )
        {
            return l2;
        }

        if( nullptr==l2 )
        {
            return l1;
        }

        l1=reverseList( l1 );
        l2=reverseList( l2 );
        
        ListNode * sentry=new ListNode(-1);
        ListNode * ret=sentry;

        int carry=0;
        ListNode * curOne=l1;
        ListNode * curTwo=l2;
        while( nullptr!=curOne && nullptr!=curTwo )
        {
            int temp=curOne->val  + curTwo->val + carry;
            carry=temp/10;
            temp%=10;

            sentry->next=new ListNode( temp );
            sentry=sentry->next;

            curOne=curOne->next;
            curTwo=curTwo->next;
        }

        while( nullptr!=curOne )
        {
            int temp=curOne->val  + carry;
            carry=temp/10;
            temp%=10;

            sentry->next=new ListNode( temp );
            sentry=sentry->next;

            curOne=curOne->next;
        }

        while( nullptr!=curTwo )
        {
            int temp= curTwo->val + carry;
            carry=temp/10;
            temp%=10;

            sentry->next=new ListNode( temp );
            sentry=sentry->next;

            curTwo=curTwo->next;
        }
        
        if( carry )
        {
            sentry->next=new ListNode( carry );
            sentry=sentry->next;
        }

        ListNode * del=ret;
        ret=ret->next;
        delete del;
        
        return reverseList( ret );
    }
};
```

### 2.5.链表的合并 

- Ⓜ️[21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        
        if(nullptr==l1)
        {
            return l2;
        }

        if(nullptr==l2)
        {
            return l1;
        }

        ListNode * rt;
        if(l1->val < l2->val)
        {
            rt=l1;
            l1=l1->next;
        }
        else
        {
            rt=l2;
            l2=l2->next;
        }

        //当前指针
        ListNode * p=rt;

        while(nullptr!=l1 && nullptr!=l2)
        {
            if(l1->val < l2->val)
            {
                p->next=l1;
                p=p->next;

                l1=l1->next;
            }
            else
            {
                p->next=l2;
                p=p->next;
                
                l2=l2->next;
            }
        }

        if(nullptr==l1)
        {
            p->next=l2;
        }
        else
        {
            p->next=l1;
        }

        return rt;
    }
};
```



- Ⓜ️[23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

ListNode * mymege( ListNode * a , ListNode * b )
{
    if( nullptr==a )
    {
        return b;
    }

    if( nullptr==b )
    {
        return a;
    }

    ListNode * rt;
    if( (a->val)<(b->val) )
    {
        rt=a;
        a=a->next;
        
    }
    else
    {
        rt=b;
        b=b->next;
       
    }


    ListNode * temp=rt;
    while( nullptr!=a && nullptr!=b )
    {
        if( (a->val)<(b->val) )
        {
            temp->next=a;
            temp=temp->next;

            a=a->next;
            
        }
        else
        {
            temp->next=b;
            temp=temp->next;

            b=b->next;
            
        }

    }

    while( nullptr!=a )
    {
        temp->next=a;
        temp=temp->next;

        a=a->next;
        
    }

    while( nullptr!=b )
    {
        temp->next=b;
        temp=temp->next;

        b=b->next;
        
    }

    temp->next=nullptr;

    return rt;
}


class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        
        int len=lists.size();
        if( 0==len )
        {
            return nullptr;
        }

        queue<ListNode *> temp;
        
        while(len-- )
        {
            temp.push( lists[len] );
        }

        while( 1!=temp.size() )
        {
            ListNode *a=temp.front();
            temp.pop();
            ListNode *b=temp.front();
            temp.pop();

            ListNode * c=mymege( a , b );
            temp.push(c);
        }

        return temp.front();

    }
};
```







## ✅03.字符串



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







### 2.6.字符的统计

#### 💦[387. 字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        

        int len=s.size();
        if( len<=0 )
        {
            return -1;
        }

        int ret=-1;
        unordered_map<char,int> mp;
        for(char  c: s )
        {
            mp[c]++;
        }

        for( int i=0; i<len; ++i )
        {
            if( 1==mp[ s[i] ] )
            {
                ret=i;
                break;
            }
        }

        return ret;
    }
};
```





#### 💦[389. 找不同](https://leetcode-cn.com/problems/find-the-difference/)

```cpp
class Solution {
public:
    char findTheDifference(string s, string t) {
        int hashFirst[26]={0};
        int hashSecond[26]={0};
        for( auto c : s )
        {
            hashFirst[ c-'a' ]++;
        }

        for( auto c : t )
        {
            hashSecond[ c-'a' ]++;
        }

        for(int i=0; i<26; ++i)
        {
            if( hashSecond[i]>hashFirst[i] )
            {
                return i+'a';
            }
        }

        //给编译器吃的
        return '-';
    }
};
```



#### 💦[383. 赎金信](https://leetcode-cn.com/problems/ransom-note/)

```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int hashFirst[26]={0};
        int hashSecond[26]={0};
        for( auto c : ransomNote )
        {
            hashFirst[c-'a']++;
        }

        for( auto c : magazine )
        {
            hashSecond[c-'a']++;
        }

        for(int i=0; i<26; ++i)
        {
            if( hashFirst[i]>hashSecond[i] ) 
            {
                return false;
            }
        }
        return true;
    }
};
```



#### 💦[242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        int Ahash[256]={0};
        int Bhash[256]={0};

        for( char c : s)
        {
            Ahash[c]++;
        }

        for( char c: t)
        {
            Bhash[c]++;
        }

        int loop=256;
        while( loop-- )
        {
            if( Ahash[loop]!= Bhash[loop] )
            {
                return false;
            }
        }


        return true;
    }
};
```





### 2.9.高精度运算

#### Ⓜ️[66. 加一](https://leetcode-cn.com/problems/plus-one/)

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {

        vector<int> ret;
        reverse( digits.begin(), digits.end() );
        int carry=1;
        for( auto num : digits )
        {
            int CurNum=carry+num;
            carry=CurNum/10;
            CurNum=CurNum%10;
            ret.push_back( CurNum );
        }

        if( 0!=carry )
        {
            ret.push_back( carry );
        }

        reverse( ret.begin(), ret.end() );

        return ret;
    }
};
```



#### Ⓜ️[67. 二进制求和](https://leetcode-cn.com/problems/add-binary/)

```cpp
class Solution {
public:
    string addBinary(string a, string b) {
        reverse( a.begin(), a.end() );
        reverse( b.begin(), b.end() );

        vector<int> solve;
        int pos=0;
        int carry=0;//进位
        while( pos<a.size() && pos<b.size() )
        {
            int num=( a[pos]-'0' )+ ( b[pos]-'0' )+carry;
            carry=num/2;
            solve.push_back( num%2 );

            ++pos;
        }

        while( pos<a.size() )
        {
            int num=( a[pos]-'0' )+carry;
            carry=num/2;
            solve.push_back( num%2 );

            ++pos;
        }

        while( pos<b.size() )
        {
            int num=( b[pos]-'0' )+carry;
            carry=num/2;
            solve.push_back( num%2 );

            ++pos;
        }

        if( carry )
        {
            solve.push_back( 1 );
        }

        reverse( solve.begin(), solve.end() );
        string res;
        for( auto num: solve )
        {
            res+=(num+'0');
        }
        return res;
        
    }
};
```



#### Ⓜ️[415. 字符串相加](https://leetcode-cn.com/problems/add-strings/)

```cpp
class Solution {
public:
    string addStrings(string num1, string num2) {

        if( 0==num1.size() )
        {
            return num2;
        }

        if( 0==num2.size() )
        {
            return num1;
        }
        
        string res;
        reverse( num1.begin(), num1.end() );
        reverse( num2.begin(), num2.end() );

        int pos=0;
        int carry=0;
        while( pos<num1.size() && pos<num2.size() )
        {
            int temp=( num1[ pos ]-'0' )+ ( num2[pos]-'0' )+carry;
            carry=temp/10;
            temp%=10;
            res+=( temp+'0' );

            ++pos;
        }

        while( pos<num1.size() )
        {
            int temp=( num1[ pos ]-'0' )+carry;
            carry=temp/10;
            temp%=10;
            res+=( temp+'0' );

            ++pos;
        }
        
        while( pos<num2.size() )
        {
            int temp=( num2[ pos ]-'0' )+carry;
            carry=temp/10;
            temp%=10;
            res+=( temp+'0' );
            
            ++pos;
        }

        if( carry )
        {
            res+='1';
        }
        
        reverse( res.begin(), res.end() );
        return res;
    }
};
```





#### Ⓜ️[43. 字符串相乘](https://leetcode-cn.com/problems/multiply-strings/)





## ✅05.双指针法

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







## ✅15.常用技巧与算法



## 15.1.博弈论



#### Ⓜ️[292. Nim 游戏](https://leetcode-cn.com/problems/nim-game/)

```cpp
class Solution {
public:
    bool canWinNim(int n) {
            if(0==n%4)
            {
                return false;
            }
            else
            {
                //4x+1,4x+2,4x+3都可以，反正我的后面几次和他弄成4
                return true;
            }
    }
};
```



