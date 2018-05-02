---
layout: post
title: Linked List Algorithm in JavaScript-Remove n-th node LeetCode Challenge-At The Hacking School-India
comments: true
author: Rohan Paul
categories: JavaScript
---
<img src="/images/fulls/linked-list-remove-n-th-node.jpg" class="fit image">


This is a very popular Algorithmic problem of removing the n-th node from a linked-list faced in a [Leetcode challenge](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/) .

**Here's the Original Challenge** - 
Given a linked list, remove the nth node from the end of list and return its head.

For example,

   Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
Note: Given n will always be valid. Try to do this in one pass.

*************************************************

**Solution Algorithm**

However also have to say, this is definitely not the best solution, and there are indeed more efficient way to tackle this problem.

- **A>** In order to remove a node from a linked list, we need to find the node that is just before the node we want to remove. Once we find that node, we change its next property to no longer reference the removed node. But, the previous node is modified to point to the node after the removed node. So the key line of code will be

	**``prevNode.next = prevNode.next.next``**

	We are just skipping over the node we want to remove and linking the “previous” node with the node just after the one we are removing.


- **B>** The nth node from the end will be (list.length - n + 1)-th node from the begining of the Linked List. 

	 For example: 

	 **1**-> **2** -> **3** -> **4** -> **5** -> **6** -> **7**

	 To return 2nd node from the end(n = 2), we need to return 6th (7 - 2 + 1) node from beginning which is node '6'.

- **C>** So the problem could be simply reduced to another one : Remove the **(L - n + 1)-th** node from the beginning in the list , where L is the list length. 

- **D>** First we will add an auxiliary dummy or fake head node which in the below program I am naming as **"currentNode"**, which points to the list head. The “dummy” node is used to simplify some corner cases such as a list with only one node, or removing the head of the list. The key step is we have to relink next pointer of the **(L - n)-th** node to the **(L - n + 2)-th** node and we are done.

**Here was the implementation of the above algo**

<p data-height="1089" data-theme-id="0" data-slug-hash="VyxOqB" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="remove-nth-node-linked-list-Leetcode-mySolution" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/VyxOqB/">remove-nth-node-linked-list-Leetcode-mySolution</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

And below, just for more practice for me on this topic, a simple generic implementation of a very simple single-linked-list. Where, I have to remove node given the node itself as the passed-in argument to the ``remove`` function.

Here, the design of this linked list involves creating two classes - a Node class for adding nodes to a linked list, and create a LinkedList class, which will provide functions for inserting nodes, removing nodes, displaying a list, and other housekeeping functions within the LinkedList as a whole.


<p data-height="1612" data-theme-id="0" data-slug-hash="EoLBao" data-default-tab="js" data-user="rohanpaul" data-embed-version="2" data-pen-title="generic-linked-list-for-node-removal.js" class="codepen">See the Pen <a href="https://codepen.io/rohanpaul/pen/EoLBao/">generic-linked-list-for-node-removal.js</a> by Rohan Paul (<a href="https://codepen.io/rohanpaul">@rohanpaul</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>


<img src="/images/fulls/THS-Linked-List/Linked-List-1-Hacking-School.jpg" class="fit image">

<img src="/images/fulls/THS-Linked-List/Linked-List-1-Hacking-School-2.jpg" class="fit image">