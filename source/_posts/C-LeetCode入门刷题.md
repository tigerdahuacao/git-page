---
title: C++LeetCode入门刷题
date: 2022-08-30 22:09:01
categories:
- 教程
tags: 
- C++
- 刷题
---

本文多次施工

---

## 两整数相加 (2235 题)

```cpp
/*
 * @lc app=leetcode.cn id=2235 lang=cpp
 *
 * [2235] 两整数相加
 */

// @lc code=start
class Solution {
public:
    int sum(int num1, int num2) {
        return num1+num2;
    }
};
// @lc code=end



```

---

## x 的平方根 (69 题)

```cpp
/*
 * @lc app=leetcode.cn id=69 lang=cpp
 *
 * [69] x 的平方根
 */

// @lc code=start
#include <cmath>

class Solution {
public:
    int mySqrt(int x) {
        return (int)sqrt(x);
    }
};
// @lc code=end


```

---

## 有效的完全平方数 (367 题)

```cpp
/*
 * @lc app=leetcode.cn id=367 lang=cpp
 *
 * [367] 有效的完全平方数
 */

// @lc code=start
class Solution {
public:
    bool isPerfectSquare(int num) {
        int ans = 0;
        for (int i = 0; i <=num; i++)
        {
            if((long long)i*i <=num){
                ans = i;
            }else{
                break;
            }
        }
        return (long long)ans * ans == num;

    }
};
// @lc code=end

```

---

## x 的 n 次幂 (50 题)

```cpp
/*
 * @lc app=leetcode.cn id=50 lang=cpp
 *
 * [50] Pow(x, n)
 */

// @lc code=start
#include <cmath>
class Solution {
public:
    double myPow(double x, int n) {
        return pow(x,n);
    }
};
// @lc code=end

```

---

## 2 的幂 (231 题)

```cpp
/*
 * @lc app=leetcode.cn id=231 lang=cpp
 *
 * [231] 2 的幂
 */

// @lc code=start
class Solution
{
public:
    bool isPowerOfTwo(int n)
    {
        if (n <= 0)
            return false;
        if (n == 1)
            return true;
        long long ans = 1;
        while (1)
        {
            ans *= 2;
            if (ans == n)
                return true;
            else if (ans > n)
            {
                return false;
            }
        }
        return false;
    }
};
// @lc code=end

```

---

## 3 的幂 (326 题)

```cpp
/*
 * @lc app=leetcode.cn id=326 lang=cpp
 *
 * [326] 3 的幂
 */

// @lc code=start
class Solution
{
public:
    bool isPowerOfThree(int n)
    {
        {
            if (n <= 0)
                return false;
            if (n == 1)
                return true;
            long long ans = 1;
            while (1)
            {
                ans *= 3;
                if (ans == n)
                    return true;
                else if (ans > n)
                {
                    return false;
                }
            }
            return false;
        }
    }
};
// @lc code=end


```

---

## 反转两次的数字 (2119 题)

```cpp
/*
 * @lc app=leetcode.cn id=2119 lang=cpp
 *
 * [2119] 反转两次的数字
 */

// @lc code=start
class Solution
{
public:
    bool isSameAfterReversals(int num)
    {
        if (num == 0)
            return true;
        if (num % 10 == 0)
            return false;

        return true;
    }

};
// @lc code=end
```

---

## 左旋转字符串

> https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/

这道题其实就是先反转全部, 再反转部分

```cpp

class Solution {
public:
    string reverseLeftWords(string s, int n) {
         int whole_len = s.size();
        s = myReserve(s, 0, whole_len - 1);
        cout << s << "\r\n\r\n";
        s= myReserve(s,0,whole_len - n -1);
        cout << s << "\r\n\r\n";
        s= myReserve(s,whole_len - n,whole_len - 1);
        cout << s << "\r\n\r\n";
        return s;
    }

    string myReserve(string s, int start, int end)
    {
        int mid = (start + end)/2 - start;
        cout << "input:" << s <<"\r\n";
        cout << "mid:" <<mid<<"\r\n";
         for (int i = 0; i <= mid; i++)
        {
            cout << s[start+i];
        }
        cout << "\r\n";
        for (int i = 0; i <= mid; i++)
        {
            char temp = s[start + i];
            s[start + i] = s[end - i];
            s[end - i] = temp;
        }
        cout << "  " << s << "\r\n";
        return s;
    }
};

```

不过有一个更好的解法:

```cpp

class Solution{
    public:
        string reverseLeftWords(string s, int k){
            int n = s.size();
            string ret = s;
            for(int i = 0; i < n; i++){
                ret[i] = s[(i+k)%n];
            }
            return ret;
        }
}

```

---

## 最大数值

> https://leetcode.cn/problems/maximum-lcci/

```cpp
class Solution {
public:
    int maximum(int a, int b) {
        return max(a,b);
    }
};
```

---

## 判断国际象棋棋盘中一个格子的颜色 (1812 题)

```cpp
/*
 * @lc app=leetcode.cn id=1812 lang=cpp
 *
 * [1812] 判断国际象棋棋盘中一个格子的颜色
 */

// @lc code=start
#include <string>
using namespace std;
class Solution
{
public:
    bool squareIsWhite(string coordinates)
    {
        int x = coordinates[0] - 'a';
        int y = coordinates[1] - '1';
        return (x + y) % 2;
    }
};
// @lc code=end
```

---

## 只出现一次的数字 (136 题)

```cpp
/*
 * @lc app=leetcode.cn id=136 lang=cpp
 *
 * [136] 只出现一次的数字
 */

// @lc code=start
using namespace std;
#include <vector>

class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (int i = 0; i < nums.size(); i++)
        {
            ans = ans ^ nums[i];
        }
        return ans;

    }
};
// @lc code=end

```

---

## 丢失的数字 (268 题)

```cpp
/*
 * @lc app=leetcode.cn id=268 lang=cpp
 *
 * [268] 丢失的数字
 */

// @lc code=start
using namespace std;
#include <vector>

class Solution
{
public:
    int missingNumber(vector<int> &nums)
    {
        int n = nums.size();
        int sum = (0 + n) * (n + 1) / 2;
        for (int i = 0; i < n; i++)
        {
            sum -= nums[i];
        }
        return sum;
    }
};
// @lc code=end

```

---

## 数组异或操作 (1486 题)

```cpp
/*
 * @lc app=leetcode.cn id=1486 lang=cpp
 *
 * [1486] 数组异或操作
 */

// @lc code=start
class Solution {
public:
    int xorOperation(int n, int start) {
        int ans = 0;
        for (int i = 0; i < n; i++)
        {
            int x = start + 2*i;
            ans = ans ^ x;
        }
        return ans;

    }
};
// @lc code=end
```

---

## 十进制整数的反码 (1009 题)

这道题的图解是这样的

```
比如是数字11, 它的二进制码是 1011
它的反码就是 0100
这里的特点是 原码和反码的和是 1111;
所以找到能表示这个十进制数的最小的全是1的数就可以(比如这个5就是4位1)

所以有下方代码的
if((1<<k) > n){
    break;
}


```

```cpp
/*
 * @lc app=leetcode.cn id=1009 lang=cpp
 *
 * [1009] 十进制整数的反码
 */

// @lc code=start
class Solution {
public:
    int bitwiseComplement(int n) {
        if(n ==0){
            return 1;
        }
        int k = 0;
        while(true){
            if((1<<k) > n){
                break;
            }
            k++;
        }
        return (1<<k) - 1 -n;
    }
};
// @lc code=end


```

---

## 数字的补数 (476 题)

和 13 题是一样的,但是会有一个溢出的问题, 需要转一下

```cpp

/*
 * @lc app=leetcode.cn id=476 lang=cpp
 *
 * [476] 数字的补数
 */

// @lc code=start
class Solution
{
public:
    int findComplement(int num)
    {
        if (num == 0)
        {
            return 1;
        }
        int k = 0;
        while (true)
        {
            if ((long long )(1 << k) > num)
            {
                break;
            }
            k++;
        }
        return (long long )(1 << k) - 1 - num;
    }
};
// @lc code=end
```

---

## 二进制中 1 的个数

> https://leetcode.cn/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/

```cpp
/*
    01001010111100
&   00000000000001
*/
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int sum = 0;
        while(n){
            if(n & 1){
                sum ++;
            }
            n >>=1;
        }
        return sum;
    }
};
```

另一种做法

```cpp
/*
  n = n & (n-1);的意思
  n - 1会找到最末尾的1

step1:
   n  :  01001010111100
&  n-1:  01001010111011

找到了倒数第三位的1, sum++

step2:
   n  :  01001010111000
&  n-1:  01001010110111

找到了倒数第4位的1, sum++

step3:
   n  :  01001010110000
&  n-1:  01001010101111

找到了倒数第5位的1, sum++

以此类推
*/
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int sum = 0;
        while(n){
            n = n & (n-1);
            sum++;
        }
        return sum;
    }
};
```

---

## 位 1 的个数 (191 题)

```cpp
/*
 * @lc app=leetcode.cn id=191 lang=cpp
 *
 * [191] 位1的个数
 */

// @lc code=start

#include <stdint.h>
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int sum = 0;
        while(n){
            n = n & (n-1);
            sum++;
        }
        return sum;
    }
};
// @lc code=end


```

---

## 汉明距离 (461 题)

2 个数作异或运算, 得到不同的位数(不同为 1,相同为 0), 然后再统计位数即可

```cpp
/*
 * @lc app=leetcode.cn id=461 lang=cpp
 *
 * [461] 汉明距离
 */

// @lc code=start
class Solution
{
    int get1Count(int x)
    {
        int count = 0;
        while (x)
        {
            x = x & (x - 1);
            count ++;
        }
        return count;
    }

public:
    int hammingDistance(int x, int y)
    {
        return get1Count(x ^ y);
    }
};
// @lc code=end
```

---

## 转换数字的最少位翻转次数 (2220 题)

和汉明距离(461 题)是一样的, 因为不一样的位才需要反转, 到头来还是计算有多少位是不同的

---

## 将数字变成 0 的操作次数(1342 题)

```cpp
/*
 * @lc app=leetcode.cn id=1342 lang=cpp
 *
 * [1342] 将数字变成 0 的操作次数
 */

// @lc code=start
class Solution
{
public:
    int numberOfSteps(int num)
    {
        int step = 0;
        while (num)
        {
            if (! (num % 2))
            {
                //是偶数
                num = num / 2;
            }
            else
            {
                //是奇数
                num = num - 1;
            }
            step ++;
        }
        return step;
    }
};
// @lc code=end

```

另外, 也可以用动态规划做:

```cpp
class Solution{
    public: int numberOfSteps(int num){
        int f[num+1];
        f[0] = 0;
        for(int i =1; i<= num; i++){
            if(i&1){
                //奇数
                f[i] = f[i-1] + 1;
            }else{
                //偶数
                f[i] = f[i/2] + 1;
            }
        }
        return f[num];
    }
}
```

---
##  n 的第 k 个因子 (1492题)

```cpp
/*
 * @lc app=leetcode.cn id=1492 lang=cpp
 *
 * [1492] n 的第 k 个因子
 */

// @lc code=start
#include <cmath>
#include <algorithm>
using namespace std;

class Solution {
public:
    int kthFactor(int n, int k) {
        int sn = sqrt(n);
        //因子数组
        int a[1001], top = 0;
        for(int i=1;i<=sn;i++){
            if(n%i ==0){
                //找到一个因子, 存入数组
                a[top++] = i;

                //因为乘法是对应的,而且我们之遍历到sqrt(n), 所以要找到另外一半
                if(n / i != i) {
                    a[top++] = n/i;
                }
            }
        }
        sort(a, a + top);
        if(top < k){
            return -1;
        }
        return a[k-1];
    }
};
// @lc code=end

```

---
##  差的绝对值为 K 的数对数目 (2006题)

```cpp
/*
 * @lc app=leetcode.cn id=2006 lang=cpp
 *
 * [2006] 差的绝对值为 K 的数对数目
 */

// @lc code=start
#include <vector>
#include <cmath>
using namespace std;

class Solution
{
public:
    int countKDifference(vector<int> &nums, int k)
    {
        int ans = 0;
        for (int i = 0; i < nums.size(); i++)
        {
            for (int j = i + 1; j < nums.size(); j++)
            {
                if (std::abs(nums[i] - nums[j]) == k)
                {
                    ans++;
                }
            }
        }
        return ans;
    }
};
// @lc code=end
```

---
## 数组串联 (1929题)
```cpp
/*
 * @lc app=leetcode.cn id=1929 lang=cpp
 *
 * [1929] 数组串联
 */

// @lc code=start
#include <vector>
#include <cmath>
using namespace std;
class Solution {
public:
    vector<int> getConcatenation(vector<int>& nums) {
        int n = nums.size();
        for(int i=0; i<n; i++){
            nums.push_back(nums[i]);
        }
        return nums;
    }
};
// @lc code=end

```

---
## IP 地址无效化 (1108题)
```cpp
/*
 * @lc app=leetcode.cn id=1108 lang=cpp
 *
 * [1108] IP 地址无效化
 */

// @lc code=start
#include <vector>
#include <cmath>
#include <string>
using namespace std;
class Solution {
public:
    string defangIPaddr(string address) {
        string ret;
        for (int i = 0; i < address.size(); i++)
        {
            if(address[i] == '.'){
                ret += "[.]";
            }else{
                ret += address[i];
            }
        }
        return ret;
    }
};
// @lc code=end

```

---

## K 进制表示下的各位数字总和 (1837题)
```cpp
/*
 * @lc app=leetcode.cn id=1837 lang=cpp
 *
 * [1837] K 进制表示下的各位数字总和
 */

// @lc code=start
#include <vector>
#include <cmath>
#include <string>
using namespace std;

class Solution {
public:
    int sumBase(int n, int k) {
        int sum = 0;
        while(n){
            //通过取余的方法, 得到进制转换后的最后一位;
            sum += n % k;
            //以此类推, 求倒数第二位
            n/=k;
        }
        return sum;
    }
};
// @lc code=end

```
