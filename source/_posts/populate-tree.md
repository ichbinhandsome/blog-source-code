---
title: 【leetcode】Populating Next Right Pointers in Each Node
date: 2020-08-25 18:50:33
tags: leetcode
categories: Data structure & Algorithm
---

### Problem:

You are given a **perfect binary tree** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

 <!-- more -->

**Follow up:**

- You may only use constant extra space.
- Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

```
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

### Methods:

* **first method**

  使用递归， 前序遍历。 

  先将同一个节点下的左子节点`next` 指向右子节点：`root.left.next = root.right`

  再类似于链表操作，因为`node.next`初始全为`NULL`, 而且在前一次的递归中`node.next`已经被赋值，如果`node.next`仍然为空，代表是这一层的最末尾节点，不需要进行操作。 否则对节点的右子节点赋值：`root.right.next = root.next.left`。 

  之所以使用前序遍历，是因为要先对`root.next`赋值，之后才能对`root.left.next` 和`root.right.next`赋值，`root.right.next`的赋值依赖于`root.next`。

  ```python
  """
  # Definition for a Node.
  class Node:
      def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
          self.val = val
          self.left = left
          self.right = right
          self.next = next
  """
  
  class Solution:
      def connect(self, root: 'Node') -> 'Node':
          if not root or not root.left: return root
          root.left.next = root.right
          if root.next:
              root.right.next = root.next.left
          self.connect(root.left)
          self.connect(root.right)
          return root
          
  ```

* **second method**

  迭代法，链表操作，每次从每层最左边的节点开始遍历。

  ```python
  """
  # Definition for a Node.
  class Node:
      def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
          self.val = val
          self.left = left
          self.right = right
          self.next = next
  """
  
  class Solution:
      def connect(self, root: 'Node') -> 'Node':
          if not root: return
          leftmost = root #最左边的node
          while leftmost.left:
              curr = leftmost #当前node
              while curr:
                  curr.left.next = curr.right
                  if curr.next: curr.right.next = curr.next.left
                  curr = curr.next
              leftmost = leftmost.left
          return root
  ```

  

  