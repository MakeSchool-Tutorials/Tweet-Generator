---
title: Markov Chains Revisited
slug: markov-chains-revisited
---

Let's take a look at another tiny corpus:

> *I went left, you went right, I went left, I went right,*

The Markov chain we get looks like this:

![another example Markov chain](order-1-markov2.svg)

By taking context into account, the Markov chain eliminates many nonsensical possibilities: "went left," and "went right," are okay, but "went I" and "went you" have zero probability. However, some patterns in the text are not reflected in the Markov chain. For example, the corpus seems to say that "I" favor going left and "you" favor going right, but in the Markov chain "I went left" and "I went right" are equally likely, as are "you went left" and "you went right". The reason is of course that distinguishing between these requires looking back *two* words, while the Markov chain only keeps track of *one*.

## Second-Order Markov Chains

As you might expect, there's nothing preventing us from keeping track of more context. The Markov chains we've seen so far are called *first-order*, because their transition probabilities are functions only of *one* state: the current one. In a *second-order* Markov chain, the transitions depend not only on the current state, but on the previous one too. Alternatively, you can think of a second-order Markov chain as a first-order one where the states are labelled with *pairs* of tokens, so that a single state has all the information needed to work out the transitions. This perspective lets us view second-order Markov chains as graphs, like before:

![example second-order Markov chain](order-2-markov.svg)

In this figure every state is labelled with two tokens: the one on the bottom is the token we have just seen (or generated), and the one above is the token that came before it (we've put them in this order so that reading the labels top-down reflects their order in the corpus).

How are the transition probabilities determined for a second-order Markov chain? It's a similar process to what we saw before. Then, we counted how often every pair of tokens occurred, so that given the current token we could tell how likely another was to follow. Now, we need to count *triples* of tokens: "I went left," occurs twice and "I went right," only once, so the state *I went* should transition to *went left,* with probability 2/3 and to *went right,* with probability 1/3. Take another look at the figure and make sure you understand where the probabilities are coming from.

> [info]
>
It may be confusing that state *I went* leads to *went left,*, which would seem to make us generate the token "went" twice. The point to remember is that now that states are labelled with pairs of tokens, only the second token is the one we output when we enter a state: the first one is there just so that we can "remember" what the previous token was. We need that information, since the whole point of a second-order Markov chain is that the transition probabilities can depend on it. Just like how an ordinary Markov chain let us pick tokens with different probabilities depending on what the last word we generated was (vs. the stochastic sampler that used the same probabilities every time), a second-order Markov chain lets us use different probabilities depending on what the *two* previous words were. So states need to be labelled with two tokens, but we'll only output one.

## To Infinity and Beyond (Well, Not Really)

Second-order Markov chains are even better than first-order ones at modeling text because they use more context, but there's no reason to stop there. Unsurprisingly, an *nth-order Markov chain* is just a Markov chain that keeps track of *n* previous tokens. Such a chain gives you a probability distribution for the next token as a function of the tuple of *n* previous words - often called an *n-gram*. So to learn an nth-order Markov chain from a corpus, we have to count how often each n-gram is followed by each token.

> [action]
>
Clearly using higher order Markov chains can give you better results (think of how many words can plausibly follow the 1-gram "my" vs. the 3-gram "I'm debugging my"), but what is the cost? Work out asymptotically how many n-grams there can be in a corpus with *w* distinct tokens, and how much space it could take to store an nth-order Markov chain learned from the corpus.

## Implementation Issues

Let's try to extend your first-order Markov chain implementation to handle higher-order chains. Hopefully, a fair amount of your code will work with only minor changes: like in the second-order case, we can think of an nth-order chain as a first-order chain whose states are labelled by n-tuples. So in particular your random walk code, which just looks up transition probabilities and moves according to them, should still work.

Computing the transition probabilities is more involved, though. Before, you could just sweep through the corpus, updating a mapping (in some appropriate data structure that will remain nameless) from tokens to the frequencies of following tokens. You can still scan through the corpus, but the input to that map is now the tuple of the last *n* tokens seen. A simple way to get that tuple is just to back up *n* tokens and read them in again, but let's try something else to use an appropriate data structure.

What we'd like to do is maintain a list of the previous *n* tokens in order, removing the oldest one when we advance a step and adding the new one we just read. One data structure that would work here is a *queue*. As the names suggest, elements added to a queue have to wait their turn, only being removed when all elements ahead of them have been. Sometimes a queue is called a FIFO (first-in, first-out) buffer. Queues typically support the following operations:

- **enqueue** an item (add it to the back of the queue)
- **dequeue** the item at the front of the queue (remove and return it)
- **iterate** over items in the queue, from front to back

> [info]
>
Let's look at an example. We'll write the contents of the queue as a list, although the queue itself might not be implemented as one. When the queue is created, it is empty: `[]`. If we then enqueue `'A'`, `'B'`, and `'C'`, the queue will be `['A' -> 'B' -> 'C']`. Doing a dequeue, the first element that was enqueued will be removed, yielding `['B' -> 'C']`. Finally, enqueuing `D` gives us `['B' -> 'C' -> 'D']`.


Implement a queue supporting the operations above. This should be straightforward given the data structures you've already implemented. Enqueuing and dequeuing should take constant time, and iteration should take constant time per item. Make sure your implementation works correctly if someone tries to dequeue an item from an empty queue: it should raise an exception that explains the problem.

> [action]
>
Using your queue implementation, extend your Markov chain learner and sentence generator to use nth-order chains. Try different values of *n* and see how the quality of your generated sentences changes. Also experiment to see how *n* affects the runtime and memory consumption of your code - are your observations in line with the asymptotic analysis you did above?

Where to Go from Here
==

Finished early? Then enjoy another helping of data structure challenges.

Ordinary queues don't place restrictions on how many items they can store - they can grow or shrink as needed. However, notice that in the case of Markov chains, we know ahead of time that we'll never need to store more than *n* items in the queue. This means we can use a simpler data structure called a [*circular buffer*](https://en.wikipedia.org/wiki/Circular_buffer) (or *ring buffer*). Circular buffers are created with a fixed size - once they fill up, any further additions will overwrite values in the buffer, starting with the oldest. If you're interested, try implementing a circular buffer, which can be done with and without using a linked list.

Think queues are cool? Try implementing a *deque* (pronounced deck): a double-ended queue where elements can be added and removed from either end in constant time.

If that bores you, here's a classic interview question: how can you implement a doubly-linked list with the `Node` class for a singly-linked list, unmodified? (And no, you can't assume some extra-wide field that you can cram two node references into; you only have space to store one reference.) Your implementation needs to support constant-time adding and removing elements on either end of the list, and linear-time iteration from the front to the back and vice versa, but you don't need to support modifications to the middle of the list.
