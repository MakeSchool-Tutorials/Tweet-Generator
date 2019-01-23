---
title: Random Dictionary Words
slug: random-dictionary-words
---

Let's take a step back and remember what our ultimate goal is in this project: to generate new sentences using a body of text (a *corpus*) as the source. Seeing as that is the end goal, what is a good next logical step to move us in that direction?

We'll practice generating sentences from a body of text in a text file. The algorithm design is fairly straight-forward: read in a text file, select a random set of words from the file, and put those words together into a "sentence".

Here are some examples of this program in action:
```bash
$ python dictionary_words.py 3
wuss paragrammatist avitaminosis.

$ python dictionary_words.py 7
Nahuan semicynical Dendrobates madoqua semitangent rockstaff heptachronous.

$ python dictionary_words.py 4
polyoicous lupulin pennyrot boree.
```

We will make several assumptions to reduce the complexity of this program:
1. The program only accepts one argument: the number of words to be selected.
1. The only parameter to the program is the number of words, the rest of the variables like the file the words come from will be hard-coded.
1. The sentences do not have to make grammatical sense
1. The word order does not matter
1. Word selection can be completely random

The words will be chosen from a text file that is already on your computer and is already formatted nicely for parsing: the `words` file. This file is available on all Unix and Unix-like systems. On my MacBook running OS X Yosemite, this file is located at `/usr/share/dict/words`. Let's take a peek at this file:

```bash
$ wc /usr/share/dict/words
235886  235886 2493109 /usr/share/dict/words
```

The `wc` (word count) command shows lines, words, and bytes in a file. That's a lot of words. What does the end of the file look like?

```bash
$ tail -5 /usr/share/dict/words
zythem
Zythia
zythum
Zyzomys
Zyzzogeton
```

OK, so the file has one word per line, alphabetized from top to bottom.

> [info]
>
A Zyzzogeton is a "rare genus of leafhopper endemic to South America", in case you were wondering.

Your job is to use the words from this file to generate random "sentences". Of course, these aren't going to make much sense or be grammatically correct. Why is that, do you think? There's some good brain food for pondering in that question.

> [action]
>
> Write the **dictonary_words.py** script using the requirements above and add it to your tutorial repository
>
> **Commit your code:**
>

# Wait! How do I work with files in Python!?

No worries, it's not that hard to learn. Here are some resources:
- Read the [tutorial in the official docs](https://docs.python.org/2.7/tutorial/inputoutput.html)
- Read the [Working with File Objects section in Dive into Python](http://www.diveintopython.net/file_handling/file_objects.html)

Try doing small bits of file I/O before moving onto the `words` file since it is so big. A convenient approach is to create your own sample file formatted in the same way as `words` with a short list of 5-10 words.

Once you are ready to work with the `words` file, make sure to design the steps of your program in pseudocode before attempting it in Python. Then think about how you can break down these steps into their smallest component parts, and encapsulate these steps in functions.

Finally, you have a few data types to choose from when you need to store the words. If you're unfamiliar with Python, try starting with the list data type, which stores an ordered list of objects (like strings, numbers, etc.).

Where to Go From Here
==

> [challenge]
>
Finished already? If you still have time left, take your program further or explore other ideas, for example:
>
- Make it fast! Can you optimize this program to generate sentences more quickly? Make sure to benchmark your script before and after optimizations.
- Write your own vocabulary study game: flash a word, player has to guess the definition, then is shown the definition.
- Build an autocomplete program: given the first characters of a word, print a list of all possible words to spell from that base.
- Make a better anagram generator that only produces real words (i.e. must be included in the `words` file).
>
