---
layout: post
title:  totally unimodular, bro
date:   2025-12-23
description: in which we characterize regularity and catch the vibe of nice optimization problems, ya feel
tags: polytope matroid linear-algebra optimization
categories: combinatorics algebra
related_posts: false
giscus_comments: true
tikzjax: true 
toc:
  beginning: true
---

This quarter, a lot of courses that I attended this quarter shifted my perspectives on various topics I thought I already knew a decent amount about. For instance, the combinatorial optimization class this quarter I took felt in part like a Kleinberg/Tardos redux, but with a much more combinatorial bent. I felt like I had already come to be friends with the minimum spanning tree problem, network flows, etc. as an undergraduate, but I'd never seen their connection to polytopes and linear programming before this quarter. I especially had no idea that there was some structure that unified these problems that indicated that they were more tractable in some sense compared to other combinatorial problems. 

To procrastinate on grinding out my writing milestone, I'd like to articulate an algebraic property that unifies all of these problems, describe how all of these basic algorithms questions are related to it, and show that it's in fact a computationally nice property (even if it may not seem like it initially). 

### an elementary application of linear algebra
As with many things, the property we care about that secretly relates all of these problems is algebraic: 

> _Defintion._ An $$n \times n$$ matrix $$A \in \RR^{n \times n}$$ is _unimodular_ if its determinant is $$\pm 1$$. 

Unimodular matrices show up in combinatorics a lot, especially if $$A \in \GL_n(\ZZ)$$. In this case, because of the adjugate formula for matrix inversion, if $$A$$ is unimodular, then $$A^{-1} \in \GL_n(\ZZ)$$ (instead of only known to be an element of $$\GL_n(\QQ)$$). This is especially nice, for instance, if you have some sort of change-of-basis relation between two quantities that have nice linear relations between them, and you'd like to know that the coefficients of this change of basis are integral.[^1] If these coefficients are integral, one can then really naturally ask what exactly it is that the basis coefficients count, which really is one of The Classic Combinatorial Questions Of All Time. 

We'd actually like a stronger algebraic condition than just unimodularity, though: 

> _Definition._ A matrix $$A$$ is _totally unimodular_ if every square minor has determinant $$0, \pm 1$$. (In particular, any totally unimodular matrix has entries in $$\set{0, \pm 1}$$.)

This feels like an annoying condition to check for a matrix. Naively, this requires an exponential-time check for an arbitrary $$m \times n$$ matrix, since you'd have to iterate over all subsets of rows and columns and take determinants. Taking the determinant is actually not the issue -- this operation is probably roughly $$O(n^3)$$ for an $$n \times n$$ matrix (I think probably doing row-reduction and tracking the elementary operations used in the process suffices). The issue is more structural, since it seems like there's just so many conditions to check, even though doing all of these checks feels mildly redundant. 

--- 

Maybe it's worth immediately saying a few things out of the gate about why such a property would be nice. One nice application has to do with linear and integer programming. Recall that a polytope $$P$$ can be described as a (bounded) polyhedron of the form 

$$
P = \set{ x \in \RR^n : Ax \le b}
$$

for some $$A \in \RR^{m \times n}$$, $$b \in \RR^n$$. Then one can consider the _linear program_ on $$P$$, given by 

$$
\max \set{c^\top x : x \in \RR^n, Ax \le b} = \max \set{c^\top x : x \in P}
$$

or the corresponding _integer program_ on $$P$$, $$\max \set{c^\top x : x \in P \cap \ZZ^n}$$ where now we force $$x \in \ZZ^n$$ instead of just $$\RR^n$$. For a linear program, the optimum value of $$c^\top x$$ is achieved at some vertex of $$P$$, and a number of methods can compute this relatively quickly in polynomial time[^3].
However, being able to do integer programming fast is NP-complete. If you can solve an arbitrary integer programming problem in polynomial time, congratulations, you've won a million dollars and broken complexity theory as we know it. 

In general, many combinatorial optimization questions can be formulated as _integer_ programs (since the search space is over discrete quantities). Short of solving a Millenium Prize problem, it would be nice to know whether a combinatorial optimization question/integer program can be solved quickly with linear programming methods. In general, there is no hope of having an optimal solution to the linear program to even be a good approximation to the optimal solution to the integer program -- think about polytopes that are super skinny and have very few lattice points, if any. For this to be a reasonable strategy, one would essentially require the linear program and the integer program to coincide, so that the vertices of our polytope $$P$$ are _integral_ (i.e. having all coordinates integers). This has no business being a phenomenon among general linear programs, but it turns out determining when this is the case is dependent precisely on our matrix $$A$$ having the aforementioned linear algebraic property: 

> _Theorem (Hoffman-Kruskal, 1956)._ Given a polytope $$P = \set{x \in \RR^n : Ax \le b}$$, $$P$$ is integral iff $$A$$ is totally unimodular. 

The forwards direction is relatively straightforward, but the reverse direction requires some work. I'll omit both arguments for brevity here. 

With this context in mind, one would love to be able to test a matrix $$A$$ for total unimodularity, since it would make the difference between solving integer programs quickly (now being able to apply continuous methods to a discrete problem). It'll turn out that this can also be done in polynomial time, and we'll see what the structure of totally unimodular matrices looks like and how one might perform this test quickly in what follows. 

#### uhh... wait, do you have an example? 
yes! the identity matrix :smile: 

Okay, you deserve better examples than that. Let's first describe some operations that preserve total unimodularity, for posterity: 

- Multiplying a row/column by $$-1$$
- Permuting rows/columns
- Taking submatrices
- Taking the transpose
- Pivoting at an entry that is $$\pm 1$$ (taking an element that is $$\pm 1$$ in our matrix as a pivot in row-reduction, eliminating all elements in its column).

These are generally easy to see. 

Still, none of these properties constitute a proper example of a totally unimodular matrix though. Fine. Here's a nice class of matrices that is totally unimodular: 
> _Proposition._ Suppose $$A \in \set{0, \pm 1}^{m \times n}$$ is a matrix with at most one $$1$$ and at most one $$-1$$ in every column. Then $$A$$ is totally unimodular.

The proof I have in mind for this statement is not terribly illuminating, and goes by induction. Consider a $$k \times k$$ minor $$M$$ of $$A$$. If $$k = 1$$, we are good to go. Otherwise, if $$k > 1$$, we can do casework on what the columns of $$M$$ look like. If $$M$$ has a column of zeroes, then $$\det(M) = 0$$ already. Similarly, if $$M$$ has exactly one $$1$$ or $$-1$$ in a column, then $$\det M$$ is (up to sign) the determinant of a $$(k-1) \times (k-1)$$ minor of $$A$$, so $$M$$ satisfies the required determinantal condition by induction. If not, then $$M$$ has exactly one $$1$$ and $$-1$$ in every column, then $$\mathbf{1} M = 0$$, where $$\mathbf 1$$ is the all-ones vector. Hence $$M$$ is non-invertible, so $$\det(M) = 0$$. $$\square$$

What's notable about this proposition is that this is sufficient to give us some hints as to why graphs might give rise to total unimodular matrices. Consider the (signed) incidence matrix $$M_D$$ of any digraph $$D = (V, A)$$, with rows indexed vertices and columns indexed by arrows connecting two vertices. Then every column has exactly one $$1$$ and one $$-1$$, and by the above we see that $$M_D$$ is totally unimodular. 

Note that if we have a digraph $$D$$ with source and sink $$s, t \in V$$, then the condition that a function $$f : A \to \RR_{\ge 0}$$ is a flow can be rephrased as a polytopal condition $$\set{x \in \RR^A_{\ge 0} : M_D' x = 0}$$, where we drop the rows corresponding to $$s$$ and $$t$$ in $$M_D$$. If we additionally impose a capacity function $$c : A \to \RR_{\ge 0}$$, we can still pretty straightforwardly show that adding the constraint $$x \le c$$ to our polytope gives an integral polytope (often called the _flow polytope_), following from the total unimodularity of $$M_D$$. Then, even if we were to attempt to solve max-flow via converting to our problem to a linear program, we see that we should be able to solve this problem in polynomial-time given integer capacities (although we are given no assurances about the existence of nice combinatorial algorithms like Ford-Fulkerson, Push-Relabel, etc.). Moreover, we claim that the Max-Flow Min-Cut Theorem is then an easy corollary of the duality theorem for linear programs, which is super neat!

This proposition is also useful in the context of certain undirected graphs. Suppose $$G = (V, E)$$ is a bipartite graph, and let $$A$$ be its adjacency matrix. Since $$G$$ is bipartite, suppose the parts are $$V = U \sqcup W$$. We can multiply all of the rows in $$A$$ corresponding to vertices in one of the parts (say, $$U$$) by $$-1$$ to get a matrix with exactly one $$1$$ and one $$-1$$ in every column. Undoing these sign flips, we see that $$A$$ was also totally unimodular. 

Unfortunately, this is the only case where the adjacency matrix of a graph is totally unimodular. If $$G$$ were not bipartite, it would have a cycle of odd length, and the minor corresponding to the adjacency matrix of this cycle would have determinant $$\pm 2$$. There's maybe some way to say positively what propert of bipartite graphs makes its adjacency matrix totally unimodular, but this feels a bit more awkward to articulate, so I won't do so here.

This statement sort of indicates why maximum bipartite matching should be a nice combinatorial optimization problem. Note that finding the maximum-size matching problem on a graph $$G$$ can be encoded as a linear program: 

$$
\max \set{\mathbf 1^\top x : x \ge 0, A x \le \mathbf 1}
$$

When $$G$$ is bipartite (equivalently, the adjacency matrix is totally unimodular), the polytope over which we're doing this linear program is integral. This is called the _matching polytope_, and we see that the vertices admit a nice combinatorial description -- namely, they must be the indicator vectors of matchings on $$G$$. The existence of a nice combinatorial algorithm for maximum bipartite matching (via an application of max-flow, for instance) is similarly indicated by the integrality of this polytope. Konig's Theorem stating that on a bipartite graph, the maximum-size matching and the minimum-size vertex cover are equal, now similarly follows from duality. There's a number of other interesting things to say about things related to matchings and polytopes, but I'll forgo them here.[^5]

The case of totally unimodular matrices coming from the node-incidence matrices of digraphs can be generalized to a much larger class of matrices: 

> _Definition._ Let $$D = (V, A)$$ be a digraph, and fix a directed spanning tree $$T = (V, A')$$ on the vertices of $$D$$. A _network matrix_ $$M$$ (with respect to $$D$$ and $$T$$) has rows indexed by the arrows $$A$$ and columns indexed by arrows in $$A'$$ such that $$M_{aa'} \in \set{0, \pm 1}$$. Suppose an arc $$a \in A$$ goes from $$u \to v$$. Then, we look at the path from $$u$$ to $$v$$ in $$T$$, and set $$M_{aa'} = \pm 1$$ depending on if $$a'$$ is traversed in the forwards/backwards direction along this path, respectively, and $$0$$ otherwise. 

> _Proposition._ All network matrices are totally unimodular. 

The proof of this is slightly more difficult, but I'll sketch one here. 

_Proof (Sketch)._ Consider a $$k \times k$$ minor of $$M$$, given by choosing subsets of arrows $$B \subseteq A$$, $$B' \subseteq A'$$. First, we claim that this matrix must itself be a network matrix. Consider contracting all arrows of $$T$$ in $$A' - B'$$ to get a new spanning tree $$T'$$ on a new vertex set $$V'$$, and perform the same identifications of vertices on $$D$$ to produce a corresponding digraph $$D'$$. We claim that this minor is the network matrix with respect to this $$D', T'$$. This allows us to reduce the problem to the special case when $$\abs A = \abs{A'}$$ so our network matrix is square, and show that its determinant is one of $$0, \pm 1$$. 

Now, if $$D$$ contains an undirected cycle among its arrows, then we can construct an element of the kernel of the network matrix, so the determinant of the network matrix is zero. Otherwise, $$D$$ is also a directed spanning tree, in which case the network matrix with respect to $$T, D$$ is the inverse of our given matrix. But then any element of $$\GL_n(\ZZ)$$ has determinant $$\pm 1$$, as desired. 

---

These are still propositions as opposed to examples proper, but I feel like having a go-to mental model for this property is really useful, since it feels fairly intractable to come up with a random example of a matrix and check if it is totally unimodular otherwise. 

We'll now turn to a different area of combinatorial optimization to analyze the structure of totally unimodular matrices: 

### the regulars are coming!
It turns out that totally unimodular matrices are intimately related to _matroids_! From the point of view of combinatorial optimization, the one-sentence tagline is that _matroids are precisely objects on which the greedy algorithm produces an optimal result_. I would rather not rehash the basics of matroids fully in detail here, but I'll say enough to get off the ground. 

Recall[^4] that a matroid $$M = (E, \mc I)$$ on (finite) ground set $$E$$ can be thought of a collection of subsets $$\mc I \subseteq 2^E$$ (called independent sets) that is non-empty, closed under inclusion, and satisfies the axiom that if $$S, T \in \mc I$$ with $$\abs S < \abs T$$, we can find some $$t \in T - S$$ such that $$S \cup \set t \in \mc I$$. Matroids combinatorially generalize the idea of having linearly independent subsets of a (finite) collection of vectors. In particular, _realizable_ (or _representable_ or _linear_) matroids are precisely those matroids whose collection of independent sets comes from the linearly independent subsets of a ground subset of vectors in $$\FF^n$$ for some field $$\FF$$. One says generally that a matroid is _realizable over a field $$\FF$$_ (or _representable_) if such a collection of vectors exists in $$\FF^n$$. Specifically, if a matroid is realizable over $$\FF = \FF_2$$, then it is a _binary_ matroid.

Most matroids are actually not realizable (for the amount of them that are, think "measure zero"). we're going to restrict our attention further to the _regular_ ones, which are very special: 

> _Definition._ A matroid is _regular_ if it is realizable over _every_ field $$\FF$$. 

Why talk about matroids? Well, here's the kicker:

> _Theorem_. A matroid is _regular_ iff it is the linear matroid corresponding to a totally unimodular matrix. 

The reverse direction is actually fairly clear. Suppose we had a totally unimodular matrix and took its columns as the elements of a linear matroid $$M$$. Without loss of generality, suppose our matrix is full-rank of rank $$r$$ and consider a basis of this matroid indexed by $$r$$ columns of this matrix. Then this collection of columns is a basis iff the corresponding $$r \times r$$ minor is invertible. Since all entries are in $$\set{0, \pm 1}$$ and so are the determinants of every minor, this minor is invertible over $$\QQ$$ iff it is invertible over $$\FF$$ for a field $$\FF$$ of any characteristic $$p \ge 2$$. Hence this matroid is regular. 

Oxley's classic book _Matroid Theory_ describes the forwards direction (crediting this argument to Brylawski, I think), which unsurprisingly requires us to perform a fairly involved construction of our matrix, but also proves a more general statement. It is actually sufficient to show that if a matroid is binary and realizible over some other field of characteristic not equal to $$2$$, then there is a totally unimodular matrix that realizes it. To do this, Oxley describes how one can construct representations over a general field $$\FF$$ and demonstrates that if a representation for a matroid over $$\FF$$ exists, given a free choice of parameters over $$\FF$$, one can construct any other representation depenedent on these parameters (and is even achievable from our original representation via standard matrix operations![^6]). If these parameters are chosen nicely, we can then construct a totally unimodular representation for our regular matroid. Let's see how this works!

---

Given a matroid $$M$$ realizable over a field $$\FF$$, let $$E = \set{e_1, e_2, \dots, e_n}$$, where the basis $$B = \set{e_1, \dots, e_r}$$ is the collection of standard basis vectors in $$\FF^r$$ (where the rank of $$M$$ is $$r$$) and $$\set{e_{r+1}, \dots, e_n}$$ are the columns of a matrix $$D$$ (so that our matrix representation of $$M$$ looks like $$\tbmat{I_r & D}$$). Note that for every $$r < k \le n$$, $$\set{e_k} \cup B$$ is obviously dependent, containing the _fundamental circuit_ $$C(e_k, B)$$ (minimal dependent set) $$\set{e_k} \cup \set{e_i : i \in [r], D_{ik} \neq 0}$$. Recording the data of these circuits for all such $$k$$ therefore corresponds to tracking whether entries in $$D$$ are zero or not. This motivates us to consider the matrix $$D^\#$$, which replaces all nonzero entries in $$D$$ by $$1$$s, called the _B-fundamental-circuit incidence matrix_. 

Observe that this matrix $$X$$ always exists for any matroid, given that we fix an ordering on the ground set $$\set{e_1, \dots, e_n}$$, where $$B = \set{e_1, \dots, e_r}$$ and let $$X_{ik} = [i \in C(e_k, B)]$$, where $$i \in [r]$$ and $$r < k \le n$$ (so $$X$$ is a $$r \times (n-r)$$ $$0-1$$ matrix). Then, given a matroid, finding a representation over $$\FF$$ corresponds to picking entries in $$\FF$$ for every $$1$$ in the fundamental-circuit incidence matrix so that the rest of the circuits work out, so that the other fundamental circuits are encoded appropriately. In this sense, $$D^\#$$ is sometimes called a _partial representation_ for our matroid $$M$$. This matrix is actually unique up to picking a basis for $$M$$ over any field $$\FF$$ -- it turns out that if we have two representations $$\tbmat{I_r & D_1}$$ and $$\tbmat{I_r & D_2}$$ over fields $$\KK_1$$ and $$\KK_2$$, then arguing using the circuits of $$M$$, $$D_1^\# = D_2^\#$$. (In particular, if $$\KK_1 = \KK_2 = \FF_2$$, then we see that binary matroids actually have a unique representation $$\tbmat{I_r & D^\#}$$.) 

We'll also need the following lemma when dealing with two representations of $$M$$ over different fields, which is also essentially linear algebra: 

> _Lemma._ Suppose matroids $$M_1$$ and $$M_2$$ (on the same ground set $$[n]$$) have representations $$\tbmat{I_r & D_1}$$ and $$\tbmat{I_r & D_2}$$ over fields $$\KK_1$$ and $$\KK_2$$. Then $$M_1 \cong M_2$$ iff for any square submatrices $$D_1'$$ and $$D_2'$$ of $$D_1$$ and $$D_2$$, respectively, that $$\det D_1' = 0$$ exactly when $$\det D_2' = 0$$.  

In our construction, given a fundamental-circuit incidence matrix $$X$$ for the matroid $$M$$, we will use an auxiliary bipartite graph $$G(X)$$ built from the data of $$X$$. In particular, $$G(X)$$ has vertex set $$[n] = [r] \sqcup ([n]-[r])$$, where we have the edge $$\set{i, k} \in E(G(X))$$ iff $$X_{ik} \neq 0$$. We will use the graphic matroid $$M(G(X))$$ to help construct our representation. First, we state a result about the flexibility of our choice of representation over a field $$\FF$$: 

> _Proposition._ Suppose $$M$$ is a matroid realizable over a field $$\FF$$ with representation $$\tbmat{I_r & D}$$. Let $$\set{b_1, \dots, b_k}$$ be a basis of $$M(G(D^\#))$$, where $$k = n - c(G(D^\#))$$ and $$c(G)$$ is the number of connected components of $$G$$. Then for any $$(t_1, \dots, t_k) \in \FF^k$$, there is a  $$\FF$$-representation of $$M$$, $$\tbmat{I_r & D'}$$ where the entries corresponding to each $$b_i$$ in $$D'$$ is $$t_i$$, obtainable via standard matrix operations[^6] from $$\tbmat{I_r & D}$$. 

The proof is frankly not very illuminating -- it proceeds by induction on the size of a forest/independent set in $$M(G(D^\#))$$ and ensuring that one can cook up a representation for any size independent set $$S \in \mc I(G(D^\#))$$ and a parameter in $$\FF$$ for every edge in our forest. If $$S = \nullset$$, this is easy -- otherwise, by induction using the fact that a forest with at least one edge has a leaf, one can tweak the entries of our representation strategically to get our new representation. (This representation is even unique in some sense, but this will not be necessary, I think.) 

Okay, with this, we have enough to start writing down a proof of the forwards direction. Consider a representation of our matroid over $$\FF$$ of the form $$\tbmat{I_r & D}$$. In order to get a totally unimodular representation using the above proposition, a natural choice is to pick $$\mathbf{1} \in \FF^k$$ where $$k = n - c(G(D^\#))$$, the all-ones vector. We can therefore perform matrix operations on our representations to force at least some of the entries in a representation of $$M$$ to be $$1$$. However, this proposition on its own a priori doesn't tell us what the rest of the entries look like -- we need more control over them. But behold: 

> _Lemma (Brylawski, 1975)._ Let $$\tbmat{I_r & D}$$ be an $$\FF$$-representation for a binary matroid $$M$$ (where $$\chr \FF \neq 2$$). Let $$B_D$$ be a basis for the matroid $$M(G(D^\#))$$. If every entry of $$D$$ corresponding to an edge of $$B_D$$ is $$1$$, then every other non-zero entry of $$D$$ has a uniquely determined value in $$\set{1, -1}$$. 

_Proof._ Pick a nonzero entry $$d \in D$$ not in the basis $$B_D$$ corresponding to an edge $$e_d \in E(G(D^\#))$$. Let $$C_d$$ be the circuit containing $$e_d$$ in $$B_D \cup \set{e_d}$$, so that among the entries corresponding to $$C_d$$, our desired entry is the only indeterminate one. We now show that this entry must be in $$\set{\pm 1}$$ by induction on $$k = \frac 12 \abs{C_d}$$. Observe that $$k \geq 2$$. 

Let $$D_d$$ be the $$k \times k$$ submatrix of $$D$$ indexed by the vertices traversed by $$C_d$$. $$D_d$$ might contain other non-zero entries $$d'$$ not in the cycle $$C_d$$. The edge $$e_{d'}$$ is therefore some diagonal of the cycle $$C_d$$, and in particular, looking at the cycles $$C_d$$ and $$C_{d'}$$, we see that $$\abs{C_{d'}} < \abs{C_d}$$ and therefore the entry corresponding to $$d'$$ must be in $$\set{\pm 1}$$ by induction. This means that every entry of $$D_d$$ is in $$\set{0, \pm 1}$$ except possibly the one corresponding to $$d$$. 

We will now reduce to the case where $$D_d$$ contains no nonzero entries other than the entries corresponding to edges in $$C_d$$. If there are other edges in the subgraph induced by the vertices of $$C_d$$, simply pick the cycle $$C_d'$$ of shortest length containing $$e_d$$, and let $$C_d = C_d'$$, with $$D_d$$ being the corresponding submatrix. Now, taking the determinant of the submatrix $$D_d$$, $$\det D_d = \pm 1 \pm d$$. Since $$M$$ is binary, over $$\FF_2$$, the representation of $$M$$ is $$\tbmat{I_r & D^\#}$$, and we see that $$\det D_d^\# = 0$$. We now invoke the lemma above about two representations of the same matroid over different fields, and hence $$\det D_d = 0$$ over $$\FF$$. This means $$d \in \set{\pm 1}$$ as well, and is determined uniquely by our choice of $$C_d'$$, completing the proof of the lemmas. $$\square$$

The above lemma now ensures that we have an $$\FF$$-representation $$\tbmat{I_r & D}$$ whose entries are in $$\set{0, \pm 1}$$. Note that if we now view this matrix over $$\QQ$$, it actually suffices to show that this representation is indeed totally unimodular. By our field-switching lemma above, it suffices to show that for any $$k \times k$$ submatrix $$D'$$ of $$D$$, that $$\det D' = 0$$ over $$\FF$$ iff $$\det D' = 0$$ over $$\QQ$$, and in fact only the forwards direction requires nontrivial checking. Suppose $$\det D' \neq 0$$ over $$\QQ$$. We will essentially row-reduce this matrix and show that in doing so, we never get any entries that are not in $$\set{0, \pm 1}$$ in any intermediate stage. If we're row-reducing using some entry $$D'_{st}$$ as a pivot, then for an entry $$D'_{ij}$$ in $$D'$$ after this row reduction, we replace $$D'_{ij} \gets D'_{ij} - D'_{it} D'_{sj} (D'_{st})^{-1} = (D'_{st})^{-1}(D'_{ij} D'_{st} - D'_{it} D'_{sj})$$. If this value is not in $$\set{0, \pm 1}$$, it must be in $$\set{\pm 2}$$. But since the submatrix $$\bmat{D'_{st} & D'_{sj} \\ D'_{it} & D'_{ij}}$$ (up to swapping rows/columns) has determinant $$0$$ over $$\FF_2$$, it must have determinant $$0$$ over $$\QQ$$. By our field-switching lemma for matroids above, this implies that $$0 \in \set{\pm 2}$$, contradiction. Hence we can iteratively row-reduce our matrix $$D'$$ to a permutation matrix (with signs), which has determinant $$\pm 1$$ over $$\QQ$$, as desired. This shows that the representation we constructed using our proposition was in fact totally unimodular! $$\blacksquare$$

It feels like we are really doing a bit of finite field craftiness over $$\FF_2$$ here, where we're using the fact that over $$\FF_2$$, if something isn't zero, it's one. There's a good amount of linear algebra shenanigans when it comes to careful field-switching here, but at its core, the construction of our desired representation is graph-theoretic at the end of the day (coming from trees and forests), which is neat. 


#### minors strictly forbidden
it's a fairly popular idea in matroid theory to characterize a property $$P$$ of matroids by a list of matroid minors that a matroid $$M$$ must not have in order for $$M$$ to have property $$P$$. Usually, one can check without too much trouble that the collection of matroids that have $$P$$ are closed under taking minors, but it's a major undertaking to work out the list of forbidden minors corresponding to any given property. For the property of representability over the finite fields $$\FF_q$$, for instance (where $$q = p^k$$ for primes $$p$$ and exponents $$k$$), I don't think it's known what this list is (or even if the list is finite) for $$q \ge 7$$, say. _Rota's conjecture_ says this list is finite but this is still pretty wide open, I think. 

Nevertheless, in the case where $$q = 2$$, it's pretty easy to do -- it turns out the only forbidden minor for binary matroids is the uniform matrid $$U_{2,4}$$ (famously non-graphic, which is sufficient to show that it's not binary). (The reverse direction requires a bit of casework, I think, but you can find this characterization in _Gordon-McNulty_ probably written up fairly nicely.) Actually, regularity has a nice forbidden minor characterization as well. 

Tutte (1958) originally proved this with his _homotopy theorem_,[^2] but having found and read the original paper, it's a bit of a "long and difficult" argument (as per Oxley). It does seem like there are interesting homological constructions involved, but it's maybe not what modern folks think about (and it uses the terminology of "flats" in a fairly archaic way). Oxley presents Gerards' proof of this theorem: 

> _Theorem (Tutte)._ A matroid is regular iff it does not have any minor isomorphic to $$U_{2,4}, F_7$$, or $$F_7^*$$ (where $$F_7$$ is the Fano plane). Equivalently, a binary matroid is regular iff it does not have any $$F_7$$ or $$F_7^*$$ minors. 

One of these directions is easy -- the reverse direction, since $$F_7$$ and its dual are only realizable over $$\FF_2$$ and not over fields of nonzero characteristic. The reverse direction is the one that requires work.

Suppose our binary matroid $$M$$ has some representation $$\tbmat{I_r & D}$$ over $$\FF_2$$. Then $$M$$ is regular iff we can add signs to some of the entries of $$D$$ to produce a totally unimodular matrix (what we call a _totally unimodular signing_). The Fano plane and its dual have the representations $$\tbmat{I_3 & X_F}$$ and $$\tbmat{I_4 & X_F^\top}$$, where 

$$
X_4 = \bmat{1 & 1 & 1 & 0 \\ 1 & 1 & 0 & 1 \\ 1 & 0 & 1 & 1}.
$$

Taking matroid minors corresponds to taking minors/submatrices of a representation, so our statement reduces to proving: 

> _Theorem._ Let $$D$$ be a $$0-1$$ matrix. If $$D$$ has no totally unimodular signing, then over $$\FF_2$$, the matrix $$\tbmat{I_r & D}$$ can be turned into $$\tbmat{I_3 & X_F}$$ or $$\tbmat{I_4 & X_F^\top}$$ via deleting/permuting rows/columns or pivoting (selecting a nonzero element as a pivot and performing row-reduction). 

This requires a little bit of graph theory and some facts about totally unimodular matrices, and then some careful constraining to extract some part of our matrix $$D$$ that must look like $$X_F$$ (up to pivoting and rearranging columns). I'm just going to state all of the preliminaries: 

> _Lemma._ Let $$G$$ be a simple, connected, bipartite graph such that deleting any two distinct vertex from the same part of $$G$$ gives a disconnected graph. Then $$G$$ is a path or a cycle. 

_Proof (sketch)._ If not, then $$G$$ has a vertex of degree at least three, and therefore has a spanning tree with a vertex of degree at least three. But then we have at least three leaves in such a spanning tree, and delete two that lie in the same component. $$\square$$          

> _Lemma._ Let $$D \in \set{0, \pm 1}^{n \times n}$$. If $$G(D^\#)$$ is a cycle, $$D$$ is totally unimodular iff the number of negative entries in $$D \equiv 0 \pmod 2$$. 

This is not too bad -- just compute the determinant of this matrix (see Oxley, Chapter 10 for details). 

> _Proposition._ Let $$D_1$$ and $$D_2$$ be totally unimodular. If $$D_1^\# = D_2^\#$$, then $$D_2$$ can be obtained from $$D_1$$ by multiplying some rows and columns by $$-1$$. 

This last one is fairly nontrivial and requires some graph theoretic arguments on the bipartite graph $$G = G(D_1^\#) = G(D_2^\#)$$ using the first two lemmas. I think this is worth some amount of exposition!

_Proof (sketch)._ Call an edge of $$G$$ _even_ if the corresponding in entries in $$D_1$$ and $$D_2$$ are equal, and _odd_ if not. First, we claim that every cycle $$C$$ in $$G$$ has an _even_ number of odd edges. This is done by induction on the number of diagonals of this cycle, and invokes the previous lemma. 

Now, we partition $$V(G)$$ into $$V_1 \sqcup V_2$$, mimicking the argument that a graph ic bipartite if all cycles are of even length. Start by putting into $$V_1$$ a representative for every connected component of $$G$$ (which we call $$W$$). For every $$u \in V(G)$$, consider the shortest paths from the vertex in $$W$$ to $$u$$. Note that every shortest path from $$u$$ to an element in $$W$$ has either an odd or even number of odd edges. To see this, take any two shortest paths to $$u$$, and their symmetric difference is a union of cycles, which have an even number of odd edges. Splitting these odd edges into two parts, each of the paths must have the same parity for the number of odd edges. This uniquely assigns a parity to every vertex $$u \in V(G)$$ -- put all of the vertices whose shortest paths to a vertex in $$W$$ has an even number of odd edges in $$V_1$$, and vice versa. The odd edges are also precisely the edges that run between $$V_1$$ and $$V_2$$. 

Now multiply the rows/columns in $$D_1$$ corresponding to vertices in $$V_2$$ by $$-1$$. This now must precisely be $$D_2$$, since it is precisely the collection of odd edges that will get their corresponding entry multiplied by $$-1$$, forcing $$D_1$$ to now match $$D_2$$. $$\blacksquare$$. 

These preliminaries are used to constrain what $$D$$ must look like. Personally, I'm not super compelled by the argument, but I'll sketch out how the steps roughly go.

_Proof._ Suppose $$D$$ has no totally unimodular signing, but suppose WLOG that every proper submatrix of $$D$$ does. Observe that $$G(D)$$ is connected (contradicting the minimality assumption) and also can't be a path or cycle (check this by hand). Then, either $$D$$ or $$D^\top$$ can be transformed into a matrix of the form $$\tbmat{u & v & D'}$$ by permuting columns, where $$u, v$$ are column vectors and $$G(D')$$ is connected, by our lemma above. We claim that $$\tbmat{u & D'}$$ and $$\tbmat{v & D'}$$ have totally unimodular, and can be chosen in such a way to be consistent on $$D'$$ (using our very nontrivial lemma above and multiplying by $$-1$$s in places appropriately). As such, assume that we have a matrix $$\tbmat{u & v & D'}$$ obtained from $$X$$ or $$X^\top$$ by permuting columns such that $$G(D')$$ is connected, and that there are totally unimodular signings $$\tbmat{u_1 & D'_1}$$ and $$\tbmat{v_1 & D_1'}$$ of $$\tbmat{u & D'}$$ and $$\tbmat{v & D'}$$, respectively, but $$\tbmat{u_1 & v_1 & D_1'}$$ is _not_ totally unimodular. 

Since $$\tbmat{u_1 & v_1 & D_1'}$$ is not totally unimodular, further pivot at entries in $$D_1'$$ so that the offending minor is in fact a square submatrix. (Lemma 10.1.8 shows that pivoting does not affect this construction, after swapping some columns around.) One can show (Lemma 10.1.9) that this minor must be a submatrix of $$\tbmat{u_1 & v_1}$$ and after multiplying some rows and columns by $$-1$$, that there is a submatrix of $$\tbmat{u_1 & v_1}$$ of the form $$\tbmat{1 & 1 \\ 1 & -1}$$, occupying the first two rows of this matrix (after permuting rows). Here's what $$\tbmat{u_1 & v_1 & D_1'}$$ looks like now: 

$$
\bmat{1 & 1 & \dots \\ 1 & -1 & \dots \\ \vdots & \vdots & \ddots}
$$

Consider the shortest path in $$G(D')$$ linking the vertices corresponding to the first two rows. If this path has length exactly two, then we have some $$2 \times 3$$ submatrix formed from the top two rows of the form $$\tbmat{1 & 1 & a \\ 1 & -1 & b}$$, for $$a, b \neq 0$$. But this contradicts the total unimodularity of either $$\tbmat{u_1 & D_1'}$$ or $$\tbmat{v_1 & D_1'}$$. This path must then be of length $$k$$ strictly larger than $$2$$. Let's look at the submatrix corresponding to this path in $$\tbmat{u_1 & v_1 & D_1'}$$, which (up to some rearrangement) must look like 

$$
\bmat{1 & 1 & 1 & 0 & 0 & \dots & 0 & 0 & 0 \\ 1 & -1 & 0 & 0 & 0 & \dots & 0 & 0 & 1 \\ \vdots & \vdots & 1 & 1 & 0 & \dots & 0 & 0 & 0 \\ \vdots & \vdots & 0 & 1 & 1 & \dots & 0 & 0 & 0 \\ \vdots & \vdots & 0 & 0 & 1 & \dots & 0 & 0 & 0 \\ \vdots & \vdots & \vdots & \vdots & \vdots & \ddots & \vdots & \vdots & \vdots \\ \vdots & \vdots & 0 & 0 & 0 & \dots & 1 & 1 & 0 \\ \vdots & \vdots & 0 & 0 & 0 & \dots & 0 & 1 & 1 \\}
$$

which will have dimensions $$k \times (k+1)$$. (These entries need not all be ones to begin with, but one can force them to be so via a strategic scaling of rows/columns by $$-1$$.) 

Pivot now at entry $$(3, 4)$$ and delete that row and column to get a $$(k-1) \times k$$ matrix: 

$$
\bmat{1 & 1 & 1 & 0 & 0 & \dots & 0 & 0 & 0 \\ 1 & -1 & 0 & 0 & 0 & \dots & 0 & 0 & 1 \\ \vdots & \vdots & -1 & 1 & 0 & \dots & 0 & 0 & 0 \\ \vdots & \vdots & 0 & 1 & 1 & \dots & 0 & 0 & 0 \\ \vdots & \vdots & 0 & 0 & 1 & \dots & 0 & 0 & 0 \\ \vdots & \vdots & \vdots & \vdots & \vdots & \ddots & \vdots & \vdots & \vdots \\ \vdots & \vdots & 0 & 0 & 0 & \dots & 1 & 1 & 0 \\ \vdots & \vdots & 0 & 0 & 0 & \dots & 0 & 1 & 1 \\}
$$

and now do our strategic rescaling to force all nonzero entries to be $$1$$. Iterating, our matrix will eventually be whittled down to a $$3 \times 4$$ matrix: 

$$
\bmat{ 1 & 1 & 1 & 0 \\ 1 & -1 & 0 & 1 \\ c & d & 1 & 1}
$$ 

We now show that this matrix is $$X_F$$ viewed over $$\FF_2$$. Given that $$\tbmat{1 & 1 & 0 \\ -1 & 0 & 1 \\ d & 1 & 1}$$ is totally unimodular, the matrices $$\tbmat{1 & 1 \\ d & 1}$$ and $$\tbmat{-1 & 1 \\ d & 1}$$ must have determinants in $$\set{0, \pm 1}$$, which forces $$d = 0$$. $$\det\left(\tbmat{1 & 1 & 0 \\ 1 & 0 & 1 \\ c & 1 & 1}\right) = c - 2$$, and this matrix must be totally unimodular, so $$c = 1$$. This gives us $$X_F$$ over $$\FF_2$$! $$\blacksquare$$

I have no intuition for this argument, but I think it's cool that it invokes graph theoretic constructions :sweat_smile: The result, on the other hand, is expected, since the Fano plane (which is the projective plane $$\FF_2 \PP^2$$) is only representable over $$\FF_2$$ and not over fields of any other characteristic, so it makes sense that it should be an obstruction to representability over all fields given that one is already binary. 


### seymour's structure 'stravaganza
we're now finally going to get to the characterization of _all_ totally unimodular matrices. This is done via _Seymour's Decomposition Theorem_, which tells us both the atoms making up any regular matroid, and how to construct one.  

> _Theorem._ Every regular matroid is a direct sum, 2-sum, or 3-sum of matroids that are graphic, cographic, or the linear matroid corresponding to the matrix $$\tbmat{I_5 & A_5}$$, where: 

$$ 
A_5 = \bmat{1 & 1 & 0 & 0 & 1 \\ 1 & 1 & 1 & 0 & 0 \\ 0 & 1 & 1 & 1 & 0 \\ 0 & 0 & 1 & 1 & 1 \\ 1 & 0 & 0 & 1 & 1 \\}
$$

I won't explain explicitly how to construct a $$k$$-sum of two matroids, but maybe the slogan: if $$M_1$$ and $$M_2$$ are graphic, overlay the two graphs and delete $$k$$-cliques (this is called the _clique sum_ of the two graphs, sort of like a connected sum of the graphs). The proof requires us to study these operations in detail and is the focus of the entirety of Chapter 13 of Oxley's book, so I'll leave you to look at it if you're interested. 

I'd like to mention this theorem since it's the one that allows us to test unimodularity. Schrijver in _Theory of Linear and Integer Programming_ actually describes an explicit polynomial-time algorithm for doing so (Chapters 19, 20). Translating Seymour's Theorem into matrix terms, we have: 

> _Corollary._ Suppose $$A$$ is totally unimodular. Then at least one of the following is true, up to permuting rows/columns: 
- $$A$$ or $$A^\top$$ is a network matrix (!!). 
- $$A$$ is $$A_5$$ (or _*mumbles*_ transposing and doing some matroid stuff gives you it)[^7]
- $$A$$ has a row/column with at most one nonzero entry, or linearly dependent rows/columns
- $$A$$ can be written as $$\tbmat{W & X \\ Y & Z}$$, with $$\rank(X) + \rank(Y) \le 2$$, and $$W$$ and $$Z$$ have at least $$4$$ rows and columns each. 

Using this, Schrijver invokes two interesting subroutines that we'll black-box: 

- _Subroutine 1._ A (polynomial-time) algorithm time algorithm that tests whether or not a given matrix is a _network matrix_. This is apparently similar to testing a graph for planarity, which can be done in linear time. 
- _Subroutine 2._ A (polynomial-time) algorithm for testing the decomposition of our matrix into blocks with rank conditions as above. This algorithm can be described in terms of _matroid intersection_, which is a polynomial-time algorithm similar to maximum matching.[^8] 

Then, we have the following (polynomial-time) recursive algorithm for total unimodularity: 

1. Check if $$A$$ or $$A^\top$$ is a network matrix using _Subroutine 1_. 
2. Check if $$A$$ is $$A_5$$ (we actually mean related here[^7]).
3. Check if $$A$$ can be decomposed appropriately using _Subroutine 2_. If not, then $$A$$ is not totally unimodular. Otherwise, if $$A = \tbmat{W & X \\ Y & Z}$$, there are a number of cases: 
- if $$\rank(X) = \rank(Y) = 0$$, then $$X = Y = 0$$, so it suffices to check whether $$W$$ and $$Z$$ are totally unimodular. 
- if $$\rank(X) = 1$$ and $$\rank(Y) = 0$$, then factor $$X = uv$$, where $$u$$ and $$v$$ are column/row $$\set{0, \pm 1}$$-vectors, respectively. It now suffices to check if $$\tbmat{W & u}$$ and $$\tbmat{v \\ Z}$$ are totally unimodular (and similarly if $$\rank(X) = 0$$ and $$\rank(Y) = 1$$). 
- if $$\rank(X) = \rank(Y) = 1$$, decompose $$A$$ as $$\tbmat{W_1 & W_2 & 0 & 0 \\ W_3 & W_4 & \mathbf{1} & 0 \\ 0 & \mathbf{1} & Z_1 & Z_2 \\ 0 & 0 & Z_3 & Z_4}$$. Then check whether the matrices $$\tbmat{W_1 & W_2 & 0 & 0 \\ W_3 & W_4 & \mathbf{1} & \mathbf{1} \\ 0 & \mathbf{1} & 0 & \eps_2}, \tbmat{\eps_1 & 0 & \mathbf{1} & 0 \\ \mathbf{1} & \mathbf{1} & Z_1 & Z_2 \\ 0 & 0 & Z_3 & Z_4}$$ are totally unimodular, where $$\eps_1, \eps_2 \in \set{\pm 1}$$ depending on the data of $$G(A^\#)$$ and the decomposition $$A = \tbmat{W & X \\ Y & Z}$$. 
- if $$\rank(X) = 2$$ and $$\rank(Y) = 0$$, pivot at a nonzero entry in $$X$$, which end up reducing to the previous case. 

While recursion is a little scary, Bixby (1982) shows that this algorithm can be implemented in $$O(m+n)^4 m$$ time, where $$A \in \set{0, \pm 1}^{m \times n}$$. This is our desired polynomial-time test that allows us to check whether or not an integer programming problem is actually doable! 


### coda: the field of one element
it's come up a number of times for me that combinatorics is "algebraic geometry over the field of one element." okay, obviously there are no fields with exactly one element, so what do people mean by this? 

I'm not sure if there's a standard reference for this construction, but I'll listen to Katz in _Matroid theory for algebraic geometers_ (2015) here. Katz invokes the theory of "blueprints" and "blue schemes," a generalization of rings and affine schemes: 

> _Definition._ A blueprint $$\mc B$$ is a monoid $$A$$ (with a multiplication operation) with neutral element $$1$$ and absorbing element $$0$$, with an equivalence relation $$\equiv$$ on $$\NN[A]$$, where the relation is closed under multiplication and addition, $$0 \equiv \sum_{x \in \nullset} x$$, and if $$a \equiv b$$ for $$a,b\in A$$, then $$a = b$$. 

Here, $$\FF_1$$ is the blueprint with $$A = \set{0, 1}$$ and empty equivalence relation. $$\FF_1$$ also has a "degree 2 cyclotomic extension" $$\FF_{1^2}$$, defind as the blueprint on $$\set{0, \pm 1}$$ with relation $$1 + (-1) \equiv 0$$.  One can also define the "none-some semifield" $$\BB_1$$, defined also on $$A = \set{0, 1}$$ with equivalence relation $$1 + 1 \equiv 1$$. Blueprints generalize fields and idemopotent semirings, apparently.

Some selected slogans: 
- Matroids are $$\BB_1$$-points in the Grassmannian $$\mathrm{Gr}(r+1, n+1)$$ over $$\BB_1$$. 
- Regular matroids are $$\FF_{1^2}$$-points in the above Grassmannian. 
- Excluded minor results for representability over $$\FF$$ comes from lifting.

For "example," the natural map $$\FF_{1^2} \to \FF_2$$ induces a map $$\mathrm{Gr}_{\FF_{1^2}}(r+11, n+1) \to \mathrm{Gr}_{\FF_2}(r+1, n+1)$$ (is this the right way?). Given a binary matroid $$M$$, Tutte's Theorem then translates to a condition on whether or not the corresponding $$\FF_2$$-point in $$\mathrm{Gr}_{\FF_2}(d+1, n+1)$$ lifts to a $$\FF_{1^2}$$-point. Apparently Pendavingh and van Zwam (2010) are able to interpret the results above in this way (which might be a really cool thing to read, although they use "partial fields" which are similar but not the same, I guess)!

Doing algebraic geometry is already scary enough -- doing it with things that aren't technically even rings is even scarier, although I think this is a popular idea (see Huh et al. pulling out a convex geometry inequality out of cohomology of schemes over "triangular hyperfields," or something...)

---
endnotes

[^1]: the most common use case for this is if $$A$$ is also triangular, but ehhhh whatever

[^2]: I did a little bit of digging into the Tutte homotopy theorem and if people actually think about it -- I couldn't find a ton of stuff about it. [This paper](https://fse.studenttheses.ub.rug.nl/35642/1/bMATH2025KocutarJ.pdf) talks about a geometric interpretation of this theorem via a (realization of the) order complex corresponding to a matroid's lattice of flats (or some poset constructed thereof). This and some other theorem in matroid theory (Tutte's path theorem?) apparently tell you about $$H_1$$ and $$H_0$$ of this space, respecively. The paper speculates about how to interpret, say, $$H_2$$ of this space, but I"m not sure if there's been further progress in this direction. 

[^3]: the simplex method is probably the best-known method for doing this fast (and works reasonably well in practice), although in the worst case this algorithm is potentially exponential in its inputs. these problems were shown to be solvable in polynomial time first by Khachiyan (1979) via the ellipsoid algorithm and then via Karmarkar's algorithm (1984), first in a line of interior-point methods. 

[^4]: or learn quickly

[^5]: okay, one thing to say: this circle of ideas leads to a  description of the _Birkhoff polytope_, the collection of the doubly stochastic $$n \times n$$ matrices in $$\RR^{n \times n}$$. In particular, one can show that this is a convex polytope in $$\RR^{n^2}$$ whose vertices are the permutation matrices, and the fact that the facet (in)equalities that describe doubly stochastic matrices are equivalent to a point in $$\RR^{n^2}$$ being a convex combination of permutation matrices!

[^6]: Oxley details a list of six specific row/column operations on a matrix that do not change the corresopnding linear matroid, namely: interchanging two rows/columns (moving the labels if exchanging columns), scaling a row/column by a nonzero scalar, adding a multiple of a row to another, and adding/deleting rows of zeroes. If two representations are related by a sequence of these moves, Oxley calls these representations _projectively equivalent_ (or _strongly equivalent_). 

[^7]: to be super explicit, Schrijver is asking you to look out for the matrices $$ A_5' = \tbmat{1 & -1 & 0 & 0 & -1 \\ -1 & 1 & -1 & 0 & 0 \\ 0 & -1 & 1 & -1 & 0 \\ 0 & 0 & -1 & 1 & -1 \\ -1 & 0 & 0 & -1 & 1 \\}$$ and $$A_5'' = \tbmat{1 & 1 & 1 & 1 & 1 \\ 1 & 1 & 1 & 0 & 0 \\ 1 & 0 & 1 & 1 & 0 \\ 1 & 0 & 0 & 1 & 1 \\ 1 & 1 & 0 & 0 & 1} $$

[^8]: technically, I think the polynomial-timeness of this algorithm assumes that looking up whether or not a set is independent in a matroid is $$O(1)$$, assuming some sort of "independence oracle." Here, I think it's like linear instead, so there's actually no issue here, but one should be careful. 