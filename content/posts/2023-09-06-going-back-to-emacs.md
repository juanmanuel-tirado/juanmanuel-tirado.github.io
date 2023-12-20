---
categories:
- Opinion
- Technology
- Programming
date: "2023-09-06T00:00:00Z"
excerpt: This is my personal story and the reasons I've found to make Emacs my default
  editor again.
tags:
- opinion
- opensource
title: Why Emacs is my default programming editor... again
url: /emacs_is_my_default_editor_again
---

If you're a developer or simply an open-source adopter you have probably come across Emacs. For the eyes of the inexperienced an incomprehensible bunch of letters in an ugly layout, for the experienced an awesome productivity tool. This is my personal view of this historical editor and why I have used it, stopped using it, and finally made it my default editor.

![Emacs Logo](https://www.gnu.org/software/emacs/images/emacs.png#center "Emacs Logo")

## How the ******** do you write something here?

The first time I saw something called Emacs was in a computer lab back in 2002. I was completely new to Linux and in that lab, I had my first crash course. As a newbie, I had to write and compile a `helloworld.c`. The first task was to write the code and for that, you need a text editor. I don't remember the distribution installed in those computers but I vividly remember a terrible UX experience trying to find a text editor. I found something similar to a Windows notepad and wrote the code, compiled it using the terminal, and felt like a hacker after displaying a `Hello World!` message in the terminal prompt. 

When I opened my `helloworld.c` file again something unexpected happened. The code was shown to me in a grey background editor. I cannot say the look-and-feel was catchy or pretty. That was XEmacs, which was the default editor in that distribution. From there... chaos. First thing, how do I write here? I could see the cursor moving up and down but typing was simply showing incomprehensible messages and suddenly I typed something. This is a text editor. Why I can't simply write as I type? okay, this is finally done. How do I save it? Oh, there is a save button. Click on it. Mmmm has it been saved? I probably spent more than five minutes doing such a simple task. That was not a nice experience. Nevertheless...


## Far beyond a text editor

For some reason I cannot explain (sadism maybe?) I started using Emacs on a daily basis. No need to say that I kept spending an embarrassingly amount of time trying to do simple things (copying, pasting, selecting text regions). Anyway, I remember that we had to explore some large text files and Emacs was the only editor that could load the files and search for text chunks without exhausting the memory. Obviously, I had no idea about the existence of `grep` or `awk`. The most perplexing thing for me was discovering that there were games inside Emacs. Some examples [here](https://www.masteringemacs.org/article/fun-games-in-emacs).

Emacs is from an architectural point of view more than a simple text editor. It has a Lisp interpreter. Actually, a dialect of LISP called [Emacs Lisp](https://en.wikipedia.org/wiki/Emacs_Lisp)(Elisp). Most of the functionality is written in this dialect, while the remainder is written in C. This means that you can easily port your scripts to other machines without recompiling.

Emacs is terribly configurable. You have the freedom to configure everything. You can control everything. Total freedom. Everything can be changed, and everything can be customised. If you start playing with your `init.el` file, you can fall into a neverending loop. The worst thing is copying and pasting others' solutions to certain problems such as using `CTRL+c` and `CTRL+v` instead of the mysterious shortcut, having larger fonts, using a different fancy layout... You maintain that file for years and it keeps growing and growing. And because you copied and pasted everything guess what... You have no idea how it works and what it was for. So you have to start paying attention and learning something about how everything works. Because it will eventually break. Somehow this is the philosophy of a software developer. This is not a price everybody will pay when using a text editor.

## You can do everything in the same place

You can do everything (or almost everything) ONLY using Emacs. I do not exaggerate. You can send email, checkout GIT branches, compile a LaTex file, display a PDF file, run terminals, run your own agenda... Everything in the same place. Have I mentioned that there is syntax highlighting or drop down suggestions? You can find experienced developers using Emacs as their only tool. Buffer content that appears and disappears after "secret" key combinations displaying this and that. Some users can be extremmely efficient and run awesome pipelines in their daily tasks. After how many years using Emacs? God knows.

## A client server architecture

Not many people know this, but Emacs can work in a server-client modality. This is interesting for two reasons. First, you can start Emacs as a server when booting your machine. Now you can run clients connecting to this instance and you don't have to wait for package loading. Second, this can be an interesting option if you want to have a machine only for development/computation. Imaging connecting from your laptop using Emacs to the server in that machine and operate as usual. Particularly useful if you develop on a different platform an cross-compilation is not an option.


## More functionality by adding packages

Emacs does not have a marketplace like other solutions such as [VSCode](https://marketplace.visualstudio.com/VSCode) or [JetBrains IDEs](https://plugins.jetbrains.com/). In this case, Emacs relies on [MELPA](https://melap.org) (Milkypostman's Emacs Lisp Package Archive) as the main source of curated packages. You can directly install them and make them run in your running instance. Obviously, this is not as fancy as a marketplace running plugins but it is closer to a developers mentality.

## 40 years of history!!!

This was the first GNU release done back in [1985 by Richard Stallman himself](https://www.gnu.org/software/emacs/history.html). That's almost 40 years!!! The code we have today differs from the original release, but the essence is still the same. Do you imagine yourself using VSCode or JetBrains for 40 years? Touché.


## The Org mode

![Org Mode](https://orgmode.org/resources/img/org-mode-unicorn.svg#center "Org Mode logo")

This for me one of the main reasons to use Emacs. The [Org mode](https://orgmode.org) is a plain text quite easy to learn but with enough descriptiveness to do things such as maintaining an agenda, displaying and showing code results, and many others. Something like:

```
#+begin_src python
import random
return [random.random() for i in range(5)]
#+end_src
```

Is not only going to be beautifully coloured, it can also be interpreted and the result displayed inline like some sort of Jupyter Notebook.

```
#+RESULTS:
| 0.9802045044675929 | 0.7814182240285613 | 0.7945518565001365 | 0.7875101844790451 | 0.999364766119846 |
```

There are a good number of packages that permit to export from the Org mode to other formats such as HTML, Markdown, LaTex, ODT, RevealJS, etc. This combined with the definition of [pipelines](https://andreyor.st/posts/2022-10-16-my-blogging-setup-with-emacs-and-org-mode/) can be extremely useful for those working on content that requires to go into different target formats.

It is almost impossible to cover all the possibilities here, but if you're curious you can take a look at the official documentation. There you can find some examples of what people do with the Org mode. Meanwhile, you can check some of the [current features](https://orgmode.org/features.html).

## The learning curve

If you have read so far, you have probably noticed that Emacs is not easy to learn. It takes time. And a lot of frustration. The main problem comes with the UX. Emacs is mainly developed to be used with your keyboard, although the mouse is supported. Unfortunately, it takes a while to understand and memorize the keyboard shortcuts. This is more of a repetitive process rather than a pure memorization exercise. Concepts such as buffers or modes are strange things to learn for somebody who wants to sit down and start typing.

If you're a developer and you want to contribute or simply write your own program you will have to learn Emacs Lisp. This is not a widely used language, so you will probably have to learn something new.


## Some historical complaints

Many users publicly complain about Emacs feeling clunky, specially compared with today's IDEs. And they are probably right. Although I have to say, it has improved a lot since my first contact with this software. However, we still have a steep learning curve.

As I mentioned earlier, Emacs is based on Lisp and this reduces the number of developers willing to design new features. Other languages such as Javascript have been proposed to be supported. However, [Stallman himself](https://emacsconf.org/2022/talks/rms/) is clear about why Javascript is not welcome at this moment. Finally, the fact that Emacs is single-threaded is a big limitation compared with other solutions. Nevertheless, if you take a look at the memory footprint you will probably embrace Emacs and ignore this limitation. 


## Helpful Emacs frameworks

While Emacs is fully configurable, and it already exists good documentation to start your own vanilla Emacs, existing frameworks will simply save you a lot of time and effort. I only mention two of them:
- [spacemacs](https://www.spacemacs.org) it is designed to help users with all the emacs commands in a consistent manner.
![spacemacs logo](https://github.com/syl20bnr/spacemacs/blob/develop/doc/img/spacemacs-python.png?raw=true "spacemacs")
- [Doom Emacs](https://github.com/doomemacs/doomemacs) is another already configured emacs with a good number of curated packages.
![Doom Emacs](https://raw.githubusercontent.com/doomemacs/doomemacs/screenshots/main.png "Doom Emacs")


## Where to start

The very first thing is to install it. There are installation packages for all distributions and platforms. If you're looking for a book, [Mastering Emacs](https://www.masteringemacs.org/) can be a good (but expensive) option. If you want to learn more about Emacs Lisp go for the official [Introduction to Programming in Emacs Lisp](https://www.gnu.org/software/emacs/manual/eintr.html).

If you're more into video tutorials, there are some nice resources on YouTube. These are my recommendations:
- [The Absolute Beginner's Guide to Emacs](https://www.youtube.com/watch?v=48JlgiBpw_I) contains all you need to understand the main concepts.
- [Org Mode Basics](https://www.youtube.com/watch?v=VcgjTEa0kU4) is a wonderful summary of how the Org mode works.
- [DistroTube](https://www.youtube.com/@DistroTube) has a list of videos about how to configure Emacs. [This](https://www.youtube.com/watch?v=d1fgypEiQkE) is the first one on the list. 
 
 Finally, you can find Emacs enthusiasts on [StackExchange](https://emacs.stackexchange.com/) or [reddit](https://www.reddit.com/r/emacs/). 


## Conclusion

Emacs is a rara avis in the software ecosystem. The community has been maintaining and developing it for almost 40 years. It is an awesome tool with a steep learning curve that can lead adopters to frustration. For those who have already adopted Emacs as their main programming tool congratulations. For those who have forgotten about the beast, give it a second try. And for those who have no idea what it feels just, give it a try by using one of the existing frameworks.

For me, it has become my default editor. This doesn't mean I am not going to use any other tool. This only means, that Emacs is my baseline and I will compare every solution with this software beast.

Thanks for reading.


 





