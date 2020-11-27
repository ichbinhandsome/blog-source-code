---
title: 【leetcode】Implement Trie (Prefix Tree)
date: 2020-08-06 18:25:16
tags: leetcode
categories: Data structure & Algorithm
---

### Problem:

Implement a trie with `insert`, `search`, and `startsWith` methods.

*Example:*

```java
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

### Methods & Solutions:

<!-- more -->

* **Frist method using `TrieNode`**

1. First define a `TrieNode` class, it has two attributes, one is `children`, the other is `isWord`. Attribute `children`  represents  the sub nodes of  parent node.  Since one node may have many different sub nodes, I set  `children`   into `dict()` or `collections.defaultdict()`.  Attribute `isWord` means weather this character is the end of inserted word, it will be used in  `trie.serach()`. we initialize `isWord = False` , then if the character is the end of word, we set it into `True`.    

   ```python
   class TrieNode:
       
       def __init__(self):
           self.children  = {} # store characters of a word according ro their indices
           # self.children  = collections.defaultdict(TrieNode)
           self.isWord = False
   ```

2. Define `__init__` in class `Trie`

   ```python
   class Trie:
   	def __init__(self):
       	"""
       	Initialize your data structure here.
       	"""
       	self.root = TrieNode() # root node is an empty node.
   ```

3. Define `insert` in class `Trie`

   ```python
   def insert(self, word: str) -> None:
           """
           Inserts a word into the trie.
           """
           node  = self.root # begin from root node in Trie
           
           for w in word:
               if w not in node.children:
                   node.children[w] = TrieNode() # store character in node.children
               node  = node.children[w] # update current node into its children node
               
           node.isWord = True # this node represent the last character of word
   ```

4. Define `search` in class `Trie`

   ```python
   def search(self, word: str) -> bool:
           """
           Returns if the word is in the trie.
           """
           node  = self.root # begin from root node, which is empty
           for w in word:
               # if character w not in inserted word, return False
               if w not in node.children: return False 
               node = node.children[w] # update current node
           return node.isWord # judge if it is the end character
   ```

5. Define `startsWith` in class `Trie`

   ```python
   def startsWith(self, prefix: str) -> bool:
           """
           Returns if there is any word in the trie that starts with the given prefix.
           """
           node = self.root # begin from root node, which is empty
           
           for w in prefix:
               if w not in node.children: return False
               node = node.children[w]
           return True
   ```

* **Second method using `dict()` from python**

Instead of creating new `TrieNode` class, we just use `dict` here. Each `dict` represents a node.

```python
class Trie:
    # use dict to implement Trie

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = {} # root node

    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        p = self.root # begin from root node
        for w in word:
            if w not in p:
                p[w] = {} # add new node into current node
            p = p[w] # update current node
            
        # set '#' as the end signal, add into current node
        p['#'] = True 

    def find(self, prefix): # helper fuction to judge if the prefix in Trie
        p = self.root
        for w in prefix:
            if w not in p: return None
            p = p[w]
        return p
           
    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        return self.find(word) is not None and '#' in self.find(word)  

    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        return self.find(prefix) is not None
```

