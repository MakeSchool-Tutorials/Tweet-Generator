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
- Can files be used as [both modules and as scripts](https://docs.python.org/2.7/tutorial/modules.html#executing-modules-as-scripts)?
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

If you're not familiar with how to define classes in Python, you may want to [read some of the Classes chapter of the Python tutorial](https://docs.python.org/2.7/tutorial/classes.html) to get a handle on the syntax. You don't need to read the whole chapter before getting started, especially if you've defined classes in other programming languages.

I've created a starter code project to help guide you in refactoring your histogram functions into classes. We'll be using *Classroom for GitHub* to manage repositories with starter code. You'll need to:

1. Sign into GitHub and be sure you can access [GitHub Classroom](https://classroom.github.com/)
2. Accept the [Histograms assignment](https://classroom.github.com/assignment-invitations/1a71b50f3f67f9d2574caf0b915c1ba1) and follow the link to the new repository
3. Clone the Histograms repository and run the unit tests to see the failures
4. Refactor your existing histogram functions into class instance methods, then run the unit tests and fix any errors until they pass

You can run the `histograms.py` module as a script to test while refactoring:

	python histograms.py

You should also run the unit tests to see which methods are passing or failing:

	python test_histograms.py

Once you finish implementing the `Dictogram` and `Listogram` classes and have made the respective unit tests pass, be sure to commit your solutions and push to GitHub!
