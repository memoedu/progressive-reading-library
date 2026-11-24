While TAing a linear algebra class last semester, I discovered that there aren’t many resources for good problems in linear algebra. I only found a few problems over the semester that I was really happy going over in section (in that they were both the right difficulty, and exciting). I’ve collected them here in case anyone else finds them cool or useful.

(Incidentally, this seems like potentially a neat project—a collection of problems in math or other subjects rated for difficulty, prerequisites, fun, etc. with hints and solutions. Does this exist?)

_Update: now with [solutions](http://www.benkuhn.net/fun-linear-algebra-solutions-part-1) to 1 and 2!_

1.  Let `$\mathbb{F}$` be the finite field of `$p^n$` elements. Let `$V$` be a `$k$`\-dimensional vector space over `$\mathbb{F}$`.
    
    1.  How many linear transformations `$T: V \to V$` are there?
    2.  How many of these are invertible?
    3.  How many are of determinant 1?
2.  Let `$f_n$` be the `$n$`th Fibonacci number.
    
    1.  Find a linear transformation `$T : \mathbb{R}^2 \to \mathbb{R}^2$` such that `$T(f_{n-1}, f_{n}) = (f_n, f_{n+1})$` for all `$n$`. Write down the matrix of `$T$` with respect to the standard basis.
    2.  Find an algorithm for determining the `$n$`th Fibonacci number in `$O(\log n)$` time rather than the typical `$O(n)$` (assuming addition and multiplication are `$O(1)$`).
    3.  What are the eigenvalues of `$T$`?
    4.  Derive the closed-form expression for the `$n$`th Fibonacci number.
3.  In an `$n$`\-dimensional real inner product space, find the size of the largest possible set of vectors `$\{v_1, \dots, v_n\}$` such that `$\langle v_i, v_j\rangle$` is strictly less than 0 if `$i \ne j$`.
    
4.  Let `$A$` be a matrix. We say the _order_ of `$A$` is `$k$` if `$k$` is the smallest integer such that `$A^k = I$`.
    
    1.  Show that is `$A$` is a `$2 \times 2$` integer matrix of finite order, then `$A^{12} = I$`.
    2.  Let `$M$` be the set of `$n \times n$` integer matrices of finite order. What is the smallest `$k$` such that if `$A \in M$`, then `$A^k = I$`?