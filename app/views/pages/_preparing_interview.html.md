Objective C Class
=================

The [NSObject protocol](https://developer.apple.com/library/mac/documentation/cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) groups methods that are fundamental to all Objective-C objects, features including: class type, responding message, retain and release.

Objective C Memory Management
=============================

[Advanced Memory Management Programming Guide](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)

###Two methods of application memory management

1. MRR, manual retain-release
2. ARC, automatic reference counting

###Two problems

1. Freeing data in use
2. Not freeing data that is no longer in use

###MRR, manual retain-release

1. Two ways to create an object:
  * “alloc”, “new”, “copy”, or “mutableCopy”, own this object.
  * Other method, autorelease
2. Take ownership of an object by retain.
3. Relinquish ownership, by release, autorelease(stay valid until at least the end of the scope that it was called in), send dealloc message if counts equals to zero.
4. retain, release and autorelease are defined in the NSObject Protocol.

###ARC, automatic reference counting

[Transitioning to ARC Release Notes](https://developer.apple.com/library/mac/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html)

* Four qualifiers for object pointers, the two important are strong and weak. Strong reference will retain object, week will not. When strong get out of scope, release will be called. Strong property is the same, but when this object is deallocated, release will be called.

* Autoreleasing is used for multiple returns by pointers. It likes the strong pointer, but will hold the result after the function return.

###Practical Tips

* Do not need to worry about class method like **[NSString stringWithFormat:]**, because they are using autorelease to return an object.

* Dealloc is never called manually. Only called by **[Super dealloc]**.

* For Property, you must declare the ownership of it, either by alloc it or retain it.

* Use set accessor as many as possible.

* Do not use accessor methods in initializer and dealloc

  * B extends A, but if accessor is used in A, it could be override in B, which means that the methods of B is called before B is initialized. The same for dealloc, method of B will be called after B disappeared.

* Use week reference to avoid memory cycle. The reference is week unless using retain.

* Be carefully when releasing the parent or collection of an object.

* Collections automatically call retain and release.

###Autorelease Pool Blocks

* release called when get out of this block.

* AppKit and UIKit frameworks process each event-loop iteration within an autorelease pool block.

* Could add custom autorelease pool block

Constructor
===========

Constructor should not be override, since it should be the duty of this class to construct its objects, not the sub-classes' duty.

###Objective C

**[[Class alloc] init]** alloc some memory on the heap and init it, two problems:

1. A sub-class always need to call the constructor of the super-class. Easy to forget.
2. Always return an instance of this class. Repeat it everywhere. Could use **instancetype**.

###Java

1. Give a default, no-parameter constructor to every class, sub-class automatically call default super constructor. But if you define a custom constructor, the default one will be removed, and sub-class need to call **super(...)**.
2. No return for constructors in Java.

Static Binding or Dynamic Binding
=================================

* Static binding => compile time
* Dynamic binding => runtime

Unicode
=======

Using 2 bytes to store one character, this is called **code point**. We usually use 4 hexadecimal number to represent it, U+xxxx. UTF-8 is one way to encoding Unicode into bits, code point from 0-127 is stored in single byte.

Bitwise Operation
=================

###XOR

1. a ^ 1 = flip a
2. a ^ 0 = a
3. a ^ a = 0

###Negative number

* flip and +1
  * 00 ->  0
  * 01 ->  1
  * 10 -> -2
  * 11 -> -1

* 10 ... 0, is the smallest number
* & bitwise and, && logical and return true or false
* the same for | and ||
* ~ flips every bits
* \>\>> repeat the sign in front, negative number divided by 2
* \>> do not repeat the sign in front
* Bitwise operations have priority.

Trie vs Hash Table
==================

###Advantages
* In order to get the hash value, we need to go through the string, it is the same for trie. And there are collisions in hash table. If we do not use all the charactors to compute the hash value, there are more collisions.
* There is no need to provide a hash function or to change hash functions as more keys are added to a trie.
* A trie can provide an alphabetical ordering of the entries by key.

###Disadvantages
* Tries can be slower in some cases than hash tables for looking up data, especially if the data is directly accessed on a hard disk drive or some other secondary storage device where the random-access time is high compared to main memory.
* Some keys, such as floating point numbers, can lead to long chains and prefixes that are not particularly meaningful. Nevertheless a bitwise trie can handle standard ieee single and double format floating point numbers.
* Some tries can require more space than a hash table, as memory may be allocated for each character in the search string, rather than a single chunk of memory for the whole entry, as in most hash tables.

RESTful API
===========

###There is no standard rules
    A simple example:
            /teachers   /teachers/:id
    Get     index       show
    Post    create    
    Put                 update
    Delete  remove      destroy

###Rails
There are 7 actions, two interesting pairs are new/create and edit/update, new and edit are just used to render a view for create and update. So the actual actions are 5. Compared with the example above, no remove.

HTTP is a stateless protocol. Sessions make it stateful. The session saves the current status of a user and the session id is sent to the user. The user can use this id to login without authenticate again.

For URL like /users/1/students, the server can save 1 to params[:user\_id] automatically.

HTTP Request
===========

###HTTPS

* Man In The Middle
  * DNS spoofing: insert the man in the middle.
  * Provide fake key to the server and client.

* Layering HTTP on a transport layer security protocol.
  * The public key from the server has certificate authority(CA).

###HTTP Structure

* HTTP header
  * request or respond line
  * MIME header

* HTTP request body
  * GET does not have body.
  * POST contains the data.

###port

There are two ports, *source port* and *destination port*. When sending an http request, browser asks the OS for an available port as the *source port*, and the *destination port* is 80.
When the server sends back the respond, the *destination port* is the browser port, the *source port* is 80.
Thus, there could be multiple browsers running and send the requests to the same server through the same port.

JQuery
======

It is a JavaScript library

Design pattern
==============

###proxy vs decorator
* proxy is used to represent the basic class.
* decorator is used to change the behavior dynamically.

Leetcode round 2
================

###02/12/2014
* valid palindrome
* remove nth node from end of list
* unique binary search trees ii
* Combinations

###02/18/2014
* Convert Sorted List to Binary Search Tree
  * stack and queue
* Minimum Depth of Binary Tree
  * BFS or DFS
* Maximum Depth of Binary Tree
  * BFS or DFS
* Rotate List
  * C++ modulus have negative result, (a%b+b)%b

C++
===

* Call stack variable will get it's destructor called when get out of scope.

* Smart pointer: scoped pointer, shared pointer(memory cycle, weak pointer).

* Call stack has fixed size, which is defined when compile.

* If you want to call a superclass constructor with an argument, you must use the subclass's constructor initialization list.

* Destructors are called automatically in the reverse order of construction. (Base classes last). Do not call base class destructors.

* Virtual function is dynamic binding. Other functions are static binding. One thing to be aware of is that if either transmitter or receiver attempted to invoke the storable constructor in their initialization lists, that call will be completely skipped when constructing a radio object.

* Pure virtual function makes the class it is defined for abstract, it is an abstract function.

* Virtual inheritance to solve the diamond problem. Create only one instance of the virtual based class. Virtual based class is constructed first and destroyed last.

###02/19/2014
* Jump Game II
  * Do not need to use DP.
* Jump Game

###02/20/2014
  * 3Sum

###02/21/2014
  * Binary Tree Level Order Traversal II
  * Longest Palindromic Substring
  * Sudoku Solver
  * Longest Valid Parentheses

###02/22/2014
  * Spiral Matrix
  * Regular Expression Matching
  * Sqrt(x)
    * Check int approximation
    * Newton's method

###02/23/2014
  * Palindrome Partitioning
  * Sort List
  * Reorder List
  * Longest Substring Without Repeating Characters
    * Map.Entry
  * Unique Paths
  * Unique Paths II
  * Minimum Path Sum
  * Triangle

###02/24/2014
  * Interleaving String