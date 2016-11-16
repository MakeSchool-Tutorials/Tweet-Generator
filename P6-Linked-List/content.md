---
title: Linked List
slug: linked-list
---

Linked List
==

We're going to take a little detour in this project to explore some core data structures and learn how they work. We'll first start with the [*linked list*](https://en.wikipedia.org/wiki/Linked_list) structure.

So, what is a linked list? In its simplest form, a linked list is a group of nodes where each node contains both some data (the value to store) and a link (or reference) to the next node in the group.

Data is appended to a linked list by creating a new node and referencing it from the last node already in the list. Removing a node is as simple as changing the reference from the previous node in the list so that it points to the next node in the list.

We'll describe the desired behavior with a sample interface. Then it will be up to you to implement the linked list yourself.

## Specifications
Our linked list must implement the following features:

- Can store any type of value in a node's **data** property
- Stores the first node as its **head**
- Stores the last node as its **tail**
- Can **append** a new value
- Can **find** a value from linked list
- Can **delete** a value from linked list

Here is an example interface:

	mylist = LinkedList()

	mylist.append('a')
	mylist.append('b')
	mylist.append('c')

	mylist.head.data  # => 'a'
	mylist.tail.data  # => 'c'
	mylist.find(lambda data: data > 'b')  # => 'c'
	mylist.delete('a')
	mylist.head.data  # => 'b'

> [action]
>
Not too bad, right? Ok, now go build your own linked list!

### Starter Code Project

I've created a starter code project to help guide you in creating the `LinkedList` structure. You'll need to:

1. Sign into GitHub
2. Accept the [Linked-List assignment](https://classroom.github.com/assignment-invitations/9b6d3ce30d111ba7dcaa0ad34763c85c) and follow the link to the new repository
3. Clone the Linked-List repository and run the unit tests to see the failures
4. Implement the missing instance methods, then run the unit tests and fix any errors until they pass

You can run the unit tests to see which methods are passing or failing with:

	python test_linkedlist.py

Once you finish implementing the `Node` and `LinkedList` classes and have made the respective unit tests pass, be sure to commit your solutions and push to GitHub!

### Help! All this talk of links and nodes has got me feeling woozy.

If it's your first time working with linked lists, it may seem confusing. Never fear, however, linked lists are actually quite simple. The terminology can be daunting at first, but once you build one for yourself you'll see how basic they actually are.

Let's start with the purpose of a linked list. Say you want to store a sequence of data, like a grocery list. Using an array (or list in Python) you would store each grocery item as an element of the list, like this:

	0           1        2       3
	["spinach", "beans", "rice", "oil"]

Finding an element from a list is easy enough, the computer just has to iterate through each item until it finds the one you want. What about adding an item to the end of the list? Well, in that case the computer just creates a new element (allocates more memory) for the list at the end, at index 4:

	0           1        2       3      4
	["spinach", "beans", "rice", "oil", "guava"]

Not too bad. But what if you wanted to insert an item at the _beginning_ of the list, at index 0? In that case, you'd have to create room at the end of the list and move _every single item after index 0_. It would take the computer more work to do this.

Think of a list like a row of chairs in a fancy theater. It's easy to place people in an empty seat, or to reach people in their seat. But what if you're late to the show and someone is in your aisle seat? Because you're not about to miss the show, you ask them to move over, which means that they'll have to ask the person next to them to move over, and on and on and on...

This is a lot of extra work. It would be much simpler if we could just put a new chair down in the position we need it.

So we come to the linked list. A linked list is like an array (or list in Python) in that it stores data as a sequence, but unlike an array it does not store its data as _elements_ which can are referenced by their _index_. Instead,  a linked list stores _nodes_ which themselves contain a _reference_ (or link) to the next node in the sequence.

Let's go back to our original grocery list problem, except now we'll display it as a linked list:

	[ ("spinach") -> ("beans") -> ("rice") -> ("oil") ]

In this notation, the `[]` denotes the linked list, each `()` represents a node, and each `->` represents a reference, or link.

Looking at this list, the following sentences are all true:

* The **node** with value `"spinach"` is at the **head** of the linked list
* The **node** with value `"beans"` is the next node after the **head**
* The **tail** of the linked list is the **node** with value `"oil"`
* There are 4 **nodes** in this linked list
* This is a **singly linked list**

That last one means that each node in the list references only one other node (the next one in the sequence). In contrast, a _doubly linked list_ has nodes which reference two nodes: the previous node and the next node in the sequence. Don't worry about doubly linked lists for now, though.

So let's see how we'd perform some basic operations with a linked list.

To add an node to the end of the list, we just create a new node and add a reference to it from the last node (tail) of the list:

	[ ("spinach") -> ("beans") -> ("rice") -> ("oil") -> ("guava") ]

Now, what about adding a node to the beginning of the list? To do that, we just create a new node and make it reference the head of the list:

	[ ("milk") -> ("spinach") -> ("beans") -> ("rice") -> ("oil") -> ("guava") ]

Because a linked list does not have indexed elements, like an array, we don't have to shift anything around. It's as simple as changing the reference on a _single node_, as opposed to moving _all of the elements_. In big-O terms, inserting a new item in a linked list has O(1) complexity, while inserting a new element in an array has O(n) complexity. Same goes for removing a node / deleting an element.

So that's pretty neat.

If the image of a linked list isn't quite sticking, let's use an analogy. Whereas an array (or list in Python) is like a row of chairs in a theater, a linked list is like a train with cars (nodes) that have cargo (values) and are connected to each other by a coupling (reference/link). The front of the train is like the head of the list, and the back (the caboose) is like the tail of the list.

The flexibility of a linked list makes it a powerful tool in many programming tasks. Have fun with it!

Where to Go From Here
==
Finished already? Is your code clean, readable, and well tested? No? Ok, go do that first. Then, add methods to your `LinkedList` class so that it can be used as an [iterable](https://wiki.python.org/moin/Iterator), like in a `for` loop like this:

	mylist = LinkedList()

	mylist.append('a')
	mylist.append('b')
	mylist.append('c')

	for data in mylist:
		print data

Need another challenge? Read about the [*doubly linked list*](https://en.wikipedia.org/wiki/Doubly_linked_list) structure and implement it in your own `DoublyLinkedList` class.
