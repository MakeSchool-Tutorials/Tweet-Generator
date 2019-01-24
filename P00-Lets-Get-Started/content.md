---
title: Let's Get Started
slug: lets-get-started
---

Learning about data structures and data processing is way more fun when you embed it in a project.  To do that, let's build a fun and silly tweet generator that generates sentences that sound realistic, but not always sensible.

Here's an example generated sentence from the Ron Paul bot:
> What currency are your thoughts on abolishing America's income tax altogether and get others to look into our private communications.

We'll build this step-by-step; diving into the programming and Python you need along the way.

1. Use a Python script to randomly generate words from a dictionary.
1. Build sentences by sampling these words using a Markov language model.
1. Implement grammar rules parsed from the text of a large document set.
1. Build data structures including linked lists, hash tables, stacks, queues, and heaps to store the words and sentences.
1.  Analyze the inner workings and performance tradeoffs of each data structure.
1. Deploy your language model to a Flask web server on Heroku and connect it to Twitter to let users tweet their favorite results.

# Learning Outcomes
By the end of this tutorial, you will:

- master Python data structures and data processing algorithms
- implement data structures including linked lists, hash tables, stacks, queues and heaps
- parse text documents and store information in data structures
- implement sampling methods and Markov chains on data sets

> [action]
> Take a moment to read through these learning outcomes and make a note of how much you already know about each one.  You'll look back at this when you are done with the tutorial.


# Prerequisites
This project is built in Python and relies on a basic knowledge of Python.

**Wait! I don't know Python!**

No problem! Python is a friendly language with lots of great resources for those of us who are just getting started.

One recommended site is the [official Python Tutorial](https://docs.python.org/2.7/tutorial/), although you are welcome (and encouraged!) to use other resources to learn.

Some of the topics you'll definitely need to research are:

- variable assignment
- function definitions
- core data types: strings, integers, floats
- collection types: lists, tuples
- the module pattern
- the system module (for accessing command-line arguments)
- the random module (for generating random numbers)

You don't need to learn all this before you start, you can learn it as you need along the way.


# Github & Deployment
As you go through this tutorial, you will regularly update your progress on Github and through a deployed solution.

- You are required to commit each new feature or update to Github.  These commits represent the minimum commit frequency; feel free to commit more often.
- You will also regularly deploy updates to your Flask web server on Heroku.

> [action]
> This tutorial uses starter code:
>
- Follow the **[Repository Setup Instructions](https://github.com/Make-School-Courses/CS-2-Tweet-Generator/blob/master/Setup.md)** to set up your Github repository from this starter code.
- Add a README.md file to your repo and add information about the project to this file.
>
> **Commit your code:**
>
```bash
$ git add .
$ git commit -m 'initial commit after starter code'
$ git push
```


Python Modules
==

Our sentence generators will be built as a series of scripts that can be executed directly in the terminal or imported and used in other files.  [Modules](https://docs.python.org/3/tutorial/modules.html) are a common pattern in Python that allow us to do both of these things in a single file.

For example, here is a program that will return a random Monty Python quote -

`python_quote.py`

	import random

	quotes = ("It's just a flesh wound.",
	          "He's not the Messiah. He's a very naughty boy!",
	          "THIS IS AN EX-PARROT!!")

	rand_index = random.randint(0, len(quotes) - 1)
	print quotes[rand_index]

As written, this code can only be run as a script because all lines are always executed, so importing this as a module into another program would make it print out a quote, even if we only want to access the `quotes` tuple. In Python, the common pattern for making files dual-purpose (modules as well as executables) is to use a conditional that will only run certain code if the current process file is the current source file, like this:

	import random

	quotes = ("It's just a flesh wound.",
	          "He's not the Messiah. He's a very naughty boy!",
	          "THIS IS AN EX-PARROT!!")


	def random_python_quote():
	    rand_index = random.randint(0, len(quotes) - 1)
	    return quotes[rand_index]

	if __name__ == '__main__':
	    quote = random_python_quote()
	    print quote


This script can be imported within another file in the same directory with `import python_quote`, which would give access to the `random_python_quote()` function, or it can be executed directly, like so:


	$ python python_quote.py
	It's just a flesh wound.

	$ python python_quote.py
	He's not the Messiah. He's a very naughty boy!


> [action]
>
> Write the **python_quote** module:
>
- In your tutorial repository (created from the starter code): create a **python_quote.py** file and copy the code above into it.
- Verify that you can execute **python_quote.py** directly from the terminal.
- Create a **quote_user.py** file that imports the **python_quote.py** module and runs the `random_python_quote()` function.
- Verify the importing works by executing **quote_user.py** directly from the terminal.
>
> **Commit your code:**
>
```bash
$ git add .
$ git commit -m 'Added python_quote module'
$ git push
```
>

Rearrange Words
==

Now that you have a repository and understand the module pattern, let's keep going by writing a basic script with only a few moving parts.

Write a script (*rearrange.py*), that randomly rearranges a set of words provided as command-line arguments to the script.  

For example, if you run the following command:

	$ python rearrange.py how now brown cow

You should see output like this:

	brown cow now how

Of course, because it is random, repeated executions of the script with the same arguments will produce different results:

	$ python rearrange.py how now brown cow
	brown cow how now

	$ python rearrange.py how now brown cow
	now how cow brown

> [action]
>
> Write the **reaarrange.py** script using the requirements above and add it to your tutorial repository
>
> **Commit your code:**
>
```bash
$ git add .
$ git commit -m 'Added rearrange module'
$ git push
```
>
> **Note:** From here on, we expect you know how to commit so we won't be adding this code.  Make sure you commit after each new feature and use meaningful commit messages.



Where to Go From Here
==

> [challenge]
>
Finished already? If you still have time left, you can keep building off of this idea or try writing other programs that work with words, for example:
>
- String reversals: reverse words, sentences
- An interactive "mad libs" script
- Anagram generator

>
