---
title: Application Architecture
slug: application-architecture
---

At this point in the project, it is a good idea to pause, step back, and take a look at the architecture of the code base. We've written a fair amount of code without much explicit thought given to the design or composition. In all likelihood, our code is messy and convoluted and in need of some careful refactoring.

When approaching questions of application architecture, there are a lot of directions to choose from. To clarify thinking about how to structure the application, start with questions such as:

- What are the key features of the application? Are these clearly separated into their own files, classes, and/or modules?
- Are the names of files, modules, functions, and variables appropriate and accurate? Would a new programmer be able to understand the names without too much contextual knowledge?
- What are the scopes of variables and are they appropriate for their use case? If there are global variables, why are they needed?
- Are the functions small and clearly specified, with as few side effects as possible?
- Are there functions that could be better organized in an Object-Oriented Programming style by defining them as methods of a class?
- Can files be used as [both modules and as scripts](https://docs.python.org/3/tutorial/modules.html#executing-modules-as-scripts)?
- Do modules all depend on each other or can they be used independently?

These questions will guide you towards thinking about application architecture in a more nuanced way. If you find that your code can use improvement (and what code *couldn't* use improvement), refactor until you are happy with it.

An example high-level architecture for the complete Tweet Generator application might look like this, with each file acting as *both* a module and a script:

	app.py          # main script, uses other modules to generate sentences
	cleanup.py      # module for cleaning up source text
	tokenize.py     # module for creating lists of tokens from a text
	word_count.py   # module for generating histograms from a list of tokens
	sample.py       # module for generating a sample word from a histogram
	sentence.py     # module for generating a sentence from a histogram

The primary interface to this application could be something like:

	$ python app.py corpus.txt
	Some random generated sentence based on word frequency.

And `app.py` could be something like this:

	import cleanup
	import tokenize
	import word_count
	import sample
	import sentence

	# define some functions that compose the above modules

	if __name__ == '__main__':
	    # code to run when file is executed

This is just one way to organize the code. What's important is that you think carefully about what kind of design makes the most sense for your code base.

Organizing Functions with Classes
==

Let's take a deeper dive into one of the above questions in particular:
- Are there functions that could be better organized in an Object-Oriented Programming style by defining them as methods of a class?

Remember how we defined several different functions to create a histogram and access word frequency data out of it? Perhaps we could organize them as instance methods of a Histogram class.

If you're not familiar with how to define classes in Python, you may want to [read some of the Classes chapter of the Python tutorial](https://docs.python.org/3/tutorial/classes.html) to get a handle on the syntax. You don't need to read the whole chapter before getting started, especially if you've defined classes in other programming languages.

I've created a starter code template to help guide you in refactoring your histogram functions into classes. You'll need to:

1. Pull the starter code from the course's origin repository (assuming you have [set up your repository](https://github.com/Product-College-Courses/CS-2-Tweet-Generator/blob/master/Setup.md) correctly)

		git pull course master

2. Now navigate to the `source` directory and run the unit tests to see which methods are passing or failing:

		python dictogram_test.py  # tests for Dictogram class (subclass of dict)
		python listogram_test.py  # tests for Listogram class (subclass of list)

3. Alternatively you can use `pytest` the run unit tests to see more readable and descriptive output:

		pip install pytest  # (only need to install once)
		pytest dictogram_test.py  # tests for Dictogram class (subclass of dict)

4. Refactor your existing histogram functions into `Dictogram` and `Listogram` class instance methods

5. Run the unit tests again and fix any errors until they pass:

		python dictogram_test.py  # standard unittest error formatting
		pytest dictogram_test.py  # pytest pretty formatting and more assertion info

6. You can also run the `dictogram.py` or `listogram.py` modules as a script to check your results while refactoring:

		python dictogram.py
		python listogram.py

Before implementing the missing class instance methods (marked `# TODO`):

	word list: ['a', 'b', 'r', 'a', 'c', 'a', 'd', 'a', 'b', 'r', 'a']
	dictogram: {}
	0 tokens, 0 types
	'r' occurs None times
	'a' occurs None times

	word list: ['one', 'fish', 'two', 'fish', 'red', 'fish', 'blue', 'fish']
	dictogram: {}
	0 tokens, 0 types
	'blue' occurs None times
	'fish' occurs None times

	word list: ['how', 'much', 'wood', 'would', 'a', 'wood', 'chuck', 'chuck', 'if', 'a', 'wood', 'chuck', 'could', 'chuck', 'wood']
	dictogram: {}
	0 tokens, 0 types
	'chuck' occurs None times
	'wood' occurs None times

After correctly implementing the histogram class instance methods:

	word list: ['a', 'b', 'r', 'a', 'c', 'a', 'd', 'a', 'b', 'r', 'a']
	dictogram: {'a': 5, 'b': 2, 'r': 2, 'c': 1, 'd': 1}
	11 tokens, 5 types
	'r' occurs 2 times
	'a' occurs 5 times

	word list: ['one', 'fish', 'two', 'fish', 'red', 'fish', 'blue', 'fish']
	dictogram: {'one': 1, 'fish': 4, 'two': 1, 'red': 1, 'blue': 1}
	8 tokens, 5 types
	'blue' occurs 1 times
	'fish' occurs 4 times

	word list: ['how', 'much', 'wood', 'would', 'a', 'wood', 'chuck', 'chuck', 'if', 'a', 'wood', 'chuck', 'could', 'chuck', 'wood']
	dictogram: {'how': 1, 'much': 1, 'wood': 4, 'would': 1, 'a': 2, 'chuck': 4, 'if': 1, 'could': 1}
	15 tokens, 8 types
	'chuck' occurs 4 times
	'wood' occurs 4 times

Once you finish implementing the `Dictogram` and `Listogram` classes and have made the respective unit tests pass, be sure to commit your solutions and push to GitHub!
