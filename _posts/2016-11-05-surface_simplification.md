---
layout: post
title:  "Article Study : Surface Simplification using Quadric Error Metrics"
date:   2016-11-05
tags: ['article', 'mesh', 'simplification', 'decimation', 'quadric', 'computer graphics']
author: "Hugo Viala"
---

# An article study series ?

Lately, I have been reading a lot of scientific papers about various topics : computer graphics, physics or mesh processing. I found that it is very interesting to have an insight about a particular field and understand what is at stake and where the current research efforts are today.

It stuck me that I could do this while watching [Casey Muratori's video on Papers we Love][1]. I wanted to get people excited about stuff happening in the scientific world and... it is not even that hard to understand really. So today, I give this idea a shot. My goal is to explain a particular scientific article, because of the repercussion it had or the new vision it brought, by outlining the basic scientific arguments it presents and highlighting the important ideas the authors contributed to. I assume that you are somewhat familiar with some concepts (basic linear algebra, ...) and I will try to explain notions as mush as possible, or give references to look up to if you are lost.

Being the first article study of the series, I chose to present a very famous article publised about 20 years ago on 3D polygon simplification who had a big impact on how the community understand mesh processing nowadays.

# Did you say mesh ?

In 3D graphics, for a game or for movie production for example, people usually handle 3-dimensional models of objets they want rendered. Those models are called meshes or polygon meshes and consist -- mostly -- of three things : vertices, edges and faces[^1]. Vertices are basically points with some data associated, typically position, normal and color. Edges are links between vertices and finally faces are triangles with three edges[^2]. This whole data caracterizes a mesh that then will be lighted, shadowed, animated and rendered by a rendering engine.


![Here a mesh from a dragon in Skyrim](/images/mesh_simplification/dragon_mesh.png)
<center>A triangle mesh of a dragon used in Skyrim</center>

# Surface Simplification. Why ?

The article I want to highlight today is called : *Surface Simplification using Quadric Error Metric* and was published in 1997 by Michael Garland and Paul S. Heckbert. This paper discuss how to simplify a given mesh into another one having less vertices / edges / faces.

But why is it an important matter ? Well because there is a trade-off to be found between performance and quality. A mesh having a lot of faces will be highly detailed, therefore will probably be a high-quality polygon. However, processing such a large amount of triangles will create performance issue : if you want to render a character mesh for a game, you only have a few milliseconds to process all the vertices in the mesh, so you might want to first *simplify* its geometric representation before processing it. Then again, the result will be of a lesser quality, but you will achieve better performance. It is up to you which trade-off you are aiming for.

![A mesh with several simplification steps](/images/mesh_simplification/mesh_simplification.png)
<center>Several steps of a mesh simplification</center>


With this framework in mind, you can now see what is the issue Garland and Heckbert were tackling 20 years ago : how to automate mesh simplification efficiently while not changing the overall structure of the mesh (i.e. minimizing the error between the input mesh and the result of the algorithm). 

More formaly, in the global surface simplification framework, there is a trade-off to be made between having a small amount of vertices but a big deviation from the original mesh, or a small deviation from the original mesh and still a big amount of vertices. Therefore, a rigorous mesh simplification problem is one of the following. Given an input mesh $M_i$ with $V_i$ vertices, an error function $\text{Err}$ (which measures of much two meshes deviates from one another), I want to have a resulting mesh $M_r$ with $V_r$ vertices such that either :

1. we want to minimize $V_r$ under the constraint $\text{Err}(M_i, M_r) \leq \epsilon$ for a given $\epsilon$
2. we want to minimize $\text{ Err}(M_i, M_r)$ under the constraint $V_r \leq M$ for a given $M$

In the first statement of the problem, we do not want the resulting mesh to deviate too much from the input mesh, while reducing the number of vertices as mush as possible. Whereas in the second statement, we want to reach a certain amount of maximum vertices and then be as close as possible from the input mesh. Those two statements looks quite the same, but there subtle difference lay in the constraint they set themselves (deviation or vertex count) to achieve their simplification goal.

The algorithm proposed by Garland and Heckbert can fullfill both of these statements. I will now explain the algorithm they proposed back then.

# Contracting edges

It is clear that, if you want to reduce the amount of vertices in a mesh in order to simplify it, you have to have a basic operator to apply to you mesh. The operator used by Garland and Heckbert is a very common yet useful one and is called the *edge contraction operator*. The basic idea is to take a pair of vertices $v_1$ and $v_2$, remove the edge between them and create a new vertex $\bar{v}$ as shown in the next diagram.


![Edge contraction operator](/images/mesh_simplification/edge_contraction_operator.png)
<center>Edge contraction operator</center>

This operator is formally written as

$$(v_1, v_2) \rightarrow \bar{v}$$

This is the main operation that will be performed by the algorithm in the course of the processing. There is, however, several issues raised by this operator concerning the topology of the resulting mesh that I will not discuss here[^3].

Now that we have this basic operation in our hand, we are one step closer to the result we are looking for. But there are still some holes to be filled : to which pair do we apply this operator to ? and how to found which optimal resulting vertex $\bar{v}$ to use ?

# Selecting vertex pairs

The first of the two question is actually pretty simple. We have to give some criteria to select which pair will be eligible to be contracted. Garland and Heckbert, in their paper, proposed the following. They defined a pair of vertices $(v_1, v_2)$ to be *valid* for contraction if 

1. $(v_1, v_2)$ is an edge in the input mesh
2. $\|\|v_1 - v_2\|\| < t$ where t is a threshold parameter
<!-- comment to avoid vim parser to be confused -->

$t$ is a parameter that we can play with. If $t = 0$, then only the actuals edges of the mesh will be collapsed. This will have the benefit of not touching the overall topology of our polygon, but may cause pairs that would have been interesting to collapse to stay untouched. On the other hand, if $t$ is too big, two vertices that are unrelated - because they are very far away from each other in the initial mesh - may be collapsed, causing the resulting mesh to deviate too much from the original one.


# Chosing the optimal contraction result from quadric error

Now we know we want to perform a contraction $(v_1, v_2) \rightarrow \bar{v}$ on a valid pair. The question now is : where do I place the resulting vertex $\bar{v}$ ? What should the result of the contraction be ?

Well, this we do not want the resulting mesh to deviate too much from the original one, we ought to place this new vertex carefully, at a place where it will minimize the overall error between $M_i$ and $M_r$. For this, we need to define what is the error we make by replacing the pair $(v_1, v_2)$ by $\bar{v}$.

Garland and Heckbert first noticed that, considering one vertex $v_1$, each triangle that had $v_1$ as one of its vertices was defining a plane from which $v_1$ was a part of. One could also compute the squared distance from a vertex $v$ and that very plane. Indeed, if $v = (x, y, z, 1)$ and the plane $P$ had the equation $ax + by + cy + d = 0$ then we would have

$$\text{d}(v, P)^2 = v^\top Q_P v$$

<!-- TODO(hugo) : better explanation of what Q is -->
where $Q_P$ is a 4x4 symmetric matrix representing a quadric matrix of the plane.

$$ Q_P = \left ( 
\begin{array}[cccc]\\
a^2 & ab & ac & ad \\
ab & b^2 & bc & bd \\
ac & bc & c^2 & cd \\
ad & bd & cd & d^2
\end{array}
\right)$$

So that computing a distance of a vertex to a plane boils down to matrix multiplication. Quite easy ! 
We can define $Q_v$ the *quadric matrix of a vertex* $v$ as the sum of all the quadric matrix of the planes of the triangles that have $v$ as a vertex. If $\mathbb{P}$ is the set of the planes supported by triangles of $v$ then

$$Q_v = \sum_{P \in \mathbb{P}} Q_P$$


Now we can define the error of a vertex contraction. Considering the operation $(v_1, v_2) \rightarrow \bar{v}$, we have, as previously defined, a matrix $Q_1$ and a matrix $Q_2$ at hand (one for each vertex). So, for a vertex $\bar{v}$ we can now define the error $\Delta$ by

$$\Delta(\bar{v}) = \bar{v}^\top \left( Q_1 + Q_2 \right) \bar{v} = \bar{v}^\top Q_e \bar{v}$$

where $Q_e$ is the final quadric error estimator[^4].

Phew. So we have defined what is the error we do when we contract a pair into one vertex. Now, our goal is to minimize this error, i.e. finding $\bar{v}$ so that $\Delta(\bar{v})$ is minimal.

If we rewrite $Q_e$ so that

$$ Q_e = \left ( 
\begin{array}[cc]\\
A & -f \\
-f^\top & g \\
\end{array}
\right)$$

then, one can show by computing direct derivative along $x$, $y$ and $z$ that minimizing $\bar{v}^\top Q_e \bar{v}$ is equivalent to solving the system

$$ Ax = f$$

Now, either $A$ is invertible, and we directly have our optimal point $\bar{v}$ at hand. If $A$ is not invertible, we can always fall back to some simple, but less precise methods such as chosing $\bar{v} = \left( v_1 + v_2\right) / 2$ for example.

# Algorithm outline

We have now defined all the concepts necessary to provide an algorith. Indeed, we know how to contract vertices, which one to contract, and what the result of the contraction should be. We now need a efficient way to process the mesh. Garland and Heckbert proposed the following steps.

1. Compute the quadric matrix $Q$ of each vertex.
2. Compute the list of all valid pairs of vertices.
3. Compute the optimal contraction target $\bar{v}$ for each valid pair. The *cost* of that pair is defined as $\Delta(\bar{v})$
4. Sort the list of pairs with increasing cost (so that the pair of lowest cost is first)
5. Iteratively removed the pair of lowest cost from the list. Contract this pair. Update the costs of all valid pair involving the contracted vertices.

You can stop this algorithm whenever you have reach the goal you set yourself in the third section, either you have reach the amount of vertex you want to have, either contracting another pair would make the overall error too big.

# Why this is great and where to go from there

The algorithm presented by Garland and Heckbert in 1997 works quite well and efficiently on a large number of meshes without destroying too much their topology.

![Result of algorithm](/images/mesh_simplification/cow_simplified.png)
<center>Result of the algorithm</center>

What is so great about this algorithm is that it first proposes to contract pair that do not have a big impact on the resulting mesh (i.e. those that have the lowest cost) while still constantly reducing the amount of vertices.Secondly, the heuristic chosen, the quadric, to evaluate the error made by a compression provides less coarser result for the pair contraction.

Even though this article was published almost 20 years ago, it outlined the basis for most mesh simplification algorithm. For example, we could talk about the article *Structure Aware Mesh Decimation* that takes this very idea of edge contraction, but gives a more refined and precise heuristic to evaluate the error of the contraction.

# References

- [The original article by Garland and Heckbert](http://mgarland.org/files/papers/quadrics.pdf)
- An extention of the article : [Structure Aware Mesh Decimation](https://hal.inria.fr/hal-01111203/file/structure%20(1).pdf)
- [Meshlab](http://meshlab.sourceforge.net/) : an open-source mesh manipulation mesh in which mesh simplification is available

---

[1]: https://www.youtube.com/watch?v=SDS5gLSiLg0
[^1]: We can define more generally a mesh in the context of *algebraic topology* with the notion of *simplicial complex*. This theory is called *homology theory* and you might want to learn more about this by googling those terms if you have a strong mathematical background.
[^2]: Faces in the mesh are not always triangles. They can be quads for examples. Triangles are widly used however.
[^3]: For more details, you can refer to the original article. Fully explaining the topology issues at stake here is out of the scope of this article.
[^4]: Notice that this estimator is the sum of the squared distances of the planes of $v_1$ and $v_2$ to $\bar{v}$
