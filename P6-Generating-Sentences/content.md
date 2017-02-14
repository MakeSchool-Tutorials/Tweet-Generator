---
title: Generating Sentences
slug: generating-sentences
---

As you probably observed with your stochastic sampler, building up sentences by randomly picking words according to their occurrence probabilities in a corpus doesn't give very convincing results. It's better than picking words randomly out of a dictionary in that rare words stay rare (Zyzzogeta are, alas, rarely mentioned on Twitter), but we're not doing anything to try to make *adjacent* words go well together. For example, suppose we were using the following (very small) corpus:

> *A man, a plan, a canal: Panama! A dog, a panic in a pagoda!*

With the stochastic sampler, the single most likely pair of words to be generated is "a a", a combination that basically never occurs in English. What's going on here is that the word "a" by itself is very common, but *given that the previous word was "a"*, it is very rare. This suggests that we can improve our sentence generator by keeping track of *context* when deciding what word to pick next.

## Markov Chains

One very popular way to do this is with a mathematical object called a *Markov chain*. A Markov chain consists of a bunch of *states* linked together by *transitions* telling you how likely it is to go from one state to another. We can visualize a Markov chain as a graph, where the vertices are states and the edges are transitions labelled with their probabilities. Here's a Markov chain learned from the corpus above, where each state corresponds to a token:

![example Markov chain](order-1-markov.svg)

Can you see where the probabilities came from? The token "a" occurs four times in the corpus, followed once each by "plan,", "canal:", "panic", and "pagoda!". So in the Markov chain, the state *a* has transitions leading to the states for those four tokens, each with probability 1/4.

To generate a sentence with a Markov chain, we perform a *random walk*: starting from some state, we pick a random transition (according to its probability), follow it, and repeat. Doing this we visit a sequence of states, which in our application corresponds to a sequence of tokens.

> [info]
>
Let's look at a concrete example. Say we start at state *A*. Our first random choice is between states *man* and *dog,*, each with probability 1/2 - say we take *dog,*. Now we don't have any choice, since there's only one transition out of state *dog,*, so we go to state *a*. Next we have four options with probability 1/4: *plan,*, *canal:*, *pagoda!*, and *panic*. Suppose our random choice is *plan,*, whose only outgoing transition sends us back to *a*. We have the same four options, and maybe a roll of the dice puts us in *plan,* again (there's nothing wrong with this - random choices in a Markov chain depend only on which state you're currently in, not which states you've visited in the past). We have to go back to *a*, and this time say we pick *canal:* from the four options. From there our only choice is to go to *Panama!*, which seems like a good place to stop. So our final sentence is "A dog, a plan, a plan, a canal: Panama!". Not very profound, but at least it's grammatical.

Note how different this way of generating sentences is from the old stochastic sampler: the probability distribution we use at each step depends on which state we're at, i.e. which word we last generated. Since the pair of tokens "a a" never occurs in our corpus, the state *a* doesn't have a transition back to itself, and so the probability of generating "a a" is *zero*.

> [action]
>
Now let's try implementing this! You'll need to write code to do two things:
>
1. *Learn a Markov chain from a corpus.* You've already written code to find how often a token appears in a corpus, but now you need to find how often a token appears *after* another token.
>
2. *Do a random walk on a Markov chain.* This should be pretty simple if you pick a good way to store the Markov chain you learn.
>
Make sure to think about what data structures to use to make your code efficient. Both learning the Markov chain and taking a step of the random walk should take time at most linear in the size of the corpus.
>
When you're ready, try your implementation out on a real corpus and see how it compares to the stochastic sampler!

<!-- html comment to break boxes -->

> [info]
>
While writing and testing your random walk implementation you might come up against a couple of points we glossed over above.
>
1. *Where to start?* Obviously you might get different results depending on how you pick a state to start your walk from. Are there some states which particularly make sense in this application? Feel free to try out different possibilities and see how it affects your results!
>
2. *What if I get stuck?* Take a look at the state *pagoda!* in the Markov chain above. Since this corresponds to the last token in the corpus, it doesn't have any outgoing transitions, and so your random walk will get stuck if it enters that state. You could of course just stop there, but what should you do if you want to generate more text? Many options are fine here (as long as your code doesn't crash!), but before doing extensive experiments you may want to think about how often this situation will arise given a large corpus.
