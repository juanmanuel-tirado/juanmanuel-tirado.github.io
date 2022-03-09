---
title: "Graph processing: a problem with no clear victor"
date: "2020-07-16 09:30:22.000000000 +02:00"
categories:
- Data science
- Graphs
- Opinion
tags:
- datascience
- graphs
- opinion
- spark
- tensorflow
permalink: "/graph-processing-a-problem-with-no-clear-victor/"
excerpt: "Even though graph processing is a problem in computer science since its very
  beginning, there is not a clear victor among all available solutions. Why?"
layout: single
clases:
    - wide
    - dark-theme
---
We all depend on the Internet to search for potential solutions to technical problems. For example, for big data problems after five minutes in Google you will find out that Spark may help you. Even if you have no idea about what is Spark you will come across this name. Something similar occurs to TensorFlow when searching for deep learning solutions,  Kubernetes for cloud, Docker for containers... **It seems that there is always one platform/framework/library for every buzzword in computer science**. However, try to look for a graph processing solution. You will find out that there is **no clear victor**. And I find this quite surprising.

In 2015 me and my colleagues at Inria published an [article](https://hal.inria.fr/hal-01111459/document) proposing a middleware that could inspire developers to offer a generic framework to implement distributed graph processing solutions. And we made that because we had a strong feeling that there was not a consistent proposal to accelerate the development of massive graph processing solutions. And this is surprising if we consider that [The Graph500 benchmark](graph500.org) has some computing intensive problems using graphs. The explosion of social networks after the born of Facebook and Twitter captured the attention of the research community and put on the table new problems in terms of computation and scalability. Additionally, there is a vast number of problems that use graphs as the underlying data structure to be used. Graphs are used for fraud detection, game theory, and a vast number of data related problems.

There is massive ecosystem of graph libraries. Just to mention a few available I have experience with we have:   [GraphTool](https://graph-tool.skewed.de/), [SNAP](https://snap.stanford.edu/snap/),  [igraph](https://igraph.org/), [NetworkX](https://networkx.github.io/), [BGL](https://www.boost.org/doc/libs/1_66_0/libs/graph/doc/), [network for R](https://cran.r-project.org/web/packages/network/network.pdf), etc. And then we have modules of extensions for other more popular platforms such as [GraphX](http://spark.apache.org/graphx/) for Spark, the Hadoop extension for graphs [Giraph](https://giraph.apache.org/), the [Neo4j](https://neo4j.com/) which is really a database not a library.

Then, we jump into a more obscure world of solutions that claim to be the most efficient in some aspect. A ton of work has been carried out by research facilities and/or universities. However, I would not suggest any developer to adopt any of these solutions. Purely. Why? Because they are hardly maintained, not documented and will eventually vanish. There are some notorious examples such as Google's Pregel which inspired Apache Giraph and was never done publicly available (correct me if I'm wrong). [GraphLab](https://arxiv.org/abs/1204.6078) code was available and slowly vanished until becoming part of [Turi](turi.com).

If you are a developer and you need to include some graph processing consider these points:
- **The programming language** you want to use may reduce your options.
- **Are you looking for a specific algorithm or you want to explore and/or implement your own solution?** The first somehow simplifies the search because you can look for the best performing algorithm implementation. If you want to do your own exploration look at the next point.
- **How large is your graph?** This is extremely important. Most of the libraries I have cited above will have problems when working with millions of vertices and edges. It is even worse if your graph does not fit into memory.

Unfortunately, at the moment of writing this post **we do not have a clear victor in the world of graph processing solutions**. Actually, most solutions used in companies are tailor made from scratch. Developers code again and again the same algorithms contributing to the mayhem of existing implementations. I am strongly convinced that there is room (and the need) for a new paradigm that may help developers to model distributed graph algorithms simplifying the development and maintainability while increasing performance. Something similar to what TensorFlow did to neural networks by abstracting common pieces and concentrating the efforts of the community into a common solution.

I would like to hear about your comments and experiences.