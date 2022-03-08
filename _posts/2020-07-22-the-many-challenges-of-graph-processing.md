---
title: "The many challenges of graph processing"
date: "2020-07-22 09:10:37.000000000 +02:00"
categories:
- Data science
- Graphs
- Software
tags:
- datascience
- graphs
- programming
- structures
mermaid: true
use_math: true
layout: splash
permalink: "/the-many-challenges-of-graph-processing/"
excerpt: "What are the main issues graph processing has to deal with? Serve these lines
  as an appetizer for those curious minds out there."
---
In a [previous post](https://jmtirado.net/graph-processing-a-problem-with-no-clear-victor/) I exposed my opinion about the lack of a clear platform/solution/framework/architecture for graph processing. However, what are the main issues graph processing has to deal with? Serve these lines as an appetizer for the curious minds out there.

A graph is a collection of vertices $V$ and edges $E$ connecting these vertices. A graph $G=(V,E)$ can be directed or undirected. In a directed graph any edge from vertex $u$ to vertex $v$ has a direction ($u {\rightarrow} v$). This is, we can go from $u$ to $v$.

<div class="mermaid">
graph LR;
   u((u))-->v((v));
</div>

If the graph is undirected we have no direction ($u {-} v$). This is, $u$ is connected to $v$.

<div class="mermaid">
graph LR;
   u((u)) --- v((v));
</div>

Additionally, we can have edges with weights ($W(e)$).

<div class="mermaid">
graph LR;
   u((Madrid))-- 621km ---v((Barcelona));
</div>

Graphs are particularly suitable to **represent absolutely anything**. From social networks to interactions between atomic forces. A very computer friendly representation of a graph is using an **adjacency matrix** $A$. $A$ is a square matrix of size V x V where $A_{ij}$ indicates that it exists an edge between vertices $i$ and $j$. For undirected graphs $A$ is a symmetrical matrix. For example, for the graph

<div class="mermaid">
graph LR;
    a((a)) --> a((a));
    a((a)) --> b((b));
	c((c)) --> b((b));
	a((a)) --> d((d));
	d((d)) --> b((b));
	b((b)) --> d((d));
</div>

we can get its representation in an adjacency matrix:

$$
\begin{array}{ccc}
 & & to \\\\ 
 & & \begin{array}{cccc} a & b & c & d \end{array} \\\\
\textit{from} & \begin{array}{c} & a \\\\ & b \\\\ & c \\\\ & d \end{array} &
  \left(\begin{array}{cccc} 
1 & 1 & 0 & 1 \\\\
0 & 0 & 0 & 1 \\\\
0 & 1 & 0 & 0 \\\\
0 & 1 & 0 & 0
  \end{array}\right)
\end{array} $$

Because the previous representation is a matrix, we can use all the available tools of algebraic operations. For example, by summing up the values of every column we have the number of edges targetting every vertex. Additionally, we can compute eigenvalues and eigenvectors that can give us interesting information  using [spectral graph theory](https://en.wikipedia.org/wiki/Spectral_graph_theory "spectral graph theory").

But there are other aspects to be taken into consideration. **What about memory utilization?** If you think about the adjacency matrix above, for a 4 x 4 matrix (16 cells) we have 10 empty cells (equal to 0). When allocating memory for this matrix, we are wasting 62% of the allocated memory with non-relevant information. For very large sparse matrices this cost is unacceptable. And we have sparse matrices in many scenarios, think about social networks. Facebook has 2.6 billion active users and there is a limit of 5000 friends per profile. Additionally, some operations become cumbersome. For example, knowing the adjacent vertices involves operations with the whole matrix.

Fortunately, we have other solutions that are more "memory friendly" or at least more suitable to certain scenarios. An **adjacency list** is a list where every item represents a vertex in the graph. For every vertex, we store pointers to its adjacent vertices (neighbours). Following with the example above:

<div class="mermaid">
graph TD;
   start(start) --> a;
	subgraph vertex a
    a(a) --> a0((a)); a0 --> a1((b)); a1 --> a2((d));
	end
	subgraph vertex b
	b(b) --> b0((b));
	end
	subgraph vertex c
	c(c) --> c0((b));
	end
	subgraph vertex d
	d(d) --> d0((b));
	end
    a --> b;
	c --> d;
	b --> c;
	d --> last(nil);
</div>

The adjacency list limits the information to be stored to the adjacent vertices. Vertex $a$ has access to a list of references to its adjacent vertices. This facilitates the implementation of **traversing** operations that can travel across the graph vertices and makes possible to store additional data enhancing **data locality**.

And here is where graphs become a really painful data structure to deal with from the point of view of performance. Let's assume we want to visit all the neighbors of vertex $a$ ($\Gamma(a)$). The adjacent vertices of $a$ are: itself, $b$ and $d$. This means that we have to access memory positions for $b$ and $d$. What happens if these memory fragments are not already cached? That is a cache miss, which means that we have to pick up that piece of graph from main memory. For vertices with a large number of neighbors cache misses will iteratively repeat. This directly impacts the performance of a graph traverser. The **lack of locality** in memory accesses is a performance limitation in most graph algorithms.

If we think of distributed solutions, the scenario is even worse. For a distributed memory solution with the graph split across nodes we may have something like:

<div class="mermaid">
graph TD;
   start(start) --> a;
   subgraph nodeA
	subgraph vertex a
    a(a) --> a0((a)); a0 --> a1((b)); a1 --> a2((d));
	end
	subgraph vertex b
	b(b) --> b0((b));
	end
	end
	subgraph nodeB
	subgraph vertex c
	c(c) --> c0((b));
	end
	subgraph vertex d
	d(d) --> d0((b));
	end
	end
    a --> b;
	c --> d;
	b -.-> c;
	d --> last(nil);
</div>

For an operation running in nodeA any access to vertices $c$ or $d$ would have to retrieve information from other node. Complexity increases if edge weights can be modified. Who is the owner of the edge? And the vertices? Solutions such as [Metis](http://glaros.dtc.umn.edu/gkhome/metis/metis/overview) can give you the best graph partition across $n$ bins or nodes. However, adapting algorithms to work with distributed partitions is not easy at all. And computing the best partition is an expensive operation.

And what about **parallelism**? This depends on the problem or algorithm to be parallelized. However, there is a clear problem in the utilization of concurrent access data structures such as queues or stacks in the classic iterators such as Breadth First Search (BFS) or Deep First Search (DFS). For example, this is the pseudocode for BFS:

```pseudocode
1  procedure BFS(G, root) is
2      let Q be a queue
3      label root as discovered
4      Q.enqueue(root)
5      while Q is not empty do
6          v := Q.dequeue()
7          if v is the goal then
8              return v
9          for all edges from v to w in G.adjacentEdges(v) do
10             if w is not labeled as discovered then
11                 label w as discovered
12                 w.parent := v
13                 Q.enqueue(w)
```

If we consider the pseudocode above to run in parallel we have to guarantee that $Q$ and labels structures are accessed/modified atomically. This imposes a tremendous bottleneck for multiple parallel instances that can result in really poor performance.

A really interesting problem is **what happens when the graph does not fit into main memory**? Cache misses are expensive in terms of performance, but accesses to secondary memory are orders of magnitude more expensive.

To summarize some of the elements that make graph processing a big headache:
- Graphs are "unstructured" by default
- Lack of locality in memory access
- Operations requiring shared data structures
- Working with graphs split across distributed memory solutions is expensive. Actually, deciding how to split the graph is computationally expensive

I'm sure you can enumerate other problems regarding graph processing.
