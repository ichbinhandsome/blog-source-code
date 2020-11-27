---
title: 【leetcode】Find All Duplicates in an Array
date: 2020-08-07 00:35:26
tags: leetcode
categories: Data structure & Algorithm
---

### Problem:

Given an array of integers, 1 ≤ a[i] ≤ *n* (*n* = size of array), some elements appear **twice** and others appear **once**.

Find all the elements that appear **twice** in this array.

Could you do it without extra space and in O(*n*) runtime?

*Example:*

```python
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]
```

### Solutions:

This is a very confusing question, since we must solve it in  O(*n*) runtime without extra space. Obviously, the easiest way is to use `hash set`, which needs extra space. Here we will construct two different tricky hash functions within the original array, which will  simulate the process of `hash set`.

<!-- more -->

* negative-positive hash function

  ```
  Traverse the array. Do following for every index i of A[].
  {
  // Since 1 ≤ a[i] ≤ n (n = size of array), so we must use abs(A[i])-1 as target index for element A[i], otherwise, list index out of range
  check for sign of A[abs(A[i])-1] ;
  if positive then
     make it negative by   A[abs(A[i])-1]= -A[abs(A[i])-1];
  else  // i.e., A[abs(A[i])-1] is negative
     this   element (ith element of list) is a repetition
  }
  ```

  ```
  Example: A[] =  {1, 1, 2, 3, 2}
  i=0; 
  Check sign of A[abs(A[0])-1] which is A[0].  A[0] is positive, so make it negative. 
  Array now becomes {-1, 1, 2, 3, 2}
  
  i=1; 
  Check sign of A[abs(A[1])-1] which is A[0].  A[0] is negative, so A[1] is a repetition.
  
  i=2; 
  Check sign of A[abs(A[2])-1] which is A[1].  A[1] is  positive, so make it negative. '
  Array now becomes {1, -1, 2, 3, 2}
  
  i=3; 
  Check sign of A[abs(A[3])-1] which is A[2].  A[2] is  positive, so make it negative. 
  Array now becomes {1, -1, -2, 3, 2}
  
  i=4; 
  Check sign of A[abs(A[4])-1] which is A[1].  A[1] is negative, so A[4] is a repetition.
  ```

  ```python
  class Solution:
      def findDuplicates(self, nums: List[int]) -> List[int]:
          res = []
          for x in nums:
               if nums[abs(x)-1] < 0:
                   res.append(abs(x))
               else:
                   nums[abs(x)-1] *= -1
           return res
  ```

<br>

* add-length hash function

  This idea of this method is similar to the first one, which also construct a hash function within the array. Instead of making target index for element A[i] negative, we add the `len(A)` into it,  i.e. `A[A[i] % len(A)] += len(A)`. If one element occurs twice, so `A[A[i] % len(A)] > 2*len(A)`. We use two loops, the first loop we add `len(A)` to the corresponding target of `A[i]`, the second loop we will traverse the modified `A`, and find all elements satisfied `A[A[i] % len(A)] > 2*len(A)` as result.

  ```python
  class Solution:
      def findDuplicates(self, nums: List[int]) -> List[int]:
          res = []
          for i in range(len(nums)):
              nums[(nums[i]-1) % len(nums)] += len(nums)
          for i in range(len(nums)):
              if nums[i] > 2*len(nums):
                  res.append(i+1)
          return res
  ```

  

  