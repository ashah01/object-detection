# [Efficient Graph-Based Image Segmentation](http://cs.brown.edu/people/pfelzens/papers/seg-ijcv.pdf)

Let $G = (V, E)$ be a graph with vertices $v_i \in V$, the set of elements to be segmented, and edges $(v_i,v_j) \in E$ corresponding to pairs of neighbouring vertices.

Each edge $(v_i, v_j) \in E$ holds a weight representing the dissimilarity between neighboring elements $v_i$ and $v_j$. The metrics for dissimilarity comprise of intensity, colour, motion, location, or other attributes. 

Any segmentation is induced by a subset $E' \subset E$. To quantifiably model regions that should be similar, edges in subset regions $E'$ should have low weights, while edges in different components should have higher weights. 

## Pairwise Region Comparison Predicate
How do we evaluate whether there's a boundary between two components. The occam razor naive neuristic is to simply compare the dissimilarity between adjacent elements along the boundary with that of neighboring elements. The internal difference of a component $C \subset V$ is defined to be the largest weight in the minimum spanning tree of the component $MST(C, E)$

$$Int(C) = \underset{e\in{MST(C, E)}}{max}w(e)$$

The difference between two components is defined to be the minimum weight edge connecting the components

$$Dif(C_1, C_2) = \underset{v_i\in{C_1,v_j\in{C_2,(v_i,v_j)\in{E}}}}{min}w((v_i,v_j))$$

This means we can define the predicate $D(C_1, C_2)$ as:

$$D(C_1, C_2) = \begin{cases}1 & Dif(C_1, C_2) > MInt(C_1, C_2),\\0 & Dif(C_1, C_2) \leq MInt(C_1, C_2)\end{cases}$$

## Segmentation Algorithm $O(m\ log\ m)$

Input: A graph $G = (V, E)$
Output: A segmentation of $V$ into components $S = (C_1,...C_r)$

1. Sort $E$ into $\pi = (o_1,...,o_m)$ by non decreasing edge weight
2. Start with a segmentation $S^0$, where each vertex $v_i$ is in its own component
3. Repeat for $q = 1,...,m$:
	0. Construct $S^q$ given $S^{q-1}$ as follows.
	1. $v_i$, $v_j$ = the vertices connected by the $q$-th edge (i.e $o_q = (v_i, v_j)$)
	2. if $v_i$ and $v_j$ are in disjoint components of $S^{q-1}$ and $w(o_q)$ is small compared to the internal difference of both components, then merge the two components
4. Return $S = S^m$
