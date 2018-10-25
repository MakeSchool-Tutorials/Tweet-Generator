---
title: Hash Table
slug: hash-table
---

In Python, one of the collection types is the dictionary type, which allows you to map keys to values. This is a common and essential data structure with a wide range of purposes.

For example, you might use a dictionary to store your friends' phone numbers:

	phones = {'Bob': '555-321-4321', 'Sue': '555-123-1234'}
	phones['Bob']  # => '555-321-4321'
	phones['Sue']  # => '555-123-1234'

You can also use a dictionary to store different types of information associated with a person:

	student = {'name': 'Amy', 'age': 22}
	student['name']  # => 'Amy'
	student['age']   # => 22

You may have used dictionaries already, but do you know how they work? The next stop we're going to take on our little detour in this project is to learn about the data structure that powers Python's dictionary feature: the [*hash table*](https://en.wikipedia.org/wiki/Hash_table).

We'll describe the desired behavior with a sample interface. Then, we'll go over some of the constraints and tools we can use. Finally, it will be up to you to implement the hash table yourself.

## Specifications
Our hash table must implement the following features:

- Can **set** any type of value to associate it with a key
- Can **get** the value associated with a key
- Can **delete** a key and its associated value
- Can test if the hash table **contains** a key
- Can get a list of all **keys** in the hash table
- Can get a list of all **values** in the hash table
- Can get a list of all **entries** (key-value pairs) in the hash table
- Can calculate the **length** of the hash table

Here is an example `HashTable` object that implements the specifications listed above. It stores the decimal digit that corresponds to its roman numeral.

	roman = HashTable()
	roman.set('I', 1)
	roman.get('I')         # => 1
	roman.set('V', 5)
	roman.set('X', 9)
	roman.set('X', 10)     # Oops, let's fix that (can update a key's value)
	roman.get('X')         # => 10
	roman.keys()           # => ['I', 'V', 'X']
	roman.values()         # => [1, 5, 10]

## Constraints
- Your hash table must only use the list and tuple collection types (no dictionaries or sets)
- Keys must be a string, a number, or a tuple of strings and/or numbers
- Reading from the table (getting a value for a given key) must have an O(log n) lookup time or better

## Tools
An essential tool for this problem is Python's built-in `hash()` function. Read up on the documentation and the Wikipedia entry for [*hash functions*](https://en.wikipedia.org/wiki/Hash_function) to understand how `hash()` works and why it is useful. Then play around with `hash()` in a REPL or with some experimental scripts.

> [action]
>
Got it? Ok, go forth and build your own hash table!

### Starter Code Project

I've created starter code and unit tests to help guide you in creating the `HashTable` structure. You'll need to:

1. Pull the Hash Table starter code from the course's origin repository

		git pull origin master

2. Run the unit tests to see which methods are passing or failing:

		python test_hashtable.py

3. Alternatively you can use `pytest` the run unit tests to see more readable and descriptive output:

		pip install pytest  # (only need to install once)
		pytest test_hashtable.py

4. Implement the missing instance methods, then run the unit tests and fix any errors until they pass

5. Run the unit tests again and fix any errors until they pass:

		python test_hashtable.py  # standard error formatting
		pytest test_hashtable.py  # or pytest pretty formatting

6. You can also run the `hashtable.py` module as a script to check your results while refactoring:

		python hashtable.py

	After correctly implementing the missing instance methods (marked `#TODO`):

		HashTable([])
		get(I): 1
		get(V): 5
		get(X): 10
		length: 3
		HashTable([('I', 1), ('X', 10), ('V', 5)])
		HashTable([('I', 1), ('V', 5)])
		HashTable([('I', 1)])
		HashTable([])
		length: 0

Once you finish implementing the `HashTable` class and have made the respective unit tests pass, be sure to commit your solutions and push to GitHub!

Roman Numerals Example
==

Want some help with how to think about this? No problem. Let's backtrack to the purpose of a hash table.

What makes hash tables so useful is that they allow us to store *values* (strings, numbers, collections, or any other object) by a *key* of our choosing, which decouples the order of the elements from our ability to insert and retrieve them. In other words, a hash table is kind of like a list, which stores values at numerical indexes, but in a hash table the indexes can be non-numerical and their order doesn't matter.

That's a lot of language. Let's look at an example, comparing a basic list to a hash table. Take a look at this list:

	roman_list = [('I', 1), ('V', 5), ('X', 10)]

It contains the same *data* as the example hash table above, but in order to access and use this data we need to know a lot more about *how* it is structured.

For example, if I wanted to find the value of the Roman numeral V, I would have to go through a series of steps to get there:

1. Iterate over each item in the list
2. For each item (a tuple), check the value of the first element (at index `0`) in the tuple
3. If the first element is equal to the value provided (`'V'` in this case), then ...
	- Return the value of the second element (at index `1`) in the tuple
5. If we reach the end of the list, then that key must not exist, so ...
	- Return `None` (or throw an error? There are multiple ways to solve this case)

Compare that with an equivalent hash table lookup:

	roman_hash_table.get('V') # => 5

The only thing I need to know about this hash table is that it has a `get()` method which takes my *key* and returns a *value*. That's it. No knowledge of the internal structure is required.

So, a major benefit of hash tables is this key-value mapping interface, also called an [*associative array*](https://en.wikipedia.org/wiki/Associative_array). They are used all over the place in modern programming.

Under the hood, however, all hash tables still have to use the fundamental building blocks that computers provide. Lists (also called arrays) are a lower-level data structure for storing ordered collections of elements in "buckets" that can then be retrieved by their index, an integer indicating their position within the 1-dimensional array of buckets.

Hash tables use these lower-level arrays in creative ways to allow programmers to use a simpler interface of key-value storage without having to worry about the underlying array structure.

The most basic abstraction step you can take towards building a real hash table would be to simply store the keys and values in a list:

	basic_table = ['I', 1, 'V', 5, 'X', 10]

With that, you could write a `get()` function that takes a key and this list, and then returns the item in the position immediately following the key:

> [solution]
>
	def get(key, assoc_array):
	    value = None
	    for index, item in enumerate(assoc_array):
	        if item == key:
	            value = assoc_array[index + 1]
	            break
	    return value
>
	get('V', basic_table) # => 5

This is the simplest possible way to structure this data, and it's not very elegant or efficient.

There are lots of other ways to structure it, and to truly implement a hash table you'll need to make use of a *hash function* to make indexes from keys. Read up on [hashing](https://en.wikipedia.org/wiki/Hash_table#Hashing) to learn more about how this works.

A Better Hash Table
==

Ok, what have we done so far? Well, we've built an alternative to Python's built-in dictionary structure. But quite frankly, it's not very good. It's too basic and not nearly performant enough to be called a _real_ hash table.

In this next stage, we're going to **refactor** our hash table (change the implementation, not the interface) so that it functions and performs more like a real, production-grade hash table.

## Using Buckets to Store Values

The first critical feature of a hash table is how elements are assigned to "buckets". If your original hash table implementation didn't use a limited number of buckets in your table and the `hash()` function to generate a hashed value from the key, and then find the remainder from dividing the hashed key by the number of buckets, and use _that_ number to pick which bucket to put the value in, then _it's not a real hash table_.

That was a mouthful. Let's use a real example to walk through it in more detail. Say we want to store the value `22` at the key `"age"` in a hash table called `student`. We'll create a hash table with 6 initially empty buckets (in Python, these can be items in a list), which we can represent visually like this:

	0   1   2   3   4   5
	[ ] [ ] [ ] [ ] [ ] [ ]

If we use Python's built-in `hash()` function to create a hash of the key (i.e. the string value `"age"`), it will give us back a number, like this:

	hash("age")
	# => 1453079729191098346

Now that we have a numeric representation of the string `"age"`, we pick a bucket to put it in by finding the remainder of this number divided by the number of buckets:

	1453079729191098346 % 6
	# => 4

This tells us to place our value in bucket `4`:

	0   1   2   3   4      5
	[ ] [ ] [ ] [ ] [ 22 ] [ ]

Because a [hash function](https://en.wikipedia.org/wiki/Hash_function) will always return the same, unique number for a given input (caveat: many hash functions use a random seed, so it will only return the same number when called within the same process), we can use this to quickly look up values later. If we want to find the value stored at key `"age"`, all we need to do is hash the key and run our modulo calculation and voilá! We know which bucket to retrieve it from.

Compare this to searching for an item in a list or array, which requires that you walk through each item before returning the item you want. Doesn't that seem like a big waste of time?

> [action]
>
Does your hash table already use `hash()` and `hash_num % num_buckets`? If so, great! If not, make sure that it does!

## Handling Collisions

There is one glaring problem with that previous example of a hash table. You may have already noticed it: what happens when the remainder of the hash key divided by the number of buckets is the same for two different keys? How do we store both values and ensure that they are still associated with the correct key?

This is called a **collision** and any hash table worth its salt needs to handle them gracefully and efficiently.

Let's illustrate the problem a bit more. Remember our example `student` hash looks something like this, with the value `22` at bucket `4`, which we access by hashing the key `"age"`:

	0   1   2   3   4    5
	[ ] [ ] [ ] [ ] [22] [ ]

Say we wanted to add the value `"Amy"` for the key "name", but the remainder of `hash("name") % 6` is _also_ `4`. What do we do? Make a list of both values, like this?:

	0   1   2   3   4             5
	[ ] [ ] [ ] [ ] [[22, "Amy"]] [ ]

No, that won't do. How do we know which value belongs to `"name"` and which to `"age"`?

There are many ways to solve this problem and if you're interested in learning more of the details you can read about [collision resolution](https://en.wikipedia.org/wiki/Hash_table#Collision_resolution). In this tutorial, we're going to handle collisions with a technique known as *chaining*. Each bucket of our hash table will be a linked list containing tuples that hold both the key and the value. This way we can gracefully handle collisions and have the performance benefits of storing values in buckets.

This way, looking up a value for a key will follow these steps:

1. Calculate the bucket for the key using our hashing algorithm (apply the `hash` function and modulo the number of buckets)
2. Find the item in the bucket's linked list that has the key as its first element
3. Return the value (the item's second element)

When inserting a new value, our hash table will follow these steps:

1. Calculate the bucket for the key using our hashing algorithm
2. Append to the bucket's linked list a tuple with both the key and value to be stored

So, it looks like we'll need to use a linked list data structure. Good thing we've already created a `LinkedList` class! Here's how we can import our `LinkedList` class from the `linkedlist` module:

	from linkedlist import LinkedList

> [action]
>
Now improve your hash table to handle collisions by using linked lists as buckets!

Where to Go From Here
==
Finished already? Is your code clean, readable, and well tested? No? Ok, go do that first. Then, expand the interface for your hash table to match that of [Python's built in dictionary](https://docs.python.org/2.7/library/stdtypes.html#mapping-types-dict). Your hash table should support many of the most commonly used features of a regular dictionary, like the `items` method to get a list of key-value pairs. You can also add several magic methods to allow dict-like subscripting syntax.

Consider how you calculate the **length** of the hash table. Do you traverse through all buckets each time the `length` method is called? Is this efficient? Is it necessary? Consider an alternative approach that doesn't require bucket traversal and implement it. Benchmark its running time against the first approach on small hash tables and large ones.

Want to make your `HashTable` class more convenient to use? Add methods so that it can be used as an [iterable](https://wiki.python.org/moin/Iterator), like in a `for` loop like this:

	roman = HashTable()
	roman.set('I', 1)
	roman.set('V', 5)
	roman.set('X', 10)
	for key, value in roman:
		print key, value

Need another challenge? Read about some of the other techniques for [collision resolution](https://en.wikipedia.org/wiki/Hash_table#Collision_resolution) (like linear probing) and implement one in your `HashTable` class. Benchmark its running time against chaining on small hash tables and large ones. What advantages and disadvantages does this approach have over chaining with linked lists?
