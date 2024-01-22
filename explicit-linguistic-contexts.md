---
type: technology
---

# Reducing Large Language Model Waste with Explicit Linguistic Contexts

Several "large language models" such as OpenAI ChatGPT, Google LaMDA,
and Meta LLaMA, have been around for a while now and shown impressive
results.

Some of these results also indicate how superfluous parts of human
communication is.

For example, it is very easy to ask ChatGPT to generate a very long
cover letter for applying for a job from a short prompt. This reduces
the information density and requires the recipient of the cover letter
to sift through, or ask ChatGPT to sift through, the text to try and
recover the original dense information.

This is really inefficient.  Can we do better?  Can we do better
*without* using AI?

My suggestion is to introduce explicit linguistic contexts to match
certain real-life situations, such that those situations only allow for
a subset of the full language used in typical situations.

Examples:

  - Job application: The employer creates a job application linguistic
    context describing which words, sentences, and concepts are
    acceptable. A publically available computer program that runs when
    the applicant submits their cover letter will reject an application
    if it does not fit into the given context.

  - Companies responding to your request to unsubscribe from a
    membership: You can disallow them to write about other offers or add
    a sales pitch at the end.  The context will include a limitation on
    concepts that include sales.

  - Going off-topic on online forums: Programmatically forbid this.

It's easier to enforce this when the company is setting the context for
incoming messages from people than when you are setting the context for
incoming messages for companies, since the companies might not care
enough about you to do anything about your automated rejections. Maybe
it would work better with a shared authority.

An analogy to all of this is OpenBSD's pledge() call.  You pledge to the
kernel that you will never need to e.g. read files, and then the kernel
will refuse any future file reading requests.  It's a security feature,
but it's also a nice analogy for what we want to do: Start out with the
entirety of some human language, and then chop off parts that we think
are unrelated to some situation.

The problem is that all of this is hard to do with typical human
languages: They are sprawling messes.  How do you define all of these
properties on e.g. English?  There is no straightforward way, and one
might be tempted to just go back to large language models here and
somehow use them to implement a filter.  But that defeats the purpose of
reducing waste.

Another approach is to reduce a new, better human language.  Humans have
a long history of "constructed languages", and all of them have several
issues.  The usual big issue is that not that many people are using
them.  We don't care too much about that, we just want it to be easy to
analyze.

In other words, the goal is to provide *more* freedom than a set of
predefined text boxes in a form, but *less* freedom than a full-fledged
unstructured piece of English text.

A good technology for that is predicates!  Instead of writing "I like to
eat vanilla icecream in my free time" you would pick the subjects and use
predicates to describe them, like so:

```
my(free time).
vanilla(icecream).
in(free time)(like(eat)(icecream))(I).
```

Some predicates are higher-order predicates.

The twist is that you don't decide yourself which predicates are
available for use - the recipient of your message does.  So you have a
good deal of freedom for determining what to write about, but there are
explicit limits.  For instance, if the recipient didn't allow for the
concept of vanilla or liking icecream, you would be restricted to

```
my(free time).
in(free time)(eat(icecream))(I).
```

which translates to "I eat icecream in my free time", which is still
meaningful, but without some of the nuance (nuance which might be deemed
unimportant).

You would then either write these predicates manually or have a program
try to translate your English into predicate form.  Here's a
work-in-progress example: https://github.com/nqpz/predicateparser

**TODO:** Think of more use cases and examples.
