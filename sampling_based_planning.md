The "traditional" approach for motion planning is to discretize the search space
and perform graph search. But what if the search space is high dimensional?
Then, if we choose a discretization resolution such that each dimension has $D$
discrete cells, the size of the resulting graph increases like $\Theta(D^N)$,
where $N$ is the dimensionality of the search space. This exponential scaling is
known as the "curse of dimensionality".

What to do? Rather than exhaustively searching, we can *sample* configurations
from the configuration space and attempt to connect these samples.

To do this, we need two things:

1. a collision checking routine, which computes
whether any given point in the configuration space belongs to the *free space*.
We don't want to waste time planning paths through obstacles.
2. an underlying metric space, so we can find nearby points to connect. A
metric space requires a "distance function" that is nonnegative, reflexive,
symmetric, and satisfies the triangle inequality. This is
easy when we're in a Euclidean space, but harder in more complex C-spaces, related
to the problem of finding geodesics on curved surfaces. See *Planning
Algorithms* Chapter 5 for more details.

# Sampling Theory

The fundamental choice here is whether to sample *randomly* or
*deterministically.*

Example of deterministic sampling: van der Corput sequence

Either way, we need to know something about how well we are covering the space.

Consider generating an infinite stream of samples from the configuration space
$C$.

One property that we want for this sample sequence is for the sequence to be
**dense**, a notion from topology. What this means is it is possible for the
sequence to get arbitrarily close to every point in $C$. It is possible to
achieve a dense sampling sequence even in uncountably infinite sets. For
example, consider that the set $\mathbb{Q}$ of rational numbers is dense in
the set of real numbers $\mathbb{R}$. Why? For any real number, there is a
sequence of rational fractions that converges to the real number. So by sampling
from the rational numbers, the resulting sequence will be *dense* in
$\mathbb{R}$. By sampling long enough, we will get arbitrarily close to any
element of the underlying set.

On top of being dense, we also want our sequence of sampled points to minimize
**dispersion**. We can define the dispersion of set of samples $P$ in a metric
space $X$ with measure function $\rho$ as

```math
\delta(P) = \max_{x \in X} \left{ \min_{p \in P} \left{ \rho(x,p) \right} \right}.
```

Intuitively, this finds the part of the metric space that is furthest from any
samples in the sequence $P$. In other words, it's a measure of the largest
uncovered area. By minimizing this quantity, we minimize the size of the largest
empty space not covered by any samples. If using the $L_2$ metric in a two
dimensional euclidean space, we would be minimizing the size of the largest
circle we can draw that is completely empty of samples.

The final property of sampling sequences that we'll discuss is **discrepancy.**
Discrepancy is focused on the problem of making sure samples are distributed
uniformly through the space and not too aligned. Besides the curse of
dimensionality, one of the problems with grid discretizations is that the grid
will sometimes "line up" with geometric features of the environment in ways that
are either especially good or especially bad for planning. We want to reduce the
sensitivity of our planning algorithms to alignment issues during sampling.

To measure discrepancy, we need to choose a collection of subsets of $C$; in
most cases, the collection chosen is the set of all axis-aligned rectangular
subsets. Call this the range space $\mathcal{R}$. Let $C$ be a measure space such that
the size of subsets is measured with function $\mu$. Then, given a point sequence $P$ from
$C$ containing $k$ samples, discrepancy is defined as

```math
D(P,\mathcal{R}) = \max_{R \in \mathcal{R}} \left{ \left| \frac{\left| P \intersect R \right|}{k} -
\frac{\mu(R)}{\mu{C} \right| \right},
```

where $|P \intersect R|$ is the number of points in $P \intersect R$.
