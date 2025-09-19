---
layout: post
title:  discrete volume polynomials
date:   2025-09-19
description: in which I am unamused and relearn how generating functions solve everything
tags: polytope gen-func toric-geo alg-geo
categories: algebra combinatorics
related_posts: false
giscus_comments: true
tikzjax: true 
toc:
  beginning: true
---

For all of the time I've spent blogging, I have yet to write about a single combinatorial topic, despite combinatorics being purportedly my main area of interest! (although, to be fair, I expect to do a LOT of reading/writing about combinatorics while at work, so maybe this is a good thing for work/life balance or something like that.) 

for the past few weeks, though, the topic of counting lattice points inside integral polytopes has had a stranglehold on my brain because of my reading course this summer. I figure it's time I break my streak of non-combinatorial posts and talk a little about _Ehrhart theory_! this is the name given to the study of counting lattice points in polytopes (also known as the the _discrete volume_ of a polytope) and relating these counts to the actual volume of the polytope. 

what follows (and everything I know about Ehrhart theory, to be quite honest) comes from Mattias Beck and Sinai Robins' book _Computing the Continuous Discretely_, and it's a really nice read! I really enjoyed how they lead the reader naturally through to important results via examples. Unfortunately, for the sake of brevity, I will be doing the opposite here and give you a major punchline with just enough juice to get there (assuming you know a little bit about polytopes and some generatingfunctionology).[^1] here we go! 

### towards the fund. thm. of Ehrhart theory

one of the main theorems of Ehrhart theory is the following (arguably) fundamental result[^2]:

> Theorem. Given a convex integer polytope $$P \subseteq \RR^d$$, the number of lattice points contained inside $$t P$$ is a polynomial in $$t$$ with rational coefficients. 

We can easily check this for some special cases. If $$P$$ was the unit cube $$[0,1]^d$$, then we can see that for $$t$$ an integer, $$\abs{tP \cap \ZZ^d} = (t+1)^d$$, which is a polynomial of degree $$d$$. If $$P = \conv(0, e_1, \dots, e_d) \subseteq \RR^d$$ is the standard $$d$$-simplex, then $$\abs{tP \cap \ZZ^d} = \binom{t + d} d$$, which is also a polynomial of degree $$d$$ with rational coefficients. 

Let's introduce some notation now to describe these counts. For a convex integer $$d$$-polytope $$P$$ and a positive integer $$t \in \ZZ^+$$, define the _Ehrhart polynomial_ of $$P$$ to be $$L_P(t) = \abs{tP \cap \ZZ^d}$$ (and let $$L_P(0) = 1$$ by convention). We don't yet know that $$L_P(t)$$ is a polynomial, but the theorem we want to show says that $$L_P(t)$$ will turn out to be! Similarly, we define the _Ehrhart series_ $$\ehr_P(x) = \sum_{t \geq 0} L_P(t) x^t$$, which will be useful for us as an intermediate construction. 

To start, first, note that we can triangulate any polytope $$P$$ into simplices $$\set{\Delta_i}_{i=1}^n$$, with the intersections of the simplices $$\Delta_i$$ being lower-dimensional simplices, and using no new vertices. Then, if we were able to show that if we were able to show the statement holds for simplices $$\Delta$$, then this triangulation would suffice for polytopes in general. In this case, we can do a version of inclusion-exclusion on the polynomials counting the lattice points of the simplices to compute the polynomial counting the lattice points for $$P$$, taking $$L_{\nullset}(t) = 0$$: 

$$
L_P(t) = \sum_{i=1}^n L_{\Delta_i}(t) - \sum_{i < j} L_{\Delta_i \cap \Delta_j}(t) + \dots + (-1)^n L_{\Delta_1 \cap \dots \cap \Delta_n}(t)
$$

As such, it suffices to show the theorem for simplices $$\Delta$$. To do this for a simplex $$\Delta \subseteq \RR^d$$, we embed $$\Delta$$ into $$\RR^{d+1}$$ in such a way where if $$\Delta = \conv(v_i \mid 1 \leq i \leq d+1)$$, and letting $$w_i = \tbmat{v_i \\ 1}$$, we embed $$\Delta \subseteq \RR^{d+1}$$ as $$\conv(w_i \mid 1 \leq i \leq d+1)$$. This puts $$\Delta$$ on the hyperplane $$x_{d+1} = 1$$ in $$\RR^{d+1}$$. Now, consider the cone $$C = \cone(\Delta)$$ in $$\RR^{d+1}$$, and note that by construction, the intersections $$C \cap \set{x_{d+1} = t}$$ are dilations $$t\Delta$$. As such, the lattice points in $$C \cap \ZZ^{d+1}$$ can be stratified where the intersection of $$C \cap \ZZ^{d+1}$$ with the hyperplane $$x_{d+1} = t$$ gives us the lattice points of $$t\Delta$$, which are exactly counted by $$L_\Delta(t)$$. This motivates us to want to keep track of the coordinates of the lattice points of $$C \cap \ZZ^{d+1}$$ and to be able to split them by their $$x_{d+1}$$-coordinate. Sounds like a job for a generating function! 

#### generatingfunctionology for cones
As above, let $$\Delta = \conv(w_i \mid 1 \leq i \leq d+1)$$ be a $$d$$-simplex embedded in $$\RR^{d+1}$$ on the hyperplane $$\set{x_{d+1} = 1}$$, and let $$C$$ be the (_simplicial_) cone over $$\Delta$$. We want to be able to enumerate the lattice points of $$C$$ in a way that keeps track of the actual values of the coordinates of each of the lattice points. Generating functions are a nice way of doing so, because stratifying a polynomial/power series by degree naturally corresponds to the splitting we want where we'd like to separate the lattice points of $$C \cap \ZZ^{d+1}$$ by their $$x_{d+1}$$-coordinate. 

To do this, we introduce slightly more general notation (as from Beck-Robins). For a subset $$S \subseteq \RR^d$$, define 

$$
\sigma_S(z_1, \dots, z_d) = \sum_{m \in S \cap \ZZ^d} \prod_{i=1}^d z_i^{m_i}
$$

where the sum is over lattice points $$m = (m_1, \dots, m_d)$$ in $$S$$. Note that each $$w_i$$ is a (minimal) ray generator of the rays that bound the simplicial cone $$C$$. Then, the translates of the half-open parallelopipeds $$\Pi = \set{\sum_{i=1}^{d+1} \lambda_i w_i, 0 \leq \lambda_i < 1}$$ by integer multiples of the generators $$w_i$$ cover all of $$C$$. We can therefore account for all of the lattice points $$C$$ (or all of the terms that appear in $$\sigma_S$$) by enumerating all of the lattice points inside $$\Pi$$, and then adding a copy for every translate. Since we can translate in each direction $$w_i$$ by a non-negative integer multiple, letting $$z^{w_i}$$ be shorthand for the product $$\prod_{j=1}^{d+1} z_j^{(w_i)_j}$$, our generating function $$\sigma_C(z_1, \dots, z_d)$$ becomes 

$$
\begin{align*}
\sigma_C(z_1, \dots, z_d, z_{d+1}) &= \sigma_\Pi(z_1, \dots, z_d, z_{d+1}) \prod_{i=1}^{d+1} (1 + z^{w_i} + (z^{w_i})^2 + \dots) \\
&= \frac{\sigma_\Pi(z_1, \dots, z_d, z_{d+1})}{\prod_{i=1}^{d+1} (1-z^{w_i})}
\end{align*}
$$

By our observations above, if we set $$z_1 = z_2 = \dots = z_d = 1$$, then our power series separates by powers of $$z_{d+1}$$, whose coefficients correspond to $$L_\Delta(t)$$. For each $$w_i$$, the $$(d+1)$$th coordinate is $$1$$ by construction, so $$z^{w_i}$$ becomes $$z_{d+1}^1$$ when setting the rest of the coordinates to $$1$$: 

$$
\begin{align*}
\sigma_C(1, \dots, 1, z_{d+1}) &= \sum_{t \geq 0} L_\Delta(t) z_{d+1}^t = \ehr_\Delta(z_{d+1}) \\
&= \frac{\sigma_\Pi(1, \dots, 1, z_{d+1})}{\prod_{i=1}^{d+1} (1-z_{d+1}^1)} = \frac{\sigma_\Pi(1, \dots, 1, z_{d+1})}{(1-z_{d+1})^{d+1}}
\end{align*}
$$

Note that $$\sigma_\Pi(1, \dots, 1, z_{d+1})$$ is a polynomial in $$z_{d+1}$$ since it counts lattice points weakly above the hyperplane $$z_{d+1} = 0$$, and then the binomial expansion

$$
(1-z_{d+1})^{-(d+1)} = \sum_{t \geq 0} \binom{t+d}{t} z_{d+1}^t
$$ 

multiplied by our polynomial $$\sigma_\Pi(1, \dots, 1, z_{d+1})$$ in $$z_{d+1}$$. This isn't quite enough to guarantee that when we expand our product $$\sigma_\Pi(1, \dots, 1, z_{d+1})\sum_{t \geq 0} \binom{t+d}{t} z_{d+1}^t $$ that we get a polynomial in $$t$$ as a coefficient of every $$z_{d+1}^t$$. Suppose for a moment that our expression is reduced in the sense that $$(1-z_{d+1}) \nmid \sigma_\Pi(1, \dots, 1, z_{d+1})$$. Then, if we had $$\deg \sigma_\Pi(1, \dots, 1, z_{d+1}) > d$$, we would be forced to treat some of the leading terms in our expansion differently as not all terms in $$\sigma_\Pi$$ could contribute to the terms of degree $$\leq d$$. Otherwise, everything is essentially fine and our desired polynomial is just an integer linear combination of polynomials in $$t$$ that look like binomial coefficients, hence giving a polynomial in $$t$$ with rational coefficients as the coefficients of $$z_{d+1}^t$$. 

Good news -- first, note that $$\sigma_\Pi(1, \dots, 1, 1) > 0$$, since $$\sigma_\Pi(1, \dots, 1, 1)$$ counts the number of integer lattice points in $$\Pi$$, which is a non-empty set since $$0 \in \Pi$$. Moreover, $$\deg \sigma_\Pi(1, \dots, 1, z_{d+1}) \leq d$$. To see this, note that in the $$(d+1)$$th coordinate of any point of $$\Pi$$, we see that $$\sum_{i=1}^{d+1} \lambda_i (w_i)_{d+1} = \sum_{i=1}^{d+1} \lambda_i < d+1$$ since $$0 \leq \lambda_i < 1$$, and since the degree $$\sigma_\Pi(1, \dots, 1, z_{d+1})$$ is an integer bounded by $$d+1$$, the degree of this polynomial is no more than $$d$$. Therefore, no further complicated arguments are needed and a naive expansion of this series will supply us with a polynomial in $$t$$ for $$L_\Delta(t)$$ after comparing coefficients. This completes the argument for integral simplices, and hence in all cases for all convex integral polytopes $$P$$ after the above inclusion-exclusion argument. In particular, we see that the Ehrhart polynomial of a convex integral $$d$$-polytope is indeed actually a polynomial of degree at most $$d$$. 

This is sort of a miraculous argument that can be generalized to polytopes that have rational coordinates as well with not too much more effort! When I first saw this result, it was pretty clear that the growth rate of $$L_P(t)$$ should be asymptotically bounded by $$t^d$$[^3], but it wasn't clear to me that this had to exactly be a polynomial (what's stopping it from being, say, something proportional to like $$t^{d-1} \log t$$ or something?). However, the generating functions argument here makes it clear that this counting function has to actually be a polynomial by power series magic. 

---

Doing this sort of lattice point counting for cones with generating functions is especially nice, especially when the boundaries of these cones have no lattice points. Let's state a couple of facts in this vein (one of which is a secret tool that we'll need to use later): 

> Corollary. Let $$C$$ be an integral $$d$$-cone with minimal ray generators $$\set{w_i}_{i=1}^d$$ and $$v \in \RR^d$$ be chosen such that $$v + C$$ has no lattice points on its boundary. Then letting $$\Pi$$ be the _open_ parallelopiped $$\Pi = \set{\sum_{i=1}^d \lambda_i w_i, \lambda_i \in (0,1)}$$, 
$$
\sigma_{v + C}(z_1, \dots, z_d) = \frac{\sigma_{v + \Pi}(z_1, \dots, z_d)}{\prod_{i=1}^d (1-z^{w_i})}
$$
using the same shorthand notation as above. 

The argument above that covers $$v + C$$ by translates of the parallelopiped $$v + \Pi$$ by integer multiples of $$w_i$$ essentially goes through without a hitch here. Note that the condition that $$v+C$$ has no lattice points on its boundary ensures that nothing fishy is happening with potentially missing/overcounting lattice points on the boundary of $$v + C$$. 

Here's another proposition regarding cones with no lattice points on their boundaries: 

> Proposition. Let $$C$$ be a simpicial integral $$d$$-cone with minimal ray generators $$\set{w_i}_{i=1}^d$$ and $$v \in \RR^d$$ be chosen such that $$v + C$$ has no lattice points on its boundary. Then $$\sigma_{v + C} \left(\frac 1{z_1}, \dots, \frac 1{z_d}\right) = (-1)^d \sigma_{-v + C}(z_1, \dots, z_d)$$. 

_Proof._ Let $$\Pi = \set{\sum_{i=1}^d \lambda_i w_i, \lambda_i \in (0, 1)}$$, again using the open parallelopiped $$\Pi$$ since we can get away with it. Then, recalling the argument above, that 

$$
\sigma_{-v + C}(z_1, \dots, z_d) = \frac{\sigma_{-v + \Pi}(z_1, \dots, z_d)}{\prod_{i=1}^d (1 - z^{w_i})}.
$$

where $$-v + C$$ does not have any lattice points on its boundary. To see this, observe that $$v + \Pi = -(- v + \Pi) + \sum_{i=1}^d w_i$$, so a lattice point on the boundary of $$-v + \Pi$$ would yield a lattice point on the boundary of $$v + \Pi$$, contradiction. This observation also allows us to note that 

$$
\sigma_{v + \Pi}(z_1, \dots, z_d) = \sigma_{-(-v + \Pi)}(z_1, \dots, z_d) \prod_{i=1}^d z^{w_i} = \sigma_{-v + \Pi}\left(\frac 1{z_1}, \dots, \frac 1{z_d}\right) \prod_{i=1}^d z^{w_i}. 
$$

Changing coordinates $$z_i \mapsto \frac 1{z_i}$$, we therefore have $$\sigma_{v + \Pi}\left(\frac 1{z_1}, \dots, \frac 1{z_d}\right) = \sigma_{-v + \Pi}(z_1, \dots, z_d) \prod_{i=1}^d z^{-w_i}$$. Then, we can rewrite $$\sigma_{v + C}(z_1, \dots, z_d)$$ with the above: 

$$
\begin{align*}
\sigma_{v + C}\left(\frac 1{z_1}, \dots, \frac 1{z_d}\right)
&= \frac{\sigma_{v+\Pi}\left(\frac 1{z_1}, \dots, \frac 1{z_d}\right)}{\prod_{i=1}^d \left(1- \frac 1{z^{w_i}} \right)} 
= \frac{\sigma_{-v+\Pi}(z_1, \dots, z_d) \prod_{i=1}^d z^{-w_i}}{\prod_{i=1}^d \left(1- \frac 1{z^{w_i}} \right)} \\
&= \frac{\sigma_{-v+\Pi}(z_1, \dots, z_d) }{\prod_{i=1}^d \left(z^{w_i} - 1 \right)} = (-1)^d \frac{\sigma_{-v+\Pi}(z_1, \dots, z_d) }{\prod_{i=1}^d \left(1 - z^{w_i} \right)} \\
&= (-1)^d \sigma_{-v+C}(z_1, \dots, z_d) 
\end{align*} 
$$

giving us the desired identity.


 
### reciprocity 
there's actually more to say about the Ehrhart polynomial! the other big thing to know about the Ehrhart polynomial of a polytope $$P$$ is the following fact about its negative values in relation to the Ehrhart polynomial of its interior, $$P^\circ$$: 

> Theorem (Ehrhart-Macdonald Reciprocity). If $$P$$ is a convex integral $$d$$-polytope, then $$L_P(-t) = (-1)^d L_{P^\circ}(t)$$. 

We should be a little careful, since $$P$$ is closed by assumption but $$P^\circ$$ is open -- here, $$L_{P^\circ}(t) = |tP^\circ \cap \ZZ^d|$$ for positive integers $$t$$. 

When I first read about this, this felt a bit like there might be some sort of secret inclusion-exclusion thing at work here, which is sometimes the case when you see signs in your counting formulas. Actually, this is still mostly just a straight generating functions argument, but a little more tech and subtlety is needed. 

Again, we're going to hope to pass through the Ehrhart series of a polytope $$P \subseteq \RR^d$$, which is a special case of $$\sigma_{\cone(P)}$$ where $$P$$ is viewed as embedded in the hyperplane $$x_{d+1} = 1$$ in $$\RR^{d+1}$$. As such, we will first prove the following related theorem: 

> Theorem (Stanley Reciprocity). Let $$C$$ be a rational $$d$$-cone with the origin as its apex. Then 
$$
\sigma_C\left(\frac 1{z_1}, \dots, \frac 1{z_d}\right) = (-1)^d \sigma_{C^\circ}(z_1, \dots, z_d).
$$

To prove this, we're going to do a little trickery. First, we will triangulate $$C$$ into a bunch of simplicial cones $$C_1, \dots, C_m$$. Then, we'll need to construct a vector $$v \in \RR^d$$ so that (1) $$C^\circ \cap \ZZ^d = (v + C) \cap \ZZ^d$$, (2) none of the cones $$v + C_j$$ or $$-v + C_j$$ have any lattice points on their boundary. The existence of this vector is an exercise in the book, which I should probably comment on briefly. I think you can construct this via some sort of minimality argument by looking at the closest lattice points to the origin, and then picking a vector $$v$$ in the cone $$C$$ that has shorter magnitude than the closest lattice point. This should guarantee that when you shift the cone $$C$$ by $$v$$, you only lose the lattice points on the boundary by some minimality argument. $$v$$ should also need to be chosen depending on the triangulation $$C_j$$ so that we don't accidentally pick up a lattice point on the boundary of $$v + C_j$$ or $$-v + C_j$$. This feels like a bit of a real-analytical style argument which I'm not super keen on thinking through all the way, but it should essentially follow from minimality and the finiteness of any triangulation. 

Now, given this property for $$v$$, we'll claim that $$(-v + C) \cap \ZZ^d$$ = $$C \cap \ZZ^d$$. Since $$-v + C$$ doesn't pick up any additional points on its boundary, all of its lattice points lie in its interior. But then because of our first property, $$(-v + C^\circ) \cap \ZZ^d = C \cap \ZZ^d$$, so we don't get any new lattice points not coming from $$C$$. With this claim proven, we can leverage the proposition we proved above about counting lattice points in a cone with no lattice points on its boundary for our perturbed cone $$-v + C$$. Here we go: 

$$
\begin{align*}
\sigma_C\left( \frac 1{z_1}, \dots, \frac 1{z_d} \right) 
&= \sigma_{-v+C}\left( \frac 1{z_1}, \dots, \frac 1{z_d} \right) \\
&= \sum_{j=1}^m \sigma_{-v+C_j}\left( \frac 1{z_1}, \dots, \frac 1{z_d} \right) \\
&= \sum_{j=1}^m (-1)^d \sigma_{v+C_j}\left( z_1 \dots, z_d \right) \\
&= (-1)^d \sigma_{v + C} (z_1, \dots, z_d) = (-1)^d \sigma_{C^\circ} (z_1, \dots, z_d)
\end{align*} 
$$

First, we leverage the fact that $$C \cap \ZZ^d = (-v + C) \cap \ZZ^d$$, as shown above. We can now split $$-v + C$$ into simplicial cones $$-v + C_j$$, and then apply the above proposition to each one, where the above proposition only applies since every single $$C_j$$ is simplicial. Note that we don't have any overcounting here since the boundary of each $$-v + C_j$$ is free of lattice points. After applying the proposition, we can now recombine the counts for the lattice points on these simplicial cones freely, since again the boundaries don't have any overlapping lattice points and the collections of lattice points per cone are disjoint. We finally use the construction of $$v$$ such that $$(v + C) \cap \ZZ^d = C^\circ \cap \ZZ^d$$ to finish. Neat! 

As we see here, I think we needed a little analysis to be able to perform this construction and to make sure all of the steps that we'd like to work actually work out. Once we have this, though, we're basically almost done. 

The last thing we need before we prove the reciprocity theorem is a final generatingfunctionology fact: 

> Lemma. Given a polynomial $$Q(t)$$, $$R_Q^+(z) = \sum_{t \geq 0} Q(t) z^t$$ and $$R_Q^-(z) = \sum_{t < 0} Q(t) z^t$$ are rational functions satisfying $$R^+_Q(z) + R^-_Q(z) = 0$$. 

{% details open me if you want to see this spelled out %}
First, observe that $$R_Q^+(z)$$ is definitely a rational function, via taking iterated derivatives, e.g.: 

$$ 
\sum_{t \geq 0} z^t = \frac 1 {1-z} 
$$

$$
\sum_{t \geq 0} t z^t = z \sum_{t \geq 0} t z^{t-1} = z \dv{} z \sum_{t \geq 0} z^t = z \dv{} z \frac 1{1-z} = \frac z{(1-z)^2}
$$

and so on and so forth for higher powers of $$t$$. 
Similarly, $$R_Q^-(z)$$ is also a rational function, e.g.: 

$$
\sum_{t < 0} z^t = \frac{z^{-1}}{1 - z^{-1}} = \frac 1{z-1}
$$


$$
\sum_{t < 0} t z^t = \sum_{t \geq 1} (-t) z^{-t} = z \sum_{t \geq 1} (-t) z^{-t-1} = z \dv{} z \sum_{t \geq 1} z^{-t} = z \dv{} z \frac {z^{-1}}{1-z^{-1}} = \frac {-z}{(z-1)^2}
$$

and again a similar computation will yield that these are rational functions for higher powers of $$t$$. Note that in these case, we have that $$R^+_Q(z) + R^-_Q(z) = 0$$. In general, it's probably best to show this statement inductively for every monomial $$t^k$$, and then linearity gives us the rest. We have base cases -- for our inductive step, observe from the analyses above that $$R_{t^{k+1}}^+(z) = z \dv{}z R_{t^k}^+(z)$$, and similarly $$R_{t^{k+1}}^-(z) = z \dv{}z R_{t^k}^-(z)$$. Then, $$R_{t^{k+1}}^+(z) + R_{t^{k+1}}^-(z) = z \dv{}z (R_{t^k}^+(z) + R^-_{t^k}(z)) = z \dv{}z (0) = 0$$ by the inductive hypothesis. By linearity, this shows the result for all polynomials $$Q(t)$$, as desired. 

{% enddetails %}

With this, let's prove Ehrhart-Macdonald reciprocity. Suppose $$P$$ is a $$d$$-polytope. Recall that $$\ehr_P(z) = \sum_{t \geq 0} L_P(t) z^t$$, and let $$\ehr_{P^\circ}(z) = \sum_{t \geq 0} L_{P^\circ}(t) z^t$$ similarly. Then with the above generatingfunctionology fact, observe that $$\ehr_P(\frac 1z)$$ is equal to $$\sum_{t \leq 0} L_P(-t) z^t = - \sum_{t \geq 1} L_P(-t) z^t$$, since $$L_P(t)$$ is a polynomial. (This isn't a direct application of the above result, but it nearly is.) In comparison, note that $$\ehr_P(\frac 1z) = \sigma_{\cone(P)}(1, \dots, 1, \frac 1z) = (-1)^{d + 1} \sigma_{\cone(P)^\circ}(1, \dots, 1, z)$$ by Stanley reciprocity. This in turn is equal to $$(-1)^{d + 1} \ehr_{P^\circ}(z)$$. Comparing coefficients by degree, we therefore have that for all positive integers $$t$$, that $$(-1)^{d} L_{P^\circ}(t) = L_P(-t)$$, as desired! 

This is a super neat result, which as stated above required a little bit of finesse -- we needed some triangulating along with a bit of analysis to sort of nudge the cone over our polytope $$P$$ in a way that allowed us to count lattice points properly. The rest is mostly just algebraic manipulation and a generating function fact that allows us to look at our power series $$\ehr_P(z)$$ in a different way. Here, the dimension matters pretty deeply for our count, as it manifests in relation to the number of ray generators for a simplicial cone over a triangulation of our $$d$$-polytope $$P$$. As stated above, usually I think signs indicate some sort of inversion happening behind the scenes -- here, this is not the case. 


### sike this is secretly a post about sheaf cohomology 

you might be wondering why this post is tagged with the algebraic geometry tag, and (if you know me) why I got into this business in the first place, since I don't think I typically enjoyed thinking about polytopes before this summer. 

It turns out that one can prove all of the statements that we've shown here about (1) the existence of the Ehrhart polynomial and (2) Ehrhart reciprocity using toric varieties using sheaf cohomology. In brief, here's how the connection goes (from Cox, Little, Schenck's _Toric Varieties_): 

- Every $$d$$-polytope $$P$$ gives a $$d$$-dimensional projective toric variety $$X_P$$ over $$\CC$$ (_not_ uniquely -- if two polytopes have the same inner normal fans, they give the same variety... I think) 
- Such a toric variety $$X_P$$ arising from a polytope $$P$$ has a canonical (Cartier) divisor $$D_P$$, corresponding to an invertible sheaf/line bundle $$\cOO_{X_P}(D_P)$$[^4]
- The global sections of such a sheaf, $$\Gamma(X_P, \cOO_{X_P}(D_P))$$ is exactly $$\bigoplus_{m \in \ZZ^d} \CC \cdot \chi^m$$ where $$\chi^m$$ is a character of the Zariski-dense torus in $$X_P$$ corresponding to the lattice point $$m$$, i.e. the dimension of the space of global sections is exactly counted by the number of lattice points inside $$P$$. 
- We can show various vanishing theorems about the sheaf cohomology of the line bundles associated to these Cartier divisors:
    - (Demazure vanishing) $$H^p(X_P, \cOO_{X_P}(D_P)) = 0$$ for $$p \geq 0$$. 
    - (Batyrev-Borisov vanishing) $$H^p(X_P, \cOO_{X_P}(-D_P)) = 0$$ for all $$p \neq d$$, and when $$p = d$$, $$H^d(X_P, \cOO_{X_P}(-D_P)) = \bigoplus_{m \in P^\circ \cap \ZZ^d} \CC \cdot \chi^{-m}$$. 
- Finally, we consider the Hilbert polynomial $$\chi(\cOO_{X_P}(tD_P))$$ (a polynomial in $$t$$ computed by the dimensions of the sheaf cohomology groups $$H^p(X_P, \cOO_{X_P}(tD_P)))$$. This polynomial will end up being the Ehrhart polynomial of $$P$$!  

In particular, one may conclude from the above that $$L_P(t) = \chi(\cOO_{X_P}(tD_P))$$ for positive integers $$t$$. Demazure vanishing says that essentially all of the sheaf cohomology groups vanish except for the zeroth one, which by definition is just the dimension of the global sections of this sheaf. However, we know that this dimension is counted by the number of lattice points in $$tP$$, so we get the existence of the Ehrhart polynomial for free. Moreover, since Batyrev-Borisov vanishing says the cohomology groups $$H^p(X_P, \cOO_{X_P}(-D_P))$$ vanish except in degree $$d$$, we get the sign $$(-1)^d$$ in the statement of Ehrhart reciprocity and dimension the number of lattice points $$(tP)^\circ$$, so $$L_P(-t) = \chi(\cOO_{X_P}(-tD_P)) = (-1)^d L_{P^\circ}(t)$$. This recovers Ehrhart reciprocity as well!

I fear I won't write about this in any more depth here, since this is the subject of my writing milestone and it's pretty involved... But I think it's super worthwhile to learn ow one would get these combinatorial results without toric geometry since this is a result that stands on its own without the algebrao-geometric backing[^5], and these are pretty cool results in their own right without any need for the geometry! 

---
endnotes

[^1]: throughout, I'm also going to restrict to talking exclusively about polytopes with integral vertices just to make my life easier, but a slightly more complicated theory exists for polytopes with rational vertices. I just don't want to deal with quasipolynomials thanks :sweat_smile: 

[^2]: I always want to be a little careful calling something a "fundamental theorem of x" because sometimes those results are neither fundamental nor a theorem about x... looking at you, fundamental theorems of algebra and arithmetic

[^3]: in particular, it will turn out that the leading coefficient/the coefficient of $$t^d$$ in $$L_P(t)$$ will be the $$d$$-dimensional volume of the $$d$$-polytope $$P$$! This sort of relates the discrete volume to the continuous volume of $$P$$. In such a way, one can compute the volume by computing the Ehrhart polynomial via interpolation and then reading off the leading coefficient, which is neat!

[^4]: all toric varieties arising in this way are at least normal and separated over $$\CC$$, so $$\pic(X_P)$$ and $$\cdiv(X_P)$$ are the same and we don't have to worry about the distinction really 

[^5]: a certain advisor of mine has told me (maybe a bit hyperbolically) that "all theorems in toric geometry are exercises in polyhedral combinatorics" and honestly he's really not wrong... in my opinion, toric geometry is a good framework for reorganizing/reorienting one's view of polyhedral stuff, but not necessarily any easier to work with 