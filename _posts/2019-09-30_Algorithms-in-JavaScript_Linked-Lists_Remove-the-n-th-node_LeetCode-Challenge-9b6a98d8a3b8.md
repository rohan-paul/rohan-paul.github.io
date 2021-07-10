# Algorithms in JavaScript: Linked Lists. Remove the n-th node — LeetCode Challenge

A super popular Algorithmic problem of removing the n-th node from a linked-list faced in a Leetcode challenge.

Algorithms in JavaScript: Linked Lists. Remove the n-th node — LeetCode Challenge

![](https://cdn-images-1.medium.com/max/1200/1*IpZAU6ZcwSMWY1F4QRcAUQ.jpeg)

A super popular Algorithmic problem of **removing the n-th node from a linked-list faced in a** [**Leetcode challenge**](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/) **.**

**Here’s the Original Challenge** — Given a linked list, remove the nth node from the end of list and return its head.

For example,

Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5. Note: Given n will always be valid.

#### Some basics on Linked List

**Singly Linked List,** in which each node contains two properties, a value and a next pointer.

*   The value property can contain anything–a primitive (string, number, boolean, etc.), object or array, or even another Linked List if you want to go full inception!
*   The next property is simply a pointer for the very next node in the Linked List (or NULL if it is the last node in the list).
*   The operations we can perform on singly linked lists are **insertion**, **deletion** and **traversal**.

![](https://cdn-images-1.medium.com/max/800/1*AZ6s5819pIfqdmh_PpHP4A.png)
undefined

**Doubly Linked List**

In a doubly linked list, each node contains a data part and two addresses, one for the previous node and one for the next node.

![](https://cdn-images-1.medium.com/max/800/1*xo0RFYnenMu78HaZqldTag.png)
undefined

**Circular Linked List**

In circular linked list the last node of the list holds the address of the first node hence forming a circular chain.

![](https://cdn-images-1.medium.com/max/800/1*N7WrF2PJ8qyu_UQAlkpinA.png)
undefined

#### Approach 1 to the above LeetCode Problem : Two pass algorithm —

I will assume that n being valid means 1 < n <= total+1 and that head!=null. If head==null or invalid n everything breaks.

Its a straightforward two-passes where I first detect the length of the linked list and the second pass is to delete its n-th node.

**First, what is a head of Linked List —** The head is not a separate node, but just a reference to the first node. It keeps the whole list by storing a pointer to the first node.

Another noted difference is that head is an ordinary local pointer variable which is stored in stack, whereas list nodes gets stored in heap. So In most jargon terms Head is just a local pointer which keeps reference to the first element of a linked list and is mostly initialised with NULL in order to distinguish an empty linked list.

*   In order to remove a node from a linked list, we need to find the node that is just before the node we want to remove. Once we find that node, we change its next property to no longer reference the removed node. But, the previous node is modified to point to the node after the removed node. So the key line of code will be
*   `**prevNode.next = prevNode.next.next**`
*   For example, to remove first node, we need to make second node as head and delete memory allocated for first node.
*   We are just skipping over the node we want to remove and linking the “previous” node with the node just after the one we are removing.
*   The nth node from the end will be (list.length — n + 1)-th node from the begining of the Linked List.
*   For example:

**1**\-> **2** -> **3** -> **4** -> **5** -> **6** -> **7**

*   To return 2nd node from the end(n = 2), we need to return 6th (7–2 + 1) node from beginning which is node ‘6’.
*   So the problem could be simply reduced to another one : Remove the **(L — n + 1)-th** node from the beginning in the list , where L is the list length.
*   First we will add an auxiliary dummy or fake head node which in the below program I am naming as **“currentNode”**, which points to the list head. The “dummy” node is used to simplify some corner cases such as a list with only one node, or removing the head of the list. The key step is we have to relink next pointer of the **(L — n)-th** node to the **(L — n + 2)-th** node and we are done.

**Here was the implementation of the above algo**

undefined
undefined

**Complexity Analysis of the above solution**

Time complexity : O(L).

The algorithm makes two traversal of the list, first to calculate list length LL and second to find the (L — n) th node. There are **2L-n operations** and time complexity is O(L).

Space complexity : O(1) as we only used constant extra space.

#### Approach 2: One pass algorithm

Instead of one pointer, we could use two pointers. The first pointer advances the list by n+1 steps from the beginning, while the second pointer starts from the beginning of the list. Now, both pointers are exactly separated by nn nodes apart. We maintain this constant gap by advancing both pointers together until the first pointer arrives past the last node.

The second pointer will be pointing at the nth node counting from the last. We relink the next pointer of the node referenced by the second pointer to point to the node’s next next node.

Example: if n = 4 (4th node from the back), our 2 pointers will be as below:

![](https://cdn-images-1.medium.com/max/800/1*hLBwXPOjT63_670p1suQGQ.png)
undefined

The back pointer is always n-1 = 3 nodes behind the front. When the front pointer reaches the end, the back pointer will point to the nth node from the back.

Here’s a single-pass solution

#### Algorithm

1.  Create and assign **frontPointer** to HEAD, then move with **n-1** steps forward.
2.  Create a **backPointer** and set it to null.
3.  Move both pointers forward until **frontPointer** points to the last node.
4.  Remove the node at back pointer, as follows:
5.  If **backPointer** is null, That means we’re trying to remove HEAD from the list.  
    Return HEAD’s next.
6.  Otherwise, set **backPointer.next** to **backPointer.next.next.** Return HEAD.

undefined
undefined

**Complexity Analysis**

Time complexity : O(L).

The algorithm makes one traversal of the list of L nodes. Therefore time complexity is O(L).

Space complexity : O(1) as we only used constant extra space.

In the one-pass approach, the first pointer scans L-n elements, second pointer scans L-n elements,  
the total scanned are: n + (L-n)\*2 = 2L — n, which is the same as solution 1.

So superficially, it seems that Approach 2 one pass algorithm has less time complexity (constant), it is in fact the same as the Approach 1

#### Further note on Complexity Analysis of Linked List

In order to delete a node and connect the previous and the next node together, you need to know their pointers. In a doubly-linked list, both pointers are available in the node that is to be deleted. The time complexity is constant in this case, i.e., O(1).

Whereas in a singly-linked list, the pointer to the previous node is unknown and can be found only by traversing the list from head until it reaches the node that has a next node pointer to the node that is to be deleted. The time complexity in this case is O(n).

While the actual process of deleting a node is constant, we must know the previous node. In a Singly Linked List, we must _search_ for the node whose _next_ property points to the node we want to delete–search is a linear operation. A Doubly Linked List gives us constant time access to all three nodes (previous, current, next) so long as we have the current.

In cases where the node to be deleted is known only by value, the list has to be searched and the time complexity becomes O(n) in both singly- and doubly-linked lists.

And below, just for more practice for me on this topic, a simple generic implementation of a very simple single-linked-list. Where, I have to remove node given the node itself as the passed-in argument to the `remove`function.

Here, the design of this linked list involves creating two classes — a **Node** class for adding nodes to a linked list, and create a **LinkedList** class, which will provide functions for inserting nodes, removing nodes, displaying a list, and other housekeeping functions within the LinkedList as a whole.

undefined
undefined

By [Rohan Paul](https://medium.com/@paulrohan) on [September 30, 2019](https://medium.com/p/9b6a98d8a3b8).

[Canonical link](https://medium.com/@paulrohan/algorithm-in-javascript-linked-list-remove-the-n-th-node-leetcode-challenge-9b6a98d8a3b8)

Exported from [Medium](https://medium.com) on December 12, 2020.