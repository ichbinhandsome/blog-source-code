---
title: 【leetcode】KMP算法
date: 2020-10-06 18:00:40
tags: leetcode
categories: Data structure & Algorithm
mathjax: true
---

资料链接：[leetcode 文章讲解](https://leetcode-cn.com/problems/implement-strstr/solution/bang-ni-ba-kmpsuan-fa-xue-ge-tong-tou-ming-ming-ba/)

视频链接：[huahua 视频讲解](https://www.youtube.com/watch?v=uKr9qIZMtzw)

*主串*（在主串中寻找匹配串），*匹配串*（需要被匹配的串）

KMP 算法主要目的是将字符串匹配控制在线性时间复杂度 $O(m+n)$ 内， 如果采用暴力算法的话时间复杂度为$O((m-n)*n)$ , 其中 $m$ 为主串的长度，$n$ 为匹配串的长度。

**主要思想**：善于利用匹配串中的信息，避免重复判断和主串上匹配指针回退。

比如匹配串为 **ABCABE**, 当我们再匹配的过程中在 **E** 这个位置上出现不匹配，而之前的 **ABCAB** 都已经匹配成功。如果是暴力算法的我们需要先回退主串中的匹配指针到开始位置（即第一个 **A** 出现的位置），并且在其下一个位置处再从头对 **ABCABE** 进行匹配。但是很显然我们并不需要回退匹配指针和从 **ABCABE** 的头部开始匹配，我们只需要在主串的当前位置处对 **CABE** 再进行匹配即可，这样就利用了匹配串里面的信息，避免了重复操作。KMP 算法就是利用匹配串的这一特性，而且 KMP 最大的一个特点就是主串上的匹配指针从来不走回头路。

<!-- more -->

**方法实现**：

现在问题在于我们如何利用匹配中的信息。 KMP 算法的第一步就是对匹配串进行建表，这个表在通常被称作为`next`表，它存储了匹配串的一些信息，通过这些信息我们能够得知在 **ABCABE** 出现 **E** 位置上不匹配时，我们只需要从 **C** 的位置再开始匹配，而非从头开始。第二步就是在主串中对匹配串匹配，碰到不匹配时，查找`next`表，回退匹配串上的指针，再移动匹配串和主串上的指针进行匹配，而主串上的匹配指针一直都不会回退，当匹配串指针回退到开始位置仍与主串不匹配时，主串上的匹配指针前进。

 * 构造 `next` 表：利用匹配串的前缀和后缀，分别构造对应位置上的前缀子串和后缀子串最长相同的长度（不包括这个位置上的元素）【*之所以不包括此位置上的元素是为了在调用`next`表时比较方便，参见 **ABAE** 的解释*】。比如 **ABAE**，在 **E** 这个位置上对应的长度是 $1$ ，因为对于 **ABA** 来说前缀是 **AB** 后缀是 **BA** ，它两最长的的相同部分为 **A**， 所以长度为 $1$。同理， **B** 位置上就是 $0$。 当 **E** 位置上出现不匹配时，我们查找 `next` 表，发现其对应的值为 $1$, 所以我们移动匹配串上的指针到索引为`1` 的位置上，即 **B** 的位置上，在判断是否匹配，因为 **A** 已经被匹配过了。

   *代码实现*：(相当于自己和自己匹配)

   ```python
   def build(p : str) -> List[int]:
       n = len(p)
       nxt = [0, 0]
       j = 0
       for i in range(1, n):
           while j > 0 and p[i] != p[j]:
               j = nxt[j]
           if p[i] == p[j]:
               j += 1
           nxt.append(j)
       return nxt
   ```

   * 匹配（主串和匹配串）

     *代码实现*：

     ```python
     def match(s: str, p: str) -> int:
         n, m = len(p), len(s)
         nxt = build(p)
         j = 0
         for i in range(m):
             while j > 0 and s[i] != p[j]:
                 j = nxt[j]
             if s[i] == p[j]:
                 j += 1
             if j == n:
                 return i-n+1
         return -1
     ```

   ​			

**Leetcode : [实现 strStr()](https://leetcode-cn.com/leetbook/read/top-interview-questions-easy/xnr003/)**

*Solution*:

````python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        def build(p):
            nxt = [0, 0]
            j = 0
            n = len(p)
            for i in range(1, n):
                while j > 0 and p[i] != p[j]:
                    j = nxt[j]
                if p[i] == p[j]: j += 1
                nxt.append(j)
            return nxt
        if len(needle) == 0: return 0
        nxt = build(needle)
        n, m = len(haystack), len(needle)
        j = 0
        for i in range(n):
            while j > 0 and haystack[i] != needle[j]:
                j = nxt[j]
            if haystack[i] == needle[j]: j += 1
            if j == m: return i-m+1
        return -1

````

````c++
class Solution {
public:
    vector<int> build(string p){
        vector<int> nxt = {0,0};
        int j = 0;
        for (int i = 1; i < p.size(); i++){
            while (j > 0 && p[i] != p[j]){
                j = nxt[j];
            }
            if (p[i] == p[j]){
                ++j;
            }
            nxt.push_back(j);
        }
        return nxt;
    }
    int strStr(string haystack, string needle) {
        if (needle.size() == 0) return 0;
        vector<int> nxt = build(needle);
        int m = haystack.size(), n = needle.size();
        int j = 0;
        for (int i = 0; i < m; i++){
            while (j > 0 && haystack[i] != needle[j]){
                j = nxt[j];
            }
            if (haystack[i] == needle[j]) ++j;
            if (j == n) return i - n + 1;
        }
        return -1;
    }
};
````





  