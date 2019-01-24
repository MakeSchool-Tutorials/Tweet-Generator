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
1. All parameters except the number of words will be hard-coded.
1. We will use the `words` file which is available on all Unix systems for our list of words.
1. The sentences do not have to make grammatical sense.
1. Word selection can be completely random and the word order does not matter.

> [action]
> Locate the `words` file on your computer. On a Macbook running OS X Yosemite, this file is located at `/usr/share/dict/words`.
>
> Use the `wc` (word count) command to show the number of lines, words, and bytes in the file.
>
```bash
$ wc /usr/share/dict/words
235886  235886 2493109 /usr/share/dict/words
```
> Use the `tail` function to see what the end of the file looks like.
>
```bash
$ tail -5 /usr/share/dict/words
zythem
Zythia
zythum
Zyzomys
Zyzzogeton
```
>
A Zyzzogeton is a "rare genus of leafhopper endemic to South America", in case you were wondering.

The file is nicely formatted with one word per line, alphabetized from top to bottom.  Your job is to use the words from this file to generate random "sentences". Of course, these aren't going to make much sense or be grammatically correct but that is fine for now.

**Wait! How do I work with files in Python!?**

No worries, it's not that hard to learn. Here are some resources:

- Read the [tutorial in the official docs](https://docs.python.org/2.7/tutorial/inputoutput.html)
- Read the [Working with File Objects section in Dive into Python](http://www.diveintopython.net/file_handling/file_objects.html)

Try doing small bits of file I/O before moving onto the `words` file since it is so big. A convenient approach is to create your own sample file formatted in the same way as `words` with a short list of 5-10 words.

Once you are ready to work with the `words` file, make sure to design the steps of your program in pseudocode before attempting it in Python. Then think about how you can break down these steps into their smallest component parts, and encapsulate these steps in functions.

Finally, you have a few data types to choose from when you need to store the words. If you're unfamiliar with Python, try starting with the list data type, which stores an ordered list of objects (like strings, numbers, etc.).

> [action]
>
> Write the **dictonary_words.py** script:
>
- read in the `words` file
- select a random set of words from the file and store in a data type
- put the number of words requested together into a "sentence"
- output your sentence

>
> **Commit your code.**
>


Where to Go From Here
==

> [challenge]
>
> Finished already? If you still have time left, take your program further or explore other ideas, for example:
>
> - Make it fast! Can you optimize this program to generate sentences more quickly? Make sure to benchmark your script before and after optimizations.
- Write your own vocabulary study game: flash a word, player has to guess the definition, then is shown the definition.
- Build an autocomplete program: given the first characters of a word, print a list of all possible words to spell from that base.
- Make a better anagram generator that only produces real words (i.e. must be included in the `words` file).
