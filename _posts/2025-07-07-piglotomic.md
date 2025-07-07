---
layout: post
title:  in honor of the yellow pig
date:   2025-07-07
description: two number theory stories (and despite losses, the pigs go marching on)
tags: comm-alg alg-nt galois
categories: algebra 
related_posts: false
---

yellow pigs often come to mind now that we are in summer proper and 
the days are very long. 
You might not be familiar with the mythical yellow pig, the darling of mathematicians 
David C. Kelly and Michael Spivak[^1]. It is a magical creature with seventeen eyelashes, 
seventeen teeth, and seventeen toes, and the official emblem of Hampshire College's Summer Studies,
directed by Kelly for 52 years. To me, it is a reminder of the joy and wonder of being
able to share mathematics in community, which is something I value dearly. 

These days, though, I think especially of Kelly. In my time as a student at Hampshire
and in my short stint as staff, I look fondly back on the ways he inspired us
to teach with humor and kindness, and to bring a certain whimsy and fun 
to mathematics. My time at Hampshire was my first formative experience in punctuating the 
mundanity of standard-issue "school math" while also avoiding falling into the thought patterns of competition math. 
In the math department here at UW, I'm reminded of him and his spirit as we attempt to 
navigate the unpredictable teaching landscape here, especially as large changes are underway. 

Unlike others, I don't have very many Kelly stories, but I'd like to share a few tidbits
of math here that remind me of him and his work in particular.

### the first story: the rival 
The number 17 holds a special place in the hearts of those who pass through the Summer Studies,
due to its close association to the program's mascot. As an alum of the program, one of the 
most-anticipated events of the six weeks is Kelly's annual talk on the mathematical and 
social history of the number 17, given every Yellow Pigs' Day on July 17th.

And there is really nobody more qualified to do this task than Kelly himself. 
In years past, as Hampshire's program has elevated the status of the number 17, there have 
been other numbers that have been proposed as contenders to rival 17 in its significance. 
This is of course a fruitless task. One notable occasion involving the upstart number 23
had proponents of the number 23 face off against Kelly and mathematician Don Goldberg 
in a competition to name as many properties of their favored number as possible.[^2] While 
Don and Kelly were each exhausting properties of the number 17 as they went along, they 
managed to outnumber their opponents' properties of 23 two to one. I'm sure the number 23
is a very special number, but whoever Kelly and Don were facing off against were simply 
not familiar with their game, so to speak.[^3]

When I remember this story in particular, I'm reminded of a fairly cursed fact that, 
while not counting positively for Kelly's team as a property of 17, I do think should 
count negatively against 23. 23 happens to be the smallest number for which a nice 
number-theoretic property of its corresponding cyclotomic extension fails, which is 
quite sad if you'd like to prove Fermat's Last Theorem the easy way, but maybe also 
quite interesting if you like algebra. 

For any integer $$n \in \ZZ$$, let $$\zeta_n$$ be the primitive $$n$$th root of unity
$$\zeta_n = e^{\frac{2\pi i}n}$$. Consider the cyclotomic extension $$L= \QQ[\zeta_n]$$ 
over $$\QQ$$, which has ring of integers $$\cOO_L = \ZZ[\zeta_n]$$. It turns out
the smallest $$n$$ for which this ring $$\cOO_L$$ is not a UFD is $$n = 23$$. Let's see why! 
Recall that in the land of Dedekind domains like $$\cOO_L$$, this is equivalent to saying 
that it is not a PID, so we just need to exhibit an ideal that is not principal. 

Before this, though, a crash course in silly algebraic number theory words, and how
they play nicely with the best (Galois) field extensions: 
{% details if $$efg$$ doesn't ring a bell, you might want to read this %}

Throughout, we work with an extension of number fields $$L / K$$ with 
corresponding rings of integers $$R = \cOO_K$$ and $$S = \cOO_L$$. 
Given a prime ideal $$\mfk p \subseteq R$$, the extension $$\mfk p^e = \mfk p S$$
factors uniquely as a product of prime ideals $$\mfk p^e = \prod_{i=1}^g \mfk q_i^{e_i}$$. The exponents $$e_i$$ are the _ramification indices_ of $$\mfk q_i$$ over 
$$\mfk p$$, denoted sometimes as $$e(\mfk q_i / \mfk p)$$, and a prime ideal 
$$\mfk q_i$$ is said to _ramify_ if $$e(\mfk q_i / \mfk p) \geq 2$$. $$\mfk q_i$$
is said to _lie over_ $$\mfk p$$, which usually means something different in other commutative algebra contexts (in particular, the seemingly weaker condition that $$\mfk q_i \cap R = \mfk p$$), but 
these turn out to be equivalent since all primes are maximal in Dedekind domains. 

In the case where $$K = \QQ$$ and $$R = \ZZ$$, we abuse language a little and 
can ask when primes $$p \in \ZZ$$ (generating their own prime ideals) _ramify_ in
$$L$$, i.e. when the prime ideal $$\idl p$$ is extended to $$\cOO_L$$, if 
the ideal splits into prime ideals with multiplicity. It turns out that the prime
$$p$$ ramifies iff it divides the discrmininant of $$L$$ over $$\QQ$$. 

We can also define a complement to the ramification index using the
language of fields. For primes $$\mfk p \subseteq R$$ and $$\mfk q_i \subseteq S$$
lying over $$\mfk p$$ as above, we can define an injection $$R / \mfk p \to S / \mfk q_i$$, since the map $$R \to S / \mfk q_i$$ has kernel $$\mfk q_i \cap R = \mfk p$$. 
Then $$S/ \mfk q_i$$ is a finite extension of the finite field $$R / \mfk p$$, and
we say the degree of this field extension is called the _inertial degree_ 
of $$\mfk q_i$$ over $$\mfk p$$, denoted $$f(\mfk q_i / \mfk p)$$. 

As it turns out, both the inertia degree and the ramification index are multiplicative in towers of field extensions and towers of primes lying over others. To be super explicit, if we have a tower of field extensions $$K \subseteq L \subseteq M$$, with rings of integers $$\cOO_K \subseteq \cOO_L \subseteq \cOO_M$$ and  prime ideals $$\mfk p \subseteq \mfk q \subseteq \mfk r$$ of each ring, respectively, with each lying over the previous, we have that 

$$
e(\mfk r / \mfk p) = e(\mfk q / \mfk p) e(\mfk r / \mfk q) 
\quad 
f(\mfk r / \mfk p) = f(\mfk q / \mfk p) f(\mfk r / \mfk q) 
$$

Before we can state some of our fanciest results, we have to quickly talk about the ideal norm. Suppose we have an extension of number fields $$L / K$$ with rings of integers $$R = \cOO_K$$ and $$S = \cOO_L$$. Then for any prime ideal $$\mfk q \subseteq S$$, the _(relative) ideal norm_ is $$N(\mfk q) = \mfk p^{f(\mfk q / \mfk p)}$$, where $$f$$ is the inertia degree and $$\mfk p = \mfk q \cap R$$ is the prime in $$R$$ that $$\mfk q$$ lies over. We can also define this in an absolute manner, where we take the base field $$K = \QQ$$, in which case we can conflate the ideal of $$\ZZ$$ and its principal generator. In this case we say instead that the _(absolute) norm_ $$N(\mfk q) = [\cOO_L : \mfk q]$$, the size of the field $$\cOO_L / \mfk q$$. The absolute and relative norms agree in the sense that the number in $$\ZZ$$ that the absolute norm produces is the generator for the principal ideal given by the ideal norm, i.e. $$[\cOO_L : \mfk q] = p^{f(\mfk q / p)}$$ if $$\mfk q \cap \ZZ = \idl p$$. 

We can use this to show the following cute lemma: 

> Lemma. Let $$L / K$$ be an extension of number fields of degree $$n$$, and $$\mfk p \subseteq \cOO_K$$. If $$\mfk p \cOO_L = \prod_{i=1}^g \mfk q_i^{e_i}$$ with $$f_i$$ the inertial degree of each $$\mfk q_i$$ over $$\mfk p$$, then $$n = \sum_{i=1}^g e_i f_i$$. 

_Proof._ We do this in the case $$K = \QQ$$ and $$\mfk p = \idl p$$. (In general, you might have to do a bit more work, by localizing at $$\mfk p$$ to compute dimensions or otherwise.) From the unique factorization of ideals into prime ideals, we see that $$N(\mfk p^e) = \prod_{i=1}^g N(\mfk q_i)^{\mfk e_i} = \prod_{i=1}^g (p^{f_i})^{e_i}$$, assuming you believe the norm is multiplicative and that the norm agrees with the relative norm. But then $$N(\mfk p^e) = p^n$$ since it is isomorphic to $$\ZZ^n / p \ZZ^n$$, and comparing exponents gives the equality. (In general, you'll have to try to convince yourself that $$N(\mfk p^e) = \mfk p^n$$ even in the relative case, which is true but requires some work.)

If $$L / K$$ is Galois, then we get the even nicer result that for any $$\sigma \in \Gal(L / K)$$, that for a prime $$\mfk q \subseteq L$$ lying over $$\mfk p$$, we have $$\sigma(\mfk q)$$ lies over $$\mfk p$$ as well. This follows from unique factorization and the fact that $$\sigma$$ fixes $$\cOO_L$$ and $$K$$: 

$$
\sigma(\mfk p^e) = \sigma(\mfk p \cOO_L) = \mfk p \cOO_L = \prod_{i=1}^g \sigma(\mfk q_i)^{e_i}
$$

Furthermore, one can show the Galois group actually acts transitively on the primes lying over $$\mfk p$$, and that we have isomorphisms $$\cOO_L / \mfk q \to \cOO_L / \sigma(\mfk q)$$ for any $$\sigma \in \Gal(L / K)$$, which forces all of the $$e$$s and $$f$$s to be the same. This then yields 

> Theorem ($$efg$$ lemma). Let $$L / K$$ be a Galois extension of number fields of degree $$n$$, and $$\mfk p \subseteq \cOO_K$$ a prime ideal. Then $$\mfk p \cOO_L = \left( \prod_{i=1}^g \mfk q_i \right)^{e}$$, and if $$f = [\cOO_L / \mfk q_i : \cOO_K / \mfk p]$$, then $$n = efg$$. 

I really like this result, it's so neat and pretty and just another reason why Galois extensions are so nice! 

{% enddetails %}

The argument we'll follow appears to be fairly standard, and appears in Milne's
algebraic number theory notes and as an exercise in Washington's 
_Introduction to Cyclotomic Fields_. 

First, recall that the extension $$L / \QQ$$ for $$L = \QQ[\zeta_n]$$ is Galois 
for all $$n$$, with Galois group $$(\ZZ / n \ZZ)^\times$$ characterized by automorphisms
$$\zeta_n \mapsto \zeta_n^a$$ with $$\gcd(a, n) = 1$$. When $$n = 23$$, the
Galois group is the cyclic group $$\ZZ / 22 \ZZ$$, which has a unique subgroup of 
index 2. By the Galois Correspondence, this subgroup of the Galois group corresponds
to a unique quadratic extension $$K / \QQ$$ contained in $$L$$, so that $$K = \QQ(\sqrt d)$$ 
for some (squarefree) integer $$d$$. 

We can actually identify this extension explicitly! Since $$n$$ is prime, we know that 
the discriminant of $$O_L$$ over $$\ZZ$$ is (up to a sign) a power of $$n = 23$$, 
and the only prime that ramifies in $$O_L$$ is therefore $$23$$. Any prime that ramifies in 
$$K$$ also ramifies in $$L$$ (from the multiplicativity of ramification indices) and the only primes that ramify in $$K$$ are the ones that
divide $$d$$ (or $$2$$, if $$d \not \equiv 1 \mod 4$$). Hence, $$d$$ can only be
divisible by $$23$$ and so $$d = \pm 23$$. We can also eliminate $$d = 23$$, since if $$K = \QQ(\sqrt{23})$$, we know that $$23 \equiv 3 \mod 4$$, and so $$2$$ ramifies in $$K$$ but not in $$L$$, contradiction. Hence $$K = \QQ(\sqrt{-23})$$, 
and since $$-23 \equiv 1 \mod 4$$, $$\cOO_K = \ZZ [\theta]$$, where 
$$\theta = \frac{1+ \sqrt{-23}}2$$. $$\theta$$ is a root of $$x^2 - x + 6 = 0$$ -- let its conjugate root is $$\theta^*$$. 

Now, we investigate $$\cOO_K$$. The idea eventually will be to find a non-principal prime ideal in $$\cOO_K$$ that lies under a non-principal prime ideal in $$\cOO_L$$, which will show that the class group of $$\cOO_L$$ is not trivial. 

The splitting of primes in quadratic extensions is pretty well-known. With some 
elementary arguments, one can see that if we let $$\mfk p = \idl{2, \theta}
\subseteq \cOO_L$$ and $$\mfk p^* = \idl{2, \theta^*}$$, one can see that $$\idl 2$$ splits in $$\cOO_K$$ as $$\mfk p \mfk p^*$$ by direct computation. $$\mfk p$$ is not principal -- if $$\mfk p = \idl \alpha$$ for some $$\alpha \in \cOO_K$$, then note that by comparing (absolute) ideal norms, $$N(\mfk p) = |N_K(\alpha)| = 2$$, since $$\cOO_K / \mfk p = \ZZ[\theta] / \idl{2, \theta} = \ZZ / 2\ZZ$$. But note that there aren't any elements of norm $$2$$ in $$\cOO_K$$ -- the norm of an element $$a + b\theta$$ in $$\cOO_K$$ is $$a^2 + ab + 6 b^2$$, and if $$a^2 + ab + 6b^2 - 2 = 0$$ for some integers $$a, b$$, then $$a = \frac 12 (-b \pm \sqrt{8 - 23 b^2})$$, so $$a = b = 0$$, contradiction. However, we can directly compute $$\mfk p^3 = \idl{\theta - 2}$$, which is principal! (Note that from computation with generators, $$\mfk p^2 = \idl{4, \theta - 2}$$ which cannot be principal, and $$\mfk p^3$$ is principal with generator $$\theta - 2$$.) 

Now, pick a prime $$\mfk P \subseteq O_K$$ lying over $$\mfk p$$. We claim that $$\mfk P$$ is non-principal. If it was, and we had $$\mfk P = \idl x$$ for some $$x \in \cOO_L$$, then the relative ideal norm gives that $$N(\mfk P) = \idl{N_{L/K}(x)}$$, which is principal. However, by definition, the relative ideal norm of $$\mfk P$$ in $$L / K$$ is $$\mfk p^{f(\mfk P/\mfk p)}$$, and the $$efg$$ lemma says that $$f(\mfk P / \mfk p) \mid [L: K] = 11$$. Therefore, the class $$[\mfk p]^{f(\mfk P / \mfk p)}$$ is nontrivial, as $$3 \not \mid 1, 11$$. This is a contradiction, so $$[\mfk P]$$ could not have been trivial, and hence $$L$$ has nontrivial class group! 

Exercise 3.17 in Marcus' _Number Fields_ allows you to do a little better. One can define a homomorphism of class groups $$\Cl_L \to \Cl_K$$ that allows us to see that the order of $$[\mfk p]$$ in $$\Cl_K$$ divides the order of $$[\mfk P]$$ multiplied by $$f(\mfk P / \mfk p)$$ (Exercise 3.16 in Marcus). This actually shows us that the order of the class group $$\Cl_L$$ must be divisible by $$3$$, regardless of whether $$f(\mfk P / \mfk p)$$ is $$1$$ or $$11$$. In cyclotomic extensions, it also happens to be the case that $$f(\mfk P / 2)$$ is the order of $$2$$ mod $$23$$ which is $$11$$, and therefore from the multiplicativity of the inertia degree and applying the $$efg$$ lemma to $$K / \QQ$$, we see that $$f(\mfk P / \mfk p)$$ is exactly $$11$$.

It's tricky to see exactly what's special about $$23$$ that makes this the smallest natural number $$n$$ where the class number of $$\QQ(\zeta_n)$$ is bigger than $$1$$. I think it's just an artifact of being a prime that's $$3 \mod 4$$ creating a just-large-enough degree extension so that the Law of Small Numbers can't kick in and force the various degrees/indices to play nicely. I bet that if one were to trace through this argument again with a smaller prime, you could potentially see this play out and the argument would fail. 

Kummer was able to exhibit this explicitly, but with a lengthy computation for a specific element of the ring with a specific norm. (There's an account of this by H. M. Edwards in _Fermat's Last Theorem: A Genetic Introduction to Algebraic Number Theory_.) However, people tend to care more about the story of irregular primes, i.e. primes $$p$$ that don't divide the class number of the field $$\QQ(\zeta_p)$$, for which Kummer's argument for proving Fermat's Last Theorem works. In this context, the number $$37$$ gets to be the first _irregular_ prime, i.e. a nasty prime that coincidentally divides the size of its class group. It's also worth noting that Kummer did all of this stuff with an older formalism of using "_ideal numbers_" (which is where the word _ideal_ comes from). Franz Lemmermeyer has a paper _"Jacobi and Kummer's Ideal Numbers"_ attempting to translate Kummer's ideas into modern mathematical language. There are some different issues you run into when attempting to develop algebraic number theory in this way, which essentially view prime ideals as kernels of homomorphisms and thinking of certain ring homomorphisms as the "primes" instead. If I have some free time at some point, I'd like to read this closer and share how this worked! 


### the second story: the orchard
at Hampshire, the first afternoon lecture (Prime Time Theorem) of the program is almost always delivered by Kelly. I really enjoyed Kelly's delivery of this talk and the results in the talk[^4], and I'd like to share some of them here. 

Suppose we are standing in an infinite orchard of apple trees, spaced evenly in a square grid $$1$$ meter apart, except at one point, where we are standing. The trees aren't infinitely thin poles, and have some thickness (i.e. a radius less than a $$\frac 12$$ meter). We'd like to know how far we can see in the orchard before a tree blocks our line of sight. 

This sounds a bit like Dirichlet's approximation theorem that allows us to approximate any real number arbitrarily closely by a rational number, which can be proved with elementary means. It turns out _Minkowski's Theorem_ can be used to prove the same approximation theorem, and the method of using Minkowski's Theorem to construct this approximation gives us an answer to our original question. 

Without further ado, here's Minkowski's Theorem, perhaps the fundamental theorem of the geometry of numbers: 

> Theorem. Let $$X$$ be a convex, centrally symmetric subset of the plane ($$\RR^2$$) with area larger than $$4$$. Then $$X$$ contains a nonzero lattice point. 

_Proof._ The argument is an application of the (infinite) Pigeonhole Principle.[^5] Divide the plane into squares of the form $$[-1, 1]^2 + 2\ZZ^2$$. By the infinite Pigeonhole Principle, there are two points in $$X$$ separated by an integer vector $$(x, y)$$ and $$(x + 2m, y + 2n)$$ for $$m, n \in \ZZ$$ with $$m, n$$ not both zero. (This maybe has some issues if you'd like to work with area very carefully, but morally, I think this is okay.) 

Since $$X$$ is centrally symmetric, $$(-x, -y)$$ is also a point in $$X$$. The midpoint of $$(-x, -y)$$ and $$(x + 2m, y+ 2n)$$ must lie in $$X$$ since $$X$$ is convex, in which case $$(m, n)$$ is our desired lattice point in $$X$$. 

Now, when I learned about this fact for the first time, I thought it was a cool result and had a really nice elementary argument with some interesting applications. A few years later, though, I was completely blindsided by seeing this theorem crop up in the context of the geometry of numbers as in algebraic number theory (although maybe I should have guessed it would, since $$\cOO_K \iso \ZZ^n$$ if $$n = [K : \QQ]$$ and we do often think of embeddings of $$K$$ into $$\RR^n$$ with $$\cOO_K$$ embedding as a lattice). 

It's probably not too hard to see that Minkowski's Theorem should extend to arbitrary $$n$$-dimensional space, if we replace $$4$$ for the area of the region on the plane with an $$n$$-dimensional volume of $$2^n$$ in $$\RR^n$$. This generalized version is what allows one to bound the size of the class group of a ring of integers, which is super neat. 

In broad strokes, here's how the argument goes. Suppose $$K$$ is our number field, $$\cOO_K$$ is our ring of integers, and $$\Cl_K$$ is the ideal class group. To do this, we show that for any class in $$\Cl_K$$, we can pick a representative of bounded norm, but then there are finitely many ideals of a bounded norm, done. 
It suffices to show that for any ideal $$I$$, there is a constant $$C$$ independent of $$I$$ such that there exists an element $$\alpha \in I$$ with $$|N_K(\alpha)| \leq C N(I)$$. If we have this fact, then for any class $$\gamma \in \Cl_K$$, consider $$\gamma^{-1}$$ represented by some fractional ideal $$J$$. Find a $$\beta \in \cOO_K$$ such that $$\beta J$$ is an ideal in $$\cOO_K$$. By assumption, we can find $$\alpha \in \beta J$$ such that $$|N_K(\alpha)| \leq C N(\beta J)$$. Now since $$\idl{\alpha} \subseteq \beta J$$, this means there is an ideal $$I$$ representing the class $$\gamma$$ such that $$\beta IJ = \idl \alpha$$, and then passing to norms gives that $$N(I) N(\beta J) = |N_K(\alpha)| \leq C N(\beta J)$$, so $$N(I) \leq C$$ is bounded. 

Therefore, the argument comes down to doing an analysis using the geometry of numbers so that we can find some $$\alpha$$ in an ideal $$I$$ whose norm is small enough to be bounded by $$N(I)$$. A lucrative idea is to then embed $$I$$ as a lattice in some real space (which is morally like $$\ZZ^n$$ up to a linear transformation), and the existence of an element of $$I$$ is like picking an element out of a lattice, which Minkowski's Theorem does. In particular, one often thinks of the $$r$$ real embeddings and $$s$$ conjugate pairs of complex embeddings of $$K$$, and thinking about the real vector space $$V = \RR^r \times \CC^s \iso \RR^n$$ for $$n = r + 2s$$, with the norm 

$$
\norm x = \sum_{i=1}^r \abs{x_i} + 2 \sum_{i = r+1}^{r+s} \abs{z_i}
$$ 

on coordinates $$x = (x_1, \dots, x_r, z_{r+1}, \dots, z_{r+s})$$. Under this norm, it turns out the closed unit ball is convex and centrally symmetric.[^6] $$I$$ is a lattice inside this space under this embedding, but with a larger fundamental domain (it has covolume $$N(I) \sqrt{\abs{\disc K}}$$). Then, with some adjustment to be done to the Minkowski inequality, we can pick the closed ball of sufficiently large radius and guarantee an element of $$I$$ must be contained within it. The norm of this element in this vector space is an upper bound for the norm of the element in $$K$$ by the AM-GM inequality, which allows us to derive the desired inequality. Based on the geometry of this norm, our constant looks pretty weird in the end (since it comes from some integration stuff): 

$$
|N_K(\alpha)| \leq \left(\frac 4 \pi\right)^s \frac{n!}{n^n} N(I) \sqrt{\abs{\disc K}} 
$$

but there it is! Minkowski's Theorem, as it turns out, is a critical ingredient in this important finiteness bound in algebraic number theory, and the geometry of numbers really was important to make this work. 


### coda 
I learned of Kelly's passing about a month ago. Before then, the ideas for this post had been circulating in my brain for a few months, since I wanted to write about algebraic number theory after winter quarter. The algebra class I'd been taking had just done Galois theory and a smattering of theory for Dedekind domains without doing much (if any) of the coolest bits. I wanted to return to my number-theoretic roots to articulate some of the great things we'd missed in class, but facing burnout after overcommitting in the winter and still busy in the spring, I never really got around to it until now. 

Hearing of Kelly's passing has reminded me of the things I've learned from him, the program he founded, and the people who I've been so lucky to meet and learn from because of it. I'm endlessly grateful that I get to enjoy  the path that I'm on primarily due to his legacy and impact on my life, although it's hard to convey that gratitude now. I figure my best way of expressing that is to honor the ideas that he stood for and connect to the mathematics that I associate closest with him. But failing that --

Kelly, thanks for being an inspiration, a role model, and an exceptional mathematician. We miss you lots. 


[^1]: yes, the same Spivak of calculus textbook fame! References to a "Steve Neen" and yellow pigs of various kinds (a cowardly cop, perhaps) can be found in his books, which is no coincidence...

[^2]: you can find this story (and others) thanks to Vincent Lefèvre [here](https://www.vinc17.net/yp17/index.en.html).

[^3]: in doing some digging, I did come across a Wikipedia page for the "23 enigma," which apparently is a documented phenomenon of people who numerologically believe in the significance of the number 23. Maybe this is why this number was chosen? 

[^4]: as a student at Hampshire in 2019, I went back into old program journals and found out that I had written the report on this talk (and didn't do so bad of a job at it!)  

[^5]: or, as veterans of the program know, this is the Pigs-in-holes Principle.
<!-- https://math.stackexchange.com/questions/117832/is-there-any-trivial-reason-for-2-is-irreducible-in-mathbbz-omega-omega/265129#265129 -->

[^6]: apparently if you're convinced this norm is actually a norm, then the unit ball in a normed linear space is always convex. I think this is an elementary triangle inequality calculation.... 
<!-- [^6]: we touched a little on convexity/uniform convexity in Banach spaces in functional analysis this spring. I think one quick way to see this is to view this Banach space as a product of $$\RR^r$$ with the 1-norm and a $$s$$ copies of $$\RR^2$$ with the 2-norm, and then the norm defined on the product is a uniformly convex product, on uniformly convex spaces. this is probably a overkill  -->