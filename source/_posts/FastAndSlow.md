---
title: 【leetcode】快慢指针
date: 2020-11-06 16:03:51
tags: leetcode
categories: Data structure & Algorithm
mathjax: true
---

### 什么是快慢指针？

快慢指针是遍历操作时的一种技巧，通常是由一个快指针和一个慢指针组成，一般应用在链表操作上。以下示例是快指针走两步，慢指针走一步：

``` python
fast, slow = head # head是链表的头节点（单链表且无环）
while fast and fast.next:
    fast = fast.next.next
    slow = slow.next
#循环终止时slow指针指向链表的中间节点
```

<!-- more -->

### 应用场景

* [判断链表是否有环](https://leetcode-cn.com/problems/linked-list-cycle/)：

  此时需要快慢指针来判断，当快慢两个指针都进入环内时，可以想象成两个人在操场跑圈，一个人的速度是另外一个人的两倍，跑了若干圈后，快的人总会在某个时间点和慢的人相遇，只要有速度差就会相遇，除非两个人速度一致都保持相对静止。所以当链表存在环时，快慢指针同时从起点出发，总会在某个时刻相遇。

  ```python
  # Definition for singly-linked list.
  # class ListNode:
  #     def __init__(self, x):
  #         self.val = x
  #         self.next = None
  
  class Solution:
      def hasCycle(self, head: ListNode) -> bool:
          fast, slow = head, head
          while fast and fast.next:
              fast = fast.next.next
              slow = slow.next
              if fast == slow:
                  return True
          return False
  ```

* [求有环链表环的起点](https://leetcode-cn.com/problems/linked-list-cycle-ii/)：

  这个思路也是利用快慢指针，不过稍微复杂一点，需要我们在这里简单做一个数学证明。假设链表在某一点有环，此时从head到这个节点的距离为 $a$ ，环的周长设为 $b$，我们知道当快慢指针都进入环内进行循环跑动时，总会在某一点相遇，那么在相遇时快指针一定比慢指针多跑了 $n$ 圈（$n$ 为整数），多跑的长度为 $nb$。 令$f$ 为快指针跑过的距离， $s$ 为慢指针跑过的距离，因为 $f = 2s$ 恒成立且 $f-s=nb$ 推出 $s=nb$。注意，此时 $f$ 和 $s$ 都不一定处于环的入口洁结点处，因为要想到达环的起始结点处还必须得跑 $a$ 的距离， 使得 $s=nb+a$, 这样才符合慢指针从链表起点出发，先跑过 $a$ 进入环内，再跑了 $n$ 圈后又回到环的入口结点的条件。所以我们需要再提前保留一个指向链表头节点的指针，在快慢指针已经相遇的条件下，移动它，慢指针也跟着一起走，当它走过 $a$ 的距离来到环的入口结点时，慢指针走过的距离也变成了 $nb+a$, 也来到了环的入口结点处，两个指针相遇，此时指向的结点就是环的入口结点。

  ```python
  # Definition for singly-linked list.
  # class ListNode:
  #     def __init__(self, x):
  #         self.val = x
  #         self.next = None
  
  class Solution:
      def detectCycle(self, head: ListNode) -> ListNode:
          #环形链表的经典解法，快慢指针
          #数学推导，当快慢指针在环内相遇时
          if not head: return head
          fast, slow, temp = head, head, head
          while fast and fast.next:
              fast = fast.next.next
              slow = slow.next
              if fast == slow:
                  while temp != slow:
                      temp = temp.next
                      slow = slow.next
                  return slow
          return None
  ```

* [相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

  这道题的思路也较为奇特，虽然只是一道简单题，但是没有发散思维或者链表成环意识的话一时半会也很难想到。A 链表和 B 链表相交于某一点或者不相交，如果相交的话可以认为一个链表前半部分分叉成 A 和 B 两部分。这时候需要双指针 指针A 和 指针B，而非快慢指针。指针A 从 A 的头结点开始出发，当走到末尾时就指向 B 的头节点，指针B从B的头节点开始出发，当走到末尾时指向 A 的头节点。原理：假设 A和B 相交前的长度分别为 $a$ 和 $b$，相交后的长度为 $c$， 当指针A走过 $a+c+b$ 的距离时第二次回到了相交点， 此时指针B 走过的距离为 $b+c+a$（指针A和指针B同速），所以也到达了相交点，指针A 和指针B 相遇。

  ```python
   Definition for singly-linked list.
  # class ListNode:
  #     def __init__(self, x):
  #         self.val = x
  #         self.next = None
  
  class Solution:
      def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
          #指针转移，走完A再走B，走完B再走A
          if not headA or not headB: return None
          curr_A, curr_B = headA, headB
          #巧妙解法，避开好多限制条件
          #如果A和B相交，则一定会有一个相交节点，此时curr_A 等于 curr_B
          #如果这两个不相交，则 curr_A == curr_B == None 此时也退出循环
          while curr_A != curr_B:
              if curr_A:
                  curr_A = curr_A.next
              else:
                  curr_A = headB
              if curr_B:
                  curr_B = curr_B.next
              else:
                  curr_B = headA
          return curr_A
  ```

* [删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

  双指针，一个领先另一个N个节点，比较直观

  ```python
  # Definition for singly-linked list.
  # class ListNode:
  #     def __init__(self, val=0, next=None):
  #         self.val = val
  #         self.next = next
  class Solution:
      def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
          if not head: return 
          fast, slow = head, head
          for i in range(n):
              fast = fast.next
          if not fast: return head.next
          while fast and fast.next:
              fast = fast.next
              slow = slow.next
          slow.next = slow.next.next
          return head
  ```

  