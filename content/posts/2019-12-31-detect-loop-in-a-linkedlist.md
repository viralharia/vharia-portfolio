---
template: post
title: Detect loop in a LinkedList
date: '2019-12-31 13:29'
description: 'Given a linked list, check if the linked list has loop or not.'
category: DataStructures
tags:
  - DataStructures
---
Below diagram shows a linked list with a loop.

![single AZ architecture diagram](/media/linked_list_loop.jpg)

I think, there are 2 main ways to detect the loop in linked list.

### Way 1 - Using `HashSet` to store the visited nodes
+ Traverse the list one by one and putting the node in a **_Visited nodes_** set. 
+ At any point, if NULL is reached then return `false`, and if next of current node points to any of the previously stored nodes in Hash then return `true`.

```java
//Detects loop in the given linked list
public static <T> boolean detectLoop(SinglyLinkedList<T> list) {
        Set<SinglyLinkedList<T>.Node> visitedNodes = new HashSet<>();
        SinglyLinkedList<T>.Node currentNode = list.headNode;        
        while(currentNode != null){
            if(visitedNodes.contains(currentNode)){
                return true;
            }
            visitedNodes.add(currentNode);
            currentNode = currentNode.nextNode;
        }
        return false;
}
```

### Way 2 - Floyd’s Cycle-Finding Algorithm OR Tortoise and Hare Algorithm
+ First off, check if the list is empty (head is null). If so, no cycle exists, so stop now.
+ Otherwise, start the first pointer tortoise on the first node `head`, and the second pointer hare on the second node `head.next`.
+ Then loop continuously until hare is `null` (which may be already true in a one-element list), advancing tortoise by one and hare by two in each iteration. The hare is guaranteed to reach the end first (if there is an end) since it started ahead and runs faster.
+ If there is no end (i.e., if there is a cycle), they will eventually point to the same node and you can stop, knowing you have found a node somewhere within the cycle.  

Consider the following loop which starts at 3:
```
head -> 1 -> 2 -> 3 -> 4 -> 5
                  ^         |
                  |         V
                  8 <- 7 <- 6
```
Starting tortoise at 1 and hare at 2, they take on the following values:
```
(tortoise,hare) = (1,2) (2,4) (3,6) (4,8) (5,4) (6,6)
```
+ The time complexity for this algorithm is `O(n)` since the number of nodes visited (by tortoise and hare) is proportional to the number of nodes.

```java
//Detects loop in the given linked list
public static <T> boolean detectLoopHareAndTortoiseAlgo(SinglyLinkedList<T> list) {
        if(list.isEmpty()){
            return false;
        }

        SinglyLinkedList<T>.Node tortoiseNode = list.headNode;
        SinglyLinkedList<T>.Node hareNode = tortoiseNode.nextNode;

        while(hareNode != null && hareNode.nextNode != null)
        {
            tortoiseNode = tortoiseNode.nextNode;	//the tortoiseNode pointer will jump 1 step
            hareNode = hareNode.nextNode.nextNode; //the fasr pointer will jump 2 steps 
			// when the pointers become equal then there must be a loop
            if(tortoiseNode == hareNode){
                return true;
            }
        }
                
        return false;
}
```

#### To find the start of the cycle:
+ Once we find the slow A and fast B meet in the cycle, make one of them still and the other continue to go one step each time, to decide the perimeter of the cycle, say, P.
+ Then we put a node at the head and let it go P steps, and put another node at the head. We advance these two nodes both one step each time, when they first meet, it's the start point of the cycle.
