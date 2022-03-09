---
title: "The best programming language"
categories:
- Opinion
- Programming
- Software
tags:
- opinion
- programming
- software
permalink: "/the-best-programming-language/"
excerpt: "Developers are wrong when they compared programming languages when looking
for the best programming language."
---

I recently came across a collection of drawbacks, missing points, and errors in the Go language ([here](https://github.com/ksimka/go-is-not-good)). Taking a look at this collection I found interesting points of view from other developers about the language itself. I found it surprising how the main design aspects in Go (no OOP, simplicity, lack of inheritance, etc.) are considered design errors and missing features. Reading all this, it came to my mind the first comments I read about Python more than ten years ago: it is very slow, no brackets are you kidding? it is untyped, etc. No need to say that Python is one of the most versatile languages out there.

## Comparing programming languages

When comparing programming languages I have observed that most authors (not academic authors) follow a certain pattern. Think about any pair of languages X and Y and replace them in the following sentences:

> When compared with Y, X is slower and less performant
> It is a shame how X manages data structures when compared with Y
> X manages dependencies terribly when compared with Y
> Although X it is better than Y what regards to Z, X cannot do this and that

You can get the idea. **Given any language X, we can find another language Y such that it outperforms X**. And this makes absolute sense to me. Programming languages, like human languages, help us to articulate a view of the world. Languages are designed with specific features in mind. Our perception of the world is difficult to be precisely expressed in a programming language ([see](https://www.mortylefkoe.com/how-our-language-determines-our-reality/)). And this makes me think about the lack of "programming culture" in the developer's world. I find developers living in some sort of self-imposed **language dogma**. 

### Single comparison with the language you are proficient with

You can find on the Internet a plethora of comparisons between languages. For example, you can search "Go vs Rust" and you will find thousands of comparisons. This [example](https://www.infoworld.com/article/3436960/rust-vs-go-how-to-choose.html) is a generic view of both languages. I find it superficial, but it is OK. A comparison of both languages from a generic perspective can be a good starting point for any curious developer. However, you can find other comparisons like [this](http://www.yager.io/programming/go.html) where it is clear that the author feels much more comfortable with other languages. This fact leads the author to point out the disadvantages of language X when compared with common features of the other languages. 

### Ignore the motivations of the language designers

Defining a programming language is not an easy task. Normally, **languages are designed to improve certain aspects of other existing languages or to improve a certain task**. Python was born to replace the ABC language. Java and its virtual machine were designed to run the same code everywhere without additional compilations. Erlang was created to improve telephony applications. We cannot expect these languages to be a particularly suitable solution for absolutely everything. **Comparing two languages designed for different goals performing the same tasks is somehow biased**. 

Think about Java. It has been around since 1995 and it can be found it large platforms that have nothing to do with its original purpose. And this could be one of the main reasons why many developers hate it. Nowadays developing an API REST in Java is convoluted when compared with any younger language. However, we forget how the Java Virtual Machine opened up a huge line of wonderful solutions such as Android.

### Developers are reluctant to learn new languages

A developer is someone who has to do a task in a limited time. In the best case she uses a programming language she is comfortable with. And this makes a lot of sense. The more proficient you are with a language, the shorter it will take you to finish a task. However, we are wired to be lazy. It is known that learning new languages changes the configuration of our brains. **Developers are reluctant to learn other languages**. It requires an additional effort and you will feel miserable until you hit some proficiency. This makes developers criticize languages they are not familiar with.

### The problem of hyped languages

Marketing is a powerful tool, and programming languages are not an exception. **From time to time, everybody wants to learn a certain hyped- language**. Any criticism of these hyped languages is taken with disrespect, even when its warring defenders have not a deep experience.

### Diminish others efforts

This comes from human psychology. When a new language is presented to a developer, she will immediately say something like

> I don't understand why they have wasted their time with this. In language X you can do this in three lines.

**This is a method to protect our own beliefs by diminishing others' efforts**. This is perfectly human. However, reiterating this kind of idea is a demonstration of ignorance and a lack of respect for other developers. A language cannot be discarded using this reasoning.

### Language worshippers

The Internet seems to create static groups of people. Those who love Justin Bieber vs those who hate him. If you are not inside the group, you are outside. The same applies to programming languages. **Some developers seem to live in a dogma where a language is the sancta sanctorum and no criticism is plausible**. This is ridiculous.

## Looking for the best programming language

Software development is not an easy task. Technology evolves at a high speed. The feeling of being outdated is common and you can feel like a dinosaur in your late thirties quite easily. The most naive pieces of advice you can get to avoid this are: go beyond your comfort zone, never stop learning, try new languages... You already know them. However, I think this is a much profound problem. **There is a generalized misconception of what makes a programming language great**.

Let's think about the following situation. A company architect decides to use language X to implement system S. The architect is sure that the many features of X will facilitate the development of S. Unfortunately, nobody in the development team is familiar nor proficient with X. What can we expect from this? Only problems. Is X the solution for S? Probably it is. However, developers may have done their best to implement S and get an MVP using language Y and then iterate over and over. The programming language is not the ultimate solution for this case.

The previous situation is a simple example. You can find many other scenarios where the selection of a language may not be based on features, performance, nor learning curve. Is for this reason that comparisons based on these elements must be taken with caution. These comparisons only make full sense in a given development context. Therefore, we can claim that **the best programming language is the one that permits to do a task in the most efficient way**. We should define what do we mean by efficient and this is outside the scope of this post. We will say that efficiency refers to development limitations such as the budget, the developers' experience, the time, etc.

## Summary

Developers tend to diminish programming languages they are not familiar with. Sometimes this criticism ignores the target problems programming languages were designed to solve making developers members of "programming language dogmas". An exercise of humbleness must be done by all of us to understand the richness of languages we have nowadays, and how certain languages are more suitable than others abstracting certain problems. Hopefully, this may help us all to adopt an open-minded development culture that ultimately will help us to reduce misinformation and improve the dissemination of new ideas and solutions.

I would love to hear your opinions.

Thanks for reading.
