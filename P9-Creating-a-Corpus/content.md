---
title: Creating a Corpus
slug: creating-a-corpus
---

The time has come to create our *corpus*: the body of text whose style and tone we want our sentence-generation bot to imitate. In computational linguistics, a set of documents with example text is called a corpus.

## Choosing a Style

First you will need to think of a style of writing we want your Twitter bot to imitate. For example: recruiter emails, biz-dev buzzwords, political writing, new age meditations, or artist's statements.

Next, find a website or other source that has a large quantity of this kind of text or style of writing. The source you find should have at least 20,000 words (ideally, well over 100,000 words) so that there are enough words and sentences to train a language model on this writing style.

If you choose a style that contains many specialized/jargon words or a large vocabulary, you will need more total text to learn from.

As alternatives, you could use copyright-free books (like those on [Project Gutenberg](https://www.gutenberg.org/)) in a similar style, or the blog from a prolific writer.

The important thing to consider is that there is (a) enough text, and (b) that the style of writing is consistent. A collection of short stories from different authors, for example, won't be able to pick up on the writing patterns of any given author. On the other end of the spectrum would be *Shakespeare*, or even something like *The Economist*, which have distinctive writing and editorial styles.

> [action]
>
Ok, now go explore the Web and find your corpus!

Corpus Collection
==
If your corpus is not already in a [plain text](https://en.wikipedia.org/wiki/Plain_text) format, you will have to do some work to convert it to plain text before you can start parsing it.

Until recently, collecting text for language processing was an arduous task. Fortunately, now there's a content extraction service called [Diffbot](https://www.diffbot.com/) that does the hard part of downloading web pages and removing HTML tags and extraneous content to extract the relevant text.

You need only give it a set of URLs and Diffbot produces a nice, clean set of text-only documents. It's important to download and save the corpus because we'll be parsing it several times throughout project development as we make our language model more sophisticated.

Let's use Diffbot to compile our corpus.

1. Create a new [Diffbot account](https://www.diffbot.com/plans/trial) (the trial plan gives you 10,000 free API calls).
2. Save the URLs for each webpage in your corpus on separate lines of a text file (call it `pages.txt`).
3. Write a script that reads `pages.txt` and sends a request to the [Diffbot Article API](https://www.diffbot.com/dev/docs/article/), then writes the full text from the response to a plain text file.
4. After testing your script on a single URL, it should keep downloading text from URLs until it includes all the text from your source articles.

Once you've done that, run the [word count command](https://en.wikipedia.org/wiki/Wc_(Unix)) to make sure that your corpus has enough words in it:

	$ wc -w Corpus/RonPaul/Text/*.txt
	...
	445244 total

If you don't have enough, you'll need to go find more articles or books from additional sources.


> [action]
>
Got it? Ok, go write a corpus collection script!

Help! How do I use an HTTP API?
==
If you've never used a web API before, no worries. It is a big topic, and one that (at least for now) you don't need to learn too much about to complete this step of the project. Every API is different, so even experienced developers still have to start by reading the docs.

The key concept behind an API is that you are writing a program to send an HTTP *request* (like the one you send whenever you visit a web page or submit a form in your browser) to a particular URL, and then using the data sent in the *response* to do whatever you want.

In the case of Diffbot, you need to send a request to their Article API URL (`http://api.diffbot.com/v3/article`), passing your developer token and the URL of the web page to scrape. Read the [Diffbot Article API docs](https://www.diffbot.com/dev/docs/article/) to learn all about it.

You can try this out using your regular web browser! Just copy and paste this URL into your browser (replacing the `yourtokenhere` with your Diffbot developer token, which would have been emailed to you when you signed up):

	http://api.diffbot.com/v3/article?token=your_token_here&url=https://en.wikipedia.org/wiki/Twitterbot

If the request worked, you probably saw a big chunk of unparsed JSON (*hint: use an in-browser JSON prettifier like [https://github.com/callumlocke/json-formatter](https://github.com/callumlocke/json-formatter) to make it easier to read*).

You could do this manually for each of your URLs, copying out the contents of the `text` field in the response, but that's boring. And tedious. And not how a programmer would solve this problem! Instead, you should write a script.

There are two different approaches to this:

1. Use the [Diffbot Python client library](https://github.com/diffbot/diffbot-python-client)
2. Use the [Diffbot Article API](https://www.diffbot.com/dev/docs/article/) directly

In either case, it just takes a little time to read the documentation and get started. If you want to use the Diffbot Web API directly, [Requests](http://requests.readthedocs.org/en/latest/) is a great HTTP library for Python that you can use to send HTTP requests and even convert the JSON response to a Python dictionary!

Here's an example script that does the job (note that it appends all the source article text to a single `corpus.txt` file):

> [solution]
>
	import requests
>
	DIFFBOT_API_URL = 'http://api.diffbot.com/v3/article'
	DIFFBOT_DEV_TOKEN = 'your_diffbot_dev_token'
>
	def get_article(article_url):
	    # set request params for API request
	    params = { 'token': DIFFBOT_DEV_TOKEN,
	               'url': article_url,
	               'discussion': 'false' }
>
	    res = requests.get(DIFFBOT_API_URL, params) # hit the Diffbot API
	    res_obj = res.json()['objects'][0]          # parse the response object
>
	    return res_obj['text']                      # pull out the text
>
	if __name__ == '__main__':
	    import sys
	    urls_file = open(sys.argv[1])
	    output_file = open('corpus.txt', 'w')
>
	    corpus = ''
>
	    for line in urls_file:
	        url = line.strip() # remove leading/trailing whitespace
	        article = get_article(url)
	        corpus += article
>
	    output_file.write(corpus)
	    print('Corpus saved to {}'.format(output_file.name))
