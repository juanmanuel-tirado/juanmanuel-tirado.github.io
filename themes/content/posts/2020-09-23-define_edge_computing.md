---
categories:
- Opinion
- Technology
clases:
- wide
- dark-theme
date: "2020-09-23T08:45:11Z"
excerpt: Edge Computing promises millions of dollars of revenue. However, we do not
  yet have a definition of what it is.
tags:
- edge computing
- fog computing
- opinion
- technology
title: We Have to Define What is Edge Computing
url: /define_edge_computing/
---
The first time I heard about edge computing was back in 2015. Since then, I have been working for startups to enable
distributed data-driven solutions for the edge. It looks like everybody (or almost everybody) is aware of what edge
computing is. However, all this time I have been working in a technological paradigm without a clear definition
statement.

At the moment of writing this post, there is no clear definition of what the edge is. These are some definitions you can
find online. Some of them are the ones used in the Wikipedia entry for edge computing.

> all computing outside the cloud happening at the edge of the network, and more specifically in applications where
real-time processing of data is required
> [Karim Arabi](http://www2.dac.com/events/videoarchive.aspx?confid=170&filter=keynote&id=170-103--0&#video)

Your mobile phone and all your wearables are the edge according to this definition.

> anything that's not a traditional data center could be the 'edge' to somebody
> [ETSI](https://www.etsi.org/newsroom/blogs/entry/what-is-edge)

This may include elements such as server proxies.

> the edge node is mostly one or two hops away from the mobile client to meet the response time constraints for
real-time games'
> [Gamelets — Multiplayer mobile games with distributed micro-clouds](https://doi.org/10.1109%2FICMU.2014.6799051)

In this case, the edge is not the final user device. It is something between the cloud and the user.

> computing that’s done at or near the source of the data, instead of relying on the cloud at one of a dozen data
centers to do all the work
> [Paul
Miller](https://www.theverge.com/circuitbreaker/2018/5/7/17327584/edge-computing-cloud-google-microsoft-apple-amazon)

This definition points out the idea of data proximity.

Well, I have to say that all these definitions are correct. Why? Because the edge is so abstract that it admits almost
any definition. The edge is so vaguely defined, that becomes something blurry and difficult to demarcate in any
architectural design. Also, we have the cloud and the fog. What does the edge have to do with the cloud? And the fog?

---

## Why Do we Need the Edge Computing?

Distributed architectures are extremely dynamic, and new requirements appear every day. We constantly revisit computing
paradigms and adapt them to fit these new requirements. Edge computing is not an exception.

YouTube appeared back in 2005, offering videos by streaming was a disruptive approach to the previous
download-it-and-then-watch-it solution. Data flows from servers to users for its consumption. Among the many technical
challenges of this approach, we have the latency issue. The more hops data has to travel through, the higher the latency
is. The best solution is to replicate data into servers near its final destination. Akamai has been doing this since
1998, and it is a 7000 employees global company.

Nowadays, data keeps flowing from servers to consumers. However, the final user is not a passive consumer any longer.
Every minute [350,000 tweets are generated](https://www.dsayce.com/social-media/tweets-day/), [300 hours of video are
uploaded to YouTube every minute](https://merchdope.com/youtube-stats/), and [100M pictures and photos are uploaded to
Instagram per day](https://www.omnicoreagency.com/instagram-statistics/). Users generate vast amounts of information to
be distributed and shared. Numbers are even larger if we include information produced by mobile phones, sensors, and
other devices. Additionally, the promised 5G will push even harder the limits of current infrastructures.

Data is pushing hard from users to data centers. The inclusion of AI applications in the end-user loop demands
additional data processing that increases the costs. Is here, where transferring part of the computation to the final
user can improve the user experience while releasing computing facilities cutting down operational costs.

Edge computing has something to do with this final step of the data consumption/production flow. However, is edge
computing a set of rules, a programming paradigm, an architecture, or a library? This is what we need to define.

---

## The Fog Finishes Where the Edge Starts

We cannot talk about edge computing without comparing it with fog computing. Unlike edge computing, the fog has a clear
definition statement. Large companies including Intel, Cisco, or Microsoft among others joined efforts and created the
[Open Fog Consortium](https://www.iiconsortium.org) back in 2015. They define fog computing as:

> A horizontal, system-level architecture that distributes computing, storage, control and networking functions closer
to the users along a cloud-to-thing continuum

You can check the reference architecture
[here](https://www.iiconsortium.org/pdf/OpenFog_Reference_Architecture_2_09_17.pdf).

This definition clearly states that fog computing is an architecture. Nor a technology, nor a protocol, nor a paradigm,
an architecture with its interconnected components. And I think this is a smart decision. The complexity of a problem
with so many variables and scenarios cannot be addressed by a single technology.

The fog occupies the room between the cloud and the edge and can be whatever. Fog is some sort of poetical name to
define everything that exists between the cloud and the edge. Surprisingly, if you check the aforementioned reference
architecture document, edge computing is only mentioned in the glossary. However, the term edge appears several times in
the document.

## Then, What Is Edge Computing?

If we assume that the fog is an architecture composed of different interconnected elements allocated between the cloud
and the edge. Then, what is the edge?

Is the edge an architecture? This would mean that the edge is probably organized into hierarchies, layers, etc., and
this does not seem to be the case.

Is the edge a protocol? If so, which one?

Is the edge a programming paradigm? I do not think so. If this were the case, there must already be a programming
language supporting this new paradigm.

Is the edge a set of recommendations/experiences/use cases? Maybe.

It is difficult to answer any of these questions. Furthermore, I ignore the fact that the fog definition somehow
overlaps with what many practitioners assume is a task to be done at the edge. Is the edge a component with some
personality? Or it only a passive entity consuming/producing data?

## Summary

As you can see, edge computing is an open concept that is not clearly defined yet. You can extract from my words that
there is a certain agreement on what the edge is. However, I think it is time to clearly define what we talk about when
we refer to edge computing. There is room for new projects to become a reference in the Edge Computing world. As Google
is the reference in search engines, I think Edge Computing will become defined once we have projects that stand out.

Hopefully, this post is a starting discussion trigger. I would love to hear your opinions about this topic.

Thanks for reading!