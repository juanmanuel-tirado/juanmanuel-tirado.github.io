---
title: "Why should you learn LaTeX, or at least give it a try?"
date: "2020-05-10 18:35:13.000000000 +02:00"
categories:
- Software
tags:
- latex
- software
mathjax: true
permalink: "/why-you-should-learn-latex-or-at-least-give-it-a-try/"
excerpt: " LaTeX is one of the most successful and amazing free software projects
ever done. It has been around for more than thirty years with two Turing awarded
researchers directly participating in its design and implementation. LaTeX must
have something special. Hopefully, after reading this post you will consider giving
it a try."
layout: splash
---
If you have to write down a document you will run your default text processor (probably MS Word) and you will not
consider any other option. This processor will fulfil all your needs. I would say that 95% of users out there has no
idea what is LaTeX. And this is perfectly fine. However, it is a pity. Because, LaTeX is one of the most successful and
amazing free software projects ever done. It has been around for more than thirty years with two Turing awarded
researchers directly participating in its design and implementation. LaTeX must have something special. Hopefully, after
reading this post you will consider giving it a try.

I will not showcase how to use LaTeX, there is a lot of wonderful tutorials around. I will only enumerate when you MUST,
SHOULD and SHOULD NOT use LaTeX.

## A bit of history
[Donald Knuth](https://en.wikipedia.org/wiki/Donald_Knuth) (Turing Award 1974) published his first edition of [The Art
of Computer Programming](https://en.wikipedia.org/wiki/The_Art_of_Computer_Programming) in 1968 when he was thirty. By
then, books were printed using monotype settings. Knuth was happy with the final print. However, the second edition in
1976 had to be typeset again because the original fonts were no longer available. When Knuth received the galley proofs
he was really disappointed. He found them inferior.

He commited himself to design his own typesetting system. We are talking about the late seventies, when digital
typesetting was a problem to be solved. Actually, Steve Jobs himself contributed to [this
topic](https://www.ted.com/talks/steve_jobs_how_to_live_before_you_die). Knuth planned to spend his sabbatical year in
1978 to finish the project. He really understimated the complexity of the task. The final solution was not ready until
1989! Knuth called this language TeX with each letter a capital Greek letters tau ($\tau$), epsilon ($\epsilon$) and chi
($\chi$). TeX is the abbreviation for τέχνη (techne) which means "art" and "craft". Knuth has always insisted that you
should pronounce it /tɛk/.

When [Leslie Lamport](https://en.wikipedia.org/wiki/Leslie_Lamport) (Turing Award 2013) started using Knuth's TeX he
started writing some macros for his own purposes. LaTeX is simply LAmport's TeX, a collection of macros on top of TeX to
make it easier. And this is the main collection we have today.

## What can I do with LaTeX?
With LaTeX you can have a really high quality typesetting document with a low effort. Yes with a low effort. I will
discuss some details later. This claim is huge. EVERYBODY can get professional results writing plain text and using
markups with a software that is free and can run virtually everywhere. That is why LaTeX is the standard in academia and
engineering.

## When you MUST use LaTeX
* **You are in academia, particularly in any
[STEM](https://en.wikipedia.org/wiki/Science,_technology,_engineering,_and_mathematics) discipline**. In this scenario
manuscripts are everything. Content is really important and requires a tremendous amount of work. In the case of PhD
manuscripts you MUST consider spending some time learning LaTeX to make the difference in your final outcome. I have
seen PhD. manuscripts written in MS Word and I have to say that somehow (for me) it diminishes the value of the
manuscript.
* **You work with abundant bibliography**. If you are preparing any document with large bibliographies you want to go
with LaTeX + BibTeX. Simply prepare your BibTeX file with your bibliography entries, tag them, and use the label in your
latex document. The compiler will do the rest. I know, there are plugins and solutions for MS Word and other text
processors. But remember, for thirty years this problem has been solved in an easy way. And from my experience, these
plugins result cumbersome.
* **You are using formulas**. Formulas like $y=\frac{1}{x}$ can be easily managed by some plugins. Formulas like
$f(x)=\frac{1}{\sigma \sqrt{2\pi}}e^{-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^2}$ can be simply impossible to
write. Every decent solution around to manage math formulas is based on LaTeX. Why not to use it directly?
* **You need outprints with figures using the best quality possible**. Formats such as SVG cannot be available for your
text processor. With LaTeX you can generate PDF documents with embed SVG figures. Not many solutions around can offer
something like this.
* **You want a free solution**.
* **You want it to be forward compatible**. LaTeX has been around for more than thirty years. We can typeset really old
documents and see how they were intended to be.
* **One entry point several output formats**. Because LaTeX is a typesetting system you can get outputs in DVI, PDF,
HTML, XML, etc. with a single document.
* **You want to forget about the document layout**. LaTeX is somehow like HTML + CSS, once you define the document
structure you simply use a markup language and the compiler will make everything coherent for you. No more paragraphs
separated with double spacing instead of single space.

## When you SHOULD use LaTeX
* **You are new to LaTeX, you have to start a new project and you are looking for all the advantages that it offers**.
This is the moment to start with LaTeX.
* **You want your documents to stand out among others**. And you will. LaTeX outcomes have a distinguishing quality and
it is loved among practitioners.
* **You are considering to write a book, article, manuscript... and maybe self-publish it**. This is a common situation
nowadays with the adoption of platforms such as Amazon Self Publishing. With LaTeX you can go from your raw text to a
high quality .epub, .mobi ebook file.

## When you SHOULD NOT use LaTeX
* **Your document is already written in other format**. The content is probably easy to be moved to LaTeX. However, the
layout of the document could be hard to get.
* **You are doing a collaborative work and you are the only LaTeX practitioner**. Do not move into LaTeX, do not even
consider it. My experience says that after starting everything to LaTeX, your colleagues will complain and you will
finally move everything to a commonly understood format two hours before the deadline.
* **The layout of your document means everything to you**. This means that you are thinking about a mesmerizing print
with 30 types of fonts, text lines crossing the text body, images in every possible place in your document... Then
probably LaTeX is not your candidate. Think about QuarkXPress.

## When people complain about LaTeX they say...
* **It is difficult**. LaTeX has a much stepper learning curve when compared with MS Word that is true. However, getting
a basic LaTeX (text, figures, titles, tables) is not so difficult. There is a million examples out there. The complexity
comes in understanding the concepts used by LaTeX such as floating objects.
* **I cannot see what I am doing**. LaTeX is not a [WYSIWYG](https://en.wikipedia.org/wiki/WYSIWYG) solution, this means
that you have to compile and then check the output. Fortunately, there are some programs such as
[TeXMaker](https://www.xm1math.net/texmaker/) that give you a better user experience.
* **Figures do not appear where I want**. Well, this is a classic misconception about how figures placement works in
LaTeX. LaTeX computes the best placement for your figures. This can be configured using
[modifiers](https://www.overleaf.com/learn/latex/Positioning_of_Figures).
* **I cannot easily change the layout of my document**. This is true. If you want to set your own document structure you
need to have a deeper understanding of the macros. There is a nice community to help you with it. However, this may
require some time and effort. However, there is a vast number of templates already
[defined](https://www.latextemplates.com/) ready to be used. You can look for the one the best suits you.

## And now...
If you read so far this probably means I captured your interest. If so, you can start learning some basics
[here](https://www.latex-tutorial.com/quick-start/) and if you need some help check the
[StackExchange](https://tex.stackexchange.com/).