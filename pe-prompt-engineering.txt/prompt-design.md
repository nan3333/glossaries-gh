
# Table of Contents

1.  [Prompt Engineering 101](#org5bede01)
    1.  [Introduction](#orgc675b6c)
        1.  [Key concepts](#orgb0a2289)
        2.  [Prompt Design 101](#org746d60b)
        3.  [Prompt basics](#org9fcbb85)
        4.  [Prompt troubleshooting](#org4ebe9fb)
        5.  [Classification](#org248bfc6)
        6.  [Improving the classifier’s efficiency](#org02492f9)
    2.  [Generation](#orgaaaa4e1)
        1.  [Advanced generation techniques](#orgede266a)
    3.  [Conversation](#org7f0a645)
    4.  [Transformation](#org4f492af)
        1.  [Translation](#org7de5ddd)
        2.  [Conversion](#orgc9aa09d)
    5.  [Summarization](#org9ce6dd2)
    6.  [Completion](#org21fec23)
    7.  [Factual responses](#orgf20dd99)
    8.  [Semantic search](#orgccac893)


<a id="org5bede01"></a>

# Prompt Engineering 101


<a id="orgc675b6c"></a>

## Introduction

Our API provides a general-purpose “text in,
text out” interface, which makes it possible
to apply it to virtually any language task.
This is different from most other language
APIs, which are designed for a single task,
such as sentiment classification or named
entity recognition.

To use the API, you simply give it a text
prompt (the text-based input or "instructions"
you provide to the API) and it will return a
text completion, attempting to match the
context or pattern you gave it. You can
“program” it by crafting a description or
writing just a few examples of what you’d like
it to do. Its success generally varies
depending on how complex the task is. A good
rule of thumb is thinking about how you would
write out a word problem for a middle schooler
to solve.

Keep in mind that the models' training data
cuts off in October 2019, so they may not have
knowledge of current events. We plan to add
more continuous training in the future. 


<a id="orgb0a2289"></a>

### Key concepts

There are three concepts that are core to the API: prompt, completion, and
tokens. The “prompt” is text input to the API, and the “completion” is the text
that the API generates based on the prompt. For example, if you give the API
the prompt, “As Descartes said, I think, therefore”, it will return the
completion “ I am” with high probability. It’s worth noting that the API is
stochastic by default, meaning that every time you call it you might get a
slightly different completion, even if the prompt stays the same.

The best way to get started exploring the API is with the Playground. It's
simply a text box where you write the prompt and click the "Submit" button to
generate the completion. Try it yourself, go to the playground and type in:

    1  As Descartes said, I think therefore

And then click Submit, and you’ll see
something like the following (the completion
is highlighted):

    1  As Descartes said, I think therefore I am

You’ll probably see a few more words than that
being generated, since the default response
length setting for the playground is much
higher. “Tokens”, which can be thought of as
pieces of words. (Much like a jigsaw puzzle,
the pieces are not cut up nicely according to
the actual images displayed). The API turns
text into tokens before processing it, as a
trick to be able to handle more text at once.
For example, the word “Descartes” gets broken
up into the tokens “Desc”, “art” and “es”,
while a short and common word like “pear” is a
single token. One thing you’ll notice is that
many tokens start with a whitespace, for
example “ hello” and “ bye”.

One limitation to keep in mind is that
combined, the text prompt and generated
completion must be below 2048 tokens (roughly
~1500 words).

The API runs models with weights from the
GPT-3 family with many speed and throughput
improvements. We currently offer the following
models: davinci, curie, babbage and ada. The
models provide a spectrum of capability, where
davinci is the most capable model and ada is
the fastest. We recommend you use davinci when
you're experimenting, since it will give the
best completions. Once you’re ready to move
your use case to production, we encourage you
to try the other models to see if you get the
same results but with lower latency. You can
use the engine comparison tool to get a better
sense of how the different models perform.


<a id="org746d60b"></a>

### Prompt Design 101

The API is capable of doing a variety of
different tasks. The prompt is the text you
send to the API to get it to generate a
response. The response, called a “completion”,
is what the API thinks is the logical
continuation of the prompt. A well-written
prompt provides enough information for the API
to know what you are asking for and how it
should respond.

The best way to learn how to use it and find
inspiration is to look at different examples
and see how they work.

These examples in this document include links
that open up Playground demonstrations where
you can interact and experiment with changing
their contents. You can also click on the
categories in the table below that jump to the
section that describe how to create prompts
for those tasks.

-   Classification
    -   Tweet Sentiment
    -   Company categorization
    -   Labeling parts of speech

-   Generation
    -   Idea Generator

-   Conversation
    -   Q&A agent
    -   Sarcastic chatbot

-   Transformation
    -   Summarize Text
    -   English -> French
    -   Movie Titles -> Emoji

-   Completion
    -   Generate React components

-   Factual Responses
    -   Provide factual answers


<a id="org9fcbb85"></a>

### Prompt basics

The API can do everything from generate
original stories to perform complex text
analysis. Because it can do so many things you
have to be explicit in showing it what you
want.

Showing, not just “telling”, is the secret to
a good prompt. Even people familiar with ML
accustomed to chatbots and single-purpose text
models can get confused by this. The API’s
power is its adaptability. The key to
unlocking this adaptability is in learning how
to show it what you want.

The API tries to guess what you want from the
prompt. If you send it the words “Give me a
list of cat breeds,” the API wouldn’t
automatically assume that you’re asking for a
list of cat breeds. You could just as easily
be asking the API to continue a conversation
where the first words are “Give me a list of
cat breeds” and the next ones are “and I’ll
tell you which ones I like.” If the API only
assumed that you wanted a list of cats it
wouldn’t be as good at content creation,
classification or other tasks.

There are three simple guidelines to creating prompts:

1.  Show and tell Make it clear to the API what
    you want either through instructions, examples
    or a combination of the two. If you want the
    API to rank a list of items in alphabetical
    order or to classify a paragraph by sentiment,
    show it that’s what you want.
2.  Provide quality data If you’re trying to
    build a classifier or get the API to follow a
    pattern, make sure that there are enough
    examples. Proofread your examples and check
    that it’s clear that there’s enough data for
    the API to create a response. The API is
    usually smart enough to see through basic
    spelling mistakes and give you a response, but
    it also might assume this is intentional and
    it can affect the response.
3.  Check your settings The temperature and
    top<sub>p</sub> settings control how deterministic the
    API is in generating a response. If you’re
    asking the API to provide you with a response
    where there’s only one right answer, then
    you’d want to set these lower. If you’re
    looking for a response that’s not obvious,
    then you might want to set them higher. The
    number one mistake people use with these
    settings is assuming that they’re “cleverness”
    or “creativity” controls.


<a id="org4ebe9fb"></a>

### Prompt troubleshooting

-   If you’re having trouble getting the API to perform as expected, follow this checklist:
    -   Is it clear what the intended generation should be?
    -   Are there enough examples?
    -   Did you check your examples for mistakes? (The API won’t tell you directly)
    -   Are you using temp and top<sub>p</sub> correctly?


<a id="org248bfc6"></a>

### Classification

To create a text classifier with the API we
provide a description of the task and provide
a few examples. In this demonstration we show
the API how to classify the sentiment of
Tweets.

     1  This is a tweet sentiment classifier
     2  Tweet: "I loved the new Batman movie!"
     3  Sentiment: Positive
     4  ###
     5  Tweet: "I hate it when my phone battery dies."
     6  Sentiment: Negative
     7  ###
     8  Tweet: "My day has been 👍"
     9  Sentiment: Positive
    10  ###
    11  Tweet: "This is the link to the article"
    12  Sentiment: Neutral
    13  ###
    14  Tweet: "This new music video blew my mind"
    15  Sentiment:

It’s worth paying attention to several
features in this example:

1.  State what the prompt does at the start At the start of the example we state
    in plain language what the classifier does: “This is a tweet sentiment
    classifier.” By stating this up front it helps the API understand much more
    quickly what the goal of the response is supposed to be and you’ll end
    needing to provide fewer examples.
2.  Use plain language to describe your inputs and outputs We use plain language
    for the input “Tweet” and the expected output “Sentiment.” For best practices,
    start with plain language descriptions. While you can often use shorthand
    or keys to indicate the input and output, when building your prompt it’s
    best to start by being as descriptive as possible and then working backwards
    removing extra words as long as the performance to the prompt is consistent.
3.  Use separators to define your examples We use “###” as a separator between
    examples. You can use other characters or line breaks, but “###” works
    pretty consistently and is also an easy to use stop sequence. Whatever
    separator you use, make sure that it’s clear to the API where an example
    starts and stops.
4.  Show the API how to respond to any case In this example we provide multiple
    outcomes “Positive”, “Negative” and “Neutral.” A neutral outcome is important
    because there will be many cases where even a human would have a hard time
    determining if something is positive or negative and situations where it’s
    neither.
5.  You can use text and emoji The classifier is a mix of text and emoji 👍. The
    API reads emoji and can even convert expressions to and from them.
6.  You need fewer examples for familiar tasks For this classifier we only
    provided a handful of examples. This is because the API already has an
    understanding of sentiment and the concept of a tweet. If you’re building a
    classifier for something the API might not be familiar with, it might be
    necessary to provide more examples.


<a id="org02492f9"></a>

### Improving the classifier’s efficiency

Now that we have a grasp of how to build a
classifier, let's take that example and make
it even more efficient so that we can use it
to get multiple results back from one API
call.

     1  This is a tweet sentiment classifier
     2  Tweet: "I loved the new Batman movie!"
     3  Sentiment: Positive
     4  ###
     5  Tweet: "I hate it when my phone battery dies"
     6  Sentiment: Negative
     7  ###
     8  Tweet: "My day has been 👍"
     9  Sentiment: Positive
    10  ###
    11  Tweet: "This is the link to the article"
    12  Sentiment: Neutral
    13  ###
    14  Tweet text
    15  
    16  1. "I loved the new Batman movie!"
    17  2. "I hate it when my phone battery dies"
    18  3. "My day has been 👍"
    19  4. "This is the link to the article"
    20  5. "This new music video blew my mind"
    21  
    22  Tweet sentiment ratings:
    23  1: Positive
    24  2: Negative
    25  3: Positive
    26  4: Neutral
    27  5: Positive
    28  
    29  ###
    30  Tweet text
    31  
    32  "I can't stand homework"
    33  "This sucks. I'm bored 😠"
    34  "I can't wait for Halloween!!!"
    35  "My cat is adorable ❤️❤️"
    36  "I hate chocolate"
    37  Tweet sentiment ratings:
    38  1.

After showing the API how tweets are
classified by sentiment we then provide it a
list of tweets and then a list of sentiment
ratings with the same number index. The API is
able to pick up from the first example how a
tweet is supposed to be classified. In the
second example it sees how to apply this to a
list of tweets. This allows the API to rate
five (and even more) tweets in just one API
call.

It’s important to note that when you ask the
API to create lists or evaluate text you need
to pay extra attention to your probability
settings (Top P or Temperature) to avoid
drift.

Make sure your probability setting is
calibrated correctly by running multiple
tests.

Don’t make your list too long or the API is
likely to drift.


<a id="orgaaaa4e1"></a>

## Generation

One of the most powerful yet simplest tasks to
accomplish with the API is generating new
ideas and versions or input. You can give the
API a list of story ideas and it will add to
that list from just a few examples. It can
create business plans, character descriptions
and marketing slogans just by providing it a
handful of examples. In this demonstration
we’ll use the API to create more examples for
how to use virtual reality in the classroom:

    1  Ideas involving education and virtual reality
    2  
    3  Virtual Mars
    4  Students get to explore Mars via virtual reality and go on missions to collect and catalog what they see.
    5  
    6  2.

All we had to do in this example is provide
the API with just a description of what the
list is about and one example. We then
prompted the API with the number two
indicating that it’s a continuation of the
list.

Although this is a very simple prompt, there
are several details worth noting:

1.  We explained the intent of the list Just like with the classifier, we tell
    the API up front what the list is about. This helps it focus on completing the
    list and not trying to guess what the pattern is behind it.
2.  Our example sets the pattern for the rest of the list Because we provided a
    one-sentence description, the API is going to try to follow that pattern for
    the rest of the items it adds to the list. If we want a more verbose response
    we need to set that up from the start.
3.  We prompt the API by adding an incomplete entry When the API sees “2.” and
    the prompt abruptly ends, the first thing it tries to do is figure out what
    should come after it. Since we already had an example with number one and gave
    the list a title, the most obvious response is to continue adding items to the
    list.


<a id="orgede266a"></a>

### Advanced generation techniques

You can improve the quality of the responses by
making a longer more diverse list in your prompt. One way to do that is to
start off with one example, let the API generate more and select the ones that
you like best and add them to the list. A few more high-quality variations can
dramatically improve the quality of the responses.


<a id="org7f0a645"></a>

## Conversation

The API is extremely adept at carrying on conversations with humans and even
with itself. With just a few lines of instruction the API can perform as a
customer service chatbot that intelligently answers questions without ever
getting flustered or a wise-cracking conversation partner that makes jokes and
puns. The key is to tell the API how it should behave and then provide a few
examples.

Here’s an example of the API playing the role of an AI answering questions:

    1  The following is a conversation with an AI assistant. The assistant is helpful, creative, clever, and very friendly.
    2  Human: Hello, who are you?
    3  AI: I am an AI created by OpenAI. How can I help you today?
    4  Human:

This is all it takes to create a chatbot
capable of carrying on a conversation. But
underneath its simplicity there are several
things going on that are worth paying
attention to:

1.  We tell the API the intent but we also tell it how to behave Just like the
    other prompts, we cue the API into what the example represents, but we also add
    another key detail: we give it explicit instructions on how to interact with
    the phrase “The assistant is helpful, creative, clever, and very friendly.”
    
    Without that instruction the API might stray and mimic the human it’s
    interacting with and become sarcastic or some other behavior we want to
    avoid.

2.  We give the API an identity At the start we have the API respond as an AI
    that was created by OpenAI. While the API has no intrinsic identity, this helps
    it respond in a way that’s as close to the truth as possible. You can use
    identity in other ways to create other kinds of chatbots. If you tell the API
    to respond as a woman who works as a research scientist in biology, you’ll get
    intelligent and thoughtful comments from the API similar to what you’d expect
    from someone with that background.
    
    In this example we create a chatbot that is a bit sarcastic and reluctantly
    answers questions:

     1  Marv is a chatbot that reluctantly answers questions.
     2  ###
     3  User: How many pounds are in a kilogram?
     4  Marv: This again? There are 2.2 pounds in a kilogram. Please make a note of this.
     5  ###
     6  User: What does HTML stand for?
     7  Marv: Was Google too busy? Hypertext Markup Language. The T is for try to ask better questions in the future.
     8  ###
     9  User: When did the first airplane fly?
    10  Marv: On December 17, 1903, Wilbur and Orville Wright made the first flights. I wish they’d come and take me away.
    11  ###
    12  User: Who was the first man in space?
    13  Marv:

To create an amusing and somewhat helpful
chatbot we provide a few examples of questions
and answers showing the API how to reply. All
it takes is just a few sarcastic responses and
the API is able to pick up the pattern and
provide an endless number of snarky responses.


<a id="org4f492af"></a>

## Transformation

The API is a LM that is familiar with a
variety of ways that words and characters can
be used to express information. This ranges
from NL text to computer code and languages
other than English. The API is also able to
understand content on a level that allows it
to summarize, convert and express it in
different ways.


<a id="org7de5ddd"></a>

### Translation

In this example we show the API how to convert
from English to French:

    1  English: I do not speak French.
    2  French: Je ne parle pas français.
    3  English: See you later!
    4  French: À tout à l'heure!
    5  English: Where is a good restaurant?
    6  French: Où est un bon restaurant?
    7  English: What rooms do you have available?
    8  French: Quelles chambres avez-vous de disponible?
    9  English:

This example works because the API already has
a grasp of French, so there’s no need to try
to teach it this language. Instead, we just
need to provide enough examples that API
understands that it’s converting from one
language to another.

If you want to translate from English to a
language the API is unfamiliar with you’d need
to provide it with more examples and a fine-
tuned model to do it fluently.


<a id="orgc9aa09d"></a>

### Conversion

In this example we convert the name of a movie
into emoji. This shows the adaptability of the
API to picking up patterns and working with
other characters.

    1  Back to Future: 👨👴🚗🕒
    2  Batman: 🤵🦇
    3  Transformers: 🚗🤖
    4  Wonder Woman: 👸🏻👸🏼👸🏽👸🏾👸🏿
    5  Spider-Man: 🕸🕷🕸🕸🕷🕸
    6  Winnie the Pooh: 🐻🐼🐻
    7  The Godfather: 👨👩👧🕵🏻‍♂️👲💥
    8  Game of Thrones: 🏹🗡🗡🏹
    9  Spider-Man:


<a id="org9ce6dd2"></a>

## Summarization

The API is able to grasp the context of text
and rephrase it in different ways. In this
example the API takes a block of text and
creates an explanation a child would
understand. This illustrates that the API has
a deep grasp of language.

     1  My ten-year-old asked me what this passage means:
     2  """
     3  A neutron star is the collapsed core of a
     4  massive supergiant star, which had a total
     5  mass of between 10 and 25 solar masses,
     6  possibly more if the star was especially
     7  metal-rich.[1] Neutron stars are the smallest
     8  and densest stellar objects, excluding black
     9  holes and hypothetical white holes, quark
    10  stars, and strange stars.[2] Neutron stars
    11  have a radius on the order of 10 kilometres
    12  (6.2 mi) and a mass of about 1.4 solar
    13  masses.[3] They result from the supernova
    14  explosion of a massive star, combined with
    15  gravitational collapse, that compresses the
    16  core past white dwarf star density to that of
    17  atomic nuclei.
    18  """
    19  I rephrased it for him, in plain language a ten-year-old can understand:
    20  """

In this example we place whatever we want
summarized between the triple quotes. It’s
worth noting that we explain both before and
after the text to be summarized what our
intent is and who the target audience is for
the summary. This is to keep the API from
drifting after it processes a large block of
text.


<a id="org21fec23"></a>

## Completion

While all prompts are forms of completions it
can be helpful to think of text completion as
its own task in instances where you want the
API to pick up where you left off. Examples of
this include helping you write a summary or
writing lines of code where the API can infer
what should come next. In this prompt the API
will help write React components by completing
the code that’s sent to the API:

     1  ''''
     2  import React from 'react';
     3  const ThreeButtonComponent=()=>(
     4  <div>
     5  <p>Button One<p>
     6  <button className="button-green" onClick={this.handleButtonClick}>Button One</button>
     7  <p>Button Two</p>
     8  <button className="button-green" onClick={this.handleButtonClick}>Button Two</button>
     9  <p>Button Three</p>
    10  <button className="button-green" onClick={this.handleButtonClick}>Button Three</button>
    11  </div>
    12  )
    13  ''''
    14  import React from 'react';
    15  const HeaderComponent=()=>(

The API already has an understanding of the
React library and is able to continue the rest
of the code once it has an example of what the
user is trying to create.

In this next prompt the API is able to
determine the intent of the writer and help
continue the train of thought about vertical
farming. It’s also an example of where the
probability setting will keep the API focused
on the intent of the prompt or let it go off
on a tangent.

    1  Vertical farming provides a novel solution for producing food locally, reducing transportation costs and


<a id="orgf20dd99"></a>

## Factual responses

The API has a lot of knowledge that it’s
learned from the data that it was been trained
on. It also has the ability to provide
responses that sound very real but are in fact
made up. There are two ways to limit the
likelihood of the API making up an answer.

1.  Provide a ground truth for the API.
    If you provide the API with a body of text
    to answer questions about (like a Wikipedia
    entry) it will be less likely to
    confabulate a response.
2.  Use a low probability and show the API how to say “I don’t know”.
    If the API understands that in cases where
    it’s less certain about a response that
    saying “I don’t know” or some variation is
    appropriate, it will be less inclined to
    make up answers.

In this example we give the API examples of
questions and answers it knows and then
examples of things it wouldn’t know and
provide question marks. We also set the
probability to zero so the API is more likely
to respond with a “?” if there is any doubt.

     1  Q: Who is Batman?
     2  A: Batman is a fictional comic book character.
     3  ###
     4  Q: What is torsalplexity?
     5  A: ?
     6  ###
     7  Q: What is Devz9?
     8  A: ?
     9  ###
    10  Q: Who is George Lucas?
    11  A: George Lucas is American film director and producer famous for creating Star Wars.
    12  ###
    13  Q: What is the capital of California?
    14  A: Sacramento.
    15  ###
    16  Q: What orbits the Earth?
    17  A: The Moon.
    18  ###
    19  Q: Who is Fred Rickerson?
    20  A: ?
    21  ###
    22  Q: What is an atom?
    23  A: An atom is a tiny particle that makes up everything.
    24  ###
    25  Q: Who is Alvan Muntz?
    26  A: ?
    27  ###
    28  Q: What is Kozar-09?
    29  A: ?
    30  ###
    31  Q: How many moons does Mars have?
    32  A: Two, Phobos and Deimos.
    33  ###
    34  Q:


<a id="orgccac893"></a>

## Semantic search

The API lets you do semantic search over
documents. This means that you can provide a
query, such as a NL question or a statement,
and find documents that answer the question or
are semantically related to the statement. The
“documents” can be words, sentences,
paragraphs or even longer documents. For
example, if you provide documents ["White
House", "hospital", "school"] and query "the
president", you’ll get a different similarity
score for each document. The higher the
similarity score, the more semantically
similar the document is to the query (in this
example, “White House” will be most similar to
“the president”).

Each search query produces a different
distribution of scores for a fixed group of
documents. For instance, if you have a group
of documents that are summaries of books, the
query "sci-fi novels" might have a mean score
of 150 and standard deviation of 50, whereas
the query "cat training" might have a mean
score of 200 and standard deviation of 10, if
you were to search these queries against every
document in the group. The variation is a
consequence of the search setup, where the
query's probability (what is used to create
the score) is conditioned on the document's
probability.

If you need scores that don't vary by query,
you can randomly sample 50-100 documents for a
query and calculate the mean and standard
deviation, then normalize new scores for that
same query using that mean and standard
deviation.

The similarity score is a positive score that
usually ranges from 0 to 300 (but can
sometimes go higher), where a score above 200
usually means the document is semantically
similar to the query. At the moment, the score
is very useful for ranking (we’ve seen it
outperform many existing semantic ranking
approaches). For example, you can use it for
re-ranking the top few hundred examples from
an existing IR system.

One thing to keep in mind for semantic search
is the tradeoff between model accuracy and
speed, as speed is often a desired property of
search. We’ve found the “ada” model sufficient
for many search tasks, and it’s substantially
faster than the most capable model, “davinci”.
We encourage you to experiment with the
different models to see if “ada” or “babbage”
work for your search use case.

The search endpoint can query up to 200
documents in one request. If you have more
documents than that, you can divide them over
multiple requests (the document similarly
scores will stay the same across requests as
the query stays the same). One limitation to
keep in mind is that the query and longest
document must be below 2000 tokens together.
You can read more about the search endpoint in
the API Reference and try it out using the
Semantic Search Tool.

