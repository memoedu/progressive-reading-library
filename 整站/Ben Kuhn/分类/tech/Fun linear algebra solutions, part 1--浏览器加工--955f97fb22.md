Someone pointed out that my “fun linear algebra problems” post would be more useful with solutions. Here’s a solution to problems 1 and 2. (My favorite solution to problem 1 part (c) requires some group theory, so it isn’t suitable for beginning linear algebraists, but there are mildly more annoying ones using only linear algebra.)

## Problem 1

> Let `$\mathbb{F}$` be the finite field of `$p^k$` elements. Let `$V$` be a `$n$`\-dimensional vector space over `$\mathbb{F}$`.
> 
> a. How many linear transformations `$T: V \to V$` are there?
> 
> b. How many of these are invertible?

(a) Linear maps `$T: V \to V$` are in bijection with `$n \times n$` matrices over the field `$\mathbb{F}$`. Each matrix entry has `$p^k$` possibilities so the total number of matrices is `$$p^{kn^2}.$$`

(b) Recall that `$T$` is invertible iff it maps a basis to another basis. Then there are as many invertible maps as there are bases of `$V$`. Imagine picking such a basis `$e_1, \dots, e_n$` sequentially; we will count how many possibilities there are for each choice.

When picking vector `$e_i$`, it may not be in the span of `$e_1, \dots, e_{i-1}$`, or we get linear dependence again. That span is a vector space of dimension `$i-1$`, so it has `$|\mathbb{F}|^{i-1}$` elements. Any of the other vectors in `$V$` are available to pick, so there are `$|V| - |\mathbb{F}|^{i-1}$` possibilities. (This also holds for `$i=1$` if we define the span of the empty set to be the zero-dimensional vector space.)

Multiplying the number of possibilities for each vector together, we get `$\prod_{i=1}^n |\mathbb{F}|^n - |\mathbb{F}|^{i-1}$` or, simplifying, `$$\prod_{i=0}^{n-1} (p^{kn} - p^{ki}).$$`

(c) Let `$GL(V)$` denote the set of invertible maps `$V \to V$`; let `$SL(V)$` denote the subset of such maps of determinant 1. Note that `$GL(V)$` is a group with composition as the group law, and that `$\det : GL(n) \to \mathbb{F}^\times$` is a group homomorphism whose kernel is `$SL(V)$`. Then `$SL(V)$` is a normal subgroup of `$GL(V)$` of index `$|\mathbb{F}^\times| = p^k - 1$`, so `$$|SL(V)| = |GL(V)|/(p^k-1).$$`

## Problem 2

> Let `$f_n$` be the `$n$`th Fibonacci number.
> 
> 1.  Find a linear transformation `$T : \mathbb{R}^2 \to \mathbb{R}^2$` such that `$T(f_{n-1}, f_{n}) = (f_n, f_{n+1})$` for all `$n$`. Write down the matrix of `$T$` with respect to the standard basis.
> 2.  Find an algorithm for determining the `$n$`th Fibonacci number in `$O(\log n)$` time rather than the typical `$O(n)$` (assuming addition and multiplication are `$O(1)$`).
> 3.  What are the eigenvalues of `$T$`?
> 4.  Derive the closed-form expression for the `$n$`th Fibonacci number.

(a) The desired map is `$T(x, y) = (y, x+y)$`; its matrix is `$$M = \left(\begin{array}{cc}0 & 1 \\ 1 & 1\end{array}\right)$$`

(b) We have `$f_n$` is the first coordinate of `$T^n(0, 1)$`, so we can compute `$f_n$` if we can compute `$M^n$`. Multiplying `$2 \times 2$` matrices is constant time, so using [repeated squaring](http://en.wikipedia.org/wiki/Repeated_squaring) we can compute `$M^n$` in logarithmic time.

(c) The characteristic polynomial of `$M$` is `$x^2 - x - 1$`, so the eigenvalues are `$\frac{1 \pm \sqrt{5}}{2} = \{\phi, 1-\phi\}$` (where `$\phi$` is the golden ratio).

(d) The eigenvectors of `$T$` are `$(1, \phi)$`, corresponding to the eigenvalue `$\phi$`, and `$(1, 1-\phi)$` with eigenvalue `$(1-\phi)$`. We can decompose `$(0, 1) = \frac{(1, \phi) - (1, 1-\phi)}{\sqrt{5}}$`. Then `$$T^n(0, 1) = T^n\left(\frac{(1, \phi) - (1, 1-\phi)}{\sqrt{5}}\right) = \frac{\phi^n(1, \phi) + (1-\phi)^n(1, 1-\phi))}{\sqrt{5}},$$` and for the closed-form expression we get `$$f_n = \frac{\phi^n - (1-\phi)^n}{\sqrt{5}}.$$`