---
title: Migration from Jekyll to Hugo
categories:
- Opinion
tags:
- optinion
date: 2023-12-20
excerpt: The blog has been migrated from Jekyll to Hugo
---

On July 2022 I decided to migrate from Wordpress to Jekyll and host the blog on GitHub (see my previous post ([here]({{< ref "posts/2022-03-18-migration.md" >}})).

## Why another migration?

There is not a particular reason for this. I was OK with Jekyll and its simplicity. However, I was a bit disappointed installing `Ruby` dependencies. I do not use it for any other purpose rather than generating static content for Jekyll. A powerful reason to make the migration is the fact that Hugo is written in Go, and guess what [I'm familiar with that language]({{< ref "build-systems-with-go" >}}).

## Style changes

I am not particularly familiar with the available Hugo themes. After a brief search I decided to configure the [Blowfish](https://blowfish.page/) theme. The theme is well documented, there is a good number of pages using it, and it fulfills my needs. I will probably continue doing some changes until I am happy with the final look.

## Hosting

I will continue hosting the blog in GitHub at least for the medium term. I am curious about using Google's [FireBase](https://firebase.google.com) so I can have some computational capabilities and do something more sophisticated. In fact the Blowfish theme has some features ready to work there.

## Time spent

I am genuinely surprised about the time I have spent in the migration. I could do everything in one evening. Hugo already has a `hugo import jekyll` command that simply collected all my posts and assets from Jekyll. The integration with GitHub actions was easy. I was stupid enough to require a couple of iterations, but nothing really technical.

I had to take a look at how to use math notation with `mathjax` and generate diagrams with `mermaid`. That required some delete, copy, and paste. Nothing traumatic. After a quick look, the old posts seem to be readable. I would like to have a feature image for these posts, like I have in their [medium](https://juanmanuel-tirado.medium.com/) versions but I think they will stay as they are right now.

## Conclusions

The migration process has been smooth and easy. No headaches. This is something to thank. I think the blog looks "more professional" although that has nothing to do with Jekyll itself. It's me that I didn't spend enough time looking for a fancy theme. Anyway, I will keep on changing the aspect in the coming months until I find it good enough. I appreciate how easy it is to configure many components.


