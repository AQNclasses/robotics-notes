---
header-includes:
  - \usepackage{amsmath}
---

# Linear Systems

Linear:

- Here, we mean linear in the sense that the relationships between system state
variables can be described with linear equations.
- Linear equations, when written as $f(x)$, obey:
    1. an **additivity property**:  $f(x1 + x2) = f(x1) + f(x2)$; and
    2. a **homogeneity property**: $f(ax) = a f(x)$

**Example 1:** a system of two switches controlling independent circuits.

We have a sensor that will report `True` if one or both of the
switches is `ON`.

Is there a way of representing this system that obeys both properties for
linearity?

Let x1 and x2 be the states of the two switches, wrapped in a vector **x** =
[x1, x2]. Our sensor reports y(**x**) = x1 OR x2. By defining `+` as `OR`,
we immediately fit the additivity property. For homogeneity, since we are
working with a binary system, the only way we can "scale" the system is to turn
circuits on or off. No matter which of the four possible states the system is
in, applying a `1` to **x** will keep all circuits in their current state, `ON`
or `OFF`, while applying a `0` will turn all circuits off (effectively, an
`AND` operator). The resulting sensor values obey linearity:

$y(0 [x1,x2]) = y([0,0]) = 0 = 0*y([x1,x2])$

$y(1 [x1, x2]) = y([x1, x2]) = 1* y([x1, x2])$

**Example 2:** A falling object.

Consider a unit mass object falling, from rest, under the influence of a gravitational
force $g$. Let $y(t)$ be the height of the object at time $t$.

We can define the system of differential equations

\begin{align\*}
\ddot{y}(t) & = -g \\
\dot{y}(t) & = \int_{t0}^t \ddot{y} dt = \dot{y}(t0) - (t-t0)g \\
y(t) & = \int_{t0}^t \dot{y} dt = y(t0) + (t-t0)\dot{y}(t0) - (t-t0)^2 g/2
\end{align\*}

which, if we consider a discrete time system where $dt = t-t0$ can be scaled to
1, we can write as

\begin{equation}
\begin{align}
\dot{y}(k+1) & = \dot{y}(k) - g \\
y(k+1) & = y(k) + \dot{y}(k) - g/2
\end{align}
\begin{equation}.

Then, if we define a state vector $[y, \dot{y}]$, we can represent our discrete system in
standard linear form as

\begin{equation}
\bm{y}(k+1) = \begin{bmatrix}
0 & 1 \\
1 & 1 
\end{bmatrix} \bm{x} + \begin{bmatrix}
0.5 \\
1
\end{bmatrix} (-g)
\end{equation}.

Note this is similar to the equation for a 2D line, $y = mx + b$, and lines are
linear (left to the reader to prove).

# Linear Estimators

Estimator:

- mathematical process for finding a "best estimate" of an underlying process,
given (sensed) data

## Linear Least Squares

Given some data,
