---
title: Let's Get Started
slug: lets-get-started
---

Let's build something fun and silly! :-) Let's make a tweet generator that generates sentences that sound realistic, but not always sensible. Here's an example from the Ron Paul bot:
> What currency are your thoughts on abolishing America's income tax altogether and get others to look into our private communications.

So how will you make this? Well, not all at once. You're gonna do it in parts, step-by-step. You'll start by making a Python script that can randomly generate words from a dictionary. Then you'll iteratively build a more complex Markov language model that you can stochastically sample to generate approximately grammatical sentences. To learn a basic grammar, you'll need to collect a large set of documents and parse their text to learn relationships between words. You'll also implement your own data structures including linked lists, hash tables, stacks, queues, and heaps to better understand their inner workings and performance tradeoffs. Throughout development, you'll regularly deploy updates of your improved language model to your Flask web server on Heroku. When you're satisfied, you'll connect it to Twitter to let users tweet their favorite results.

Python Specs
==

Before you get going right now, let's talk about *modules* for a moment. Modules are a common pattern in Python that allows files to be run independently (as a *script*) and also imported by other files (as *modules*).

For example, here is a program that will return a random Monty Python quote -
`python_quote.py`

	import random

	quotes = ("It's just a flesh wound.",
	          "He's not the Messiah. He's a very naughty boy!",
	          "THIS IS AN EX-PARROT!!")

	rand_index = random.randint(0, len(quotes) - 1)
	print quotes[rand_index]

This code can only be run as a script because all lines are always executed, so importing this as a module into another program would make it print out a quote, even if we only want to access the `quotes` tuple. In Python, the common pattern for making files dual-purpose (modules as well as executables) is to use a conditional that will only run certain code if the current process file is the current source file, like this:

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


Rearrange Words
==

Off to the races! The first step is to get our bearings with Python by writing a basic script with only a few moving parts.

To start, we'll be building a script that randomly rearranges a set of words provided as command-line arguments to the script.

For example, if you run the following command (assuming your script name is *rearrange.py*, though you can use any name you like)

	$ python rearrange.py how now brown cow

You should see output like this:

	brown cow now how

Of course, because it is random, repeated executions of the script with the same arguments will produce different results:

	$ python rearrange.py how now brown cow
	brown cow how now

	$ python rearrange.py how now brown cow
	now how cow brown

That's it. You have all of the specs needed to produce a correct program.

> [action]
>
So, are you ready? Go code!

<!-- html comment to break boxes -->


*Wait! I don't know Python!*
==
No problem! Python is a friendly language with lots of great resources for those of us who are just getting started.

One recommended site is the [official Python Tutorial](https://docs.python.org/2.7/tutorial/), although you are welcome (and encouraged!) to use other resources to learn.

Some of the topics you'll definitely need to research are:

- variable assignment
- function definitions
- core data types: strings, integers, floats
- collection types: lists, tuples
- the system module (for accessing command-line arguments)
- the random module (for generating random numbers)

Where to Go From Here
==

> [action]
>
Finished already? If you still have time left, you can keep building off of this idea or try writing other programs that work with words, for example:

- String reversals: reverse words, sentences
- An interactive "mad libs" script
- Anagram generator
