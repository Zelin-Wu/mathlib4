# Schauder Fixed Point Theorem

This project presents a formalization of the **Schauder Fixed Point Theorem** in Lean 4. The proof is structured by reducing the infinite-dimensional case to a finite-dimensional approximation, allowing us to leverage the **Brouwer Fixed Point Theorem**. While the high-dimensional Brouwer theorem is treated as given (via a `sorry`), the rest of the topological and analytical framework is fully formalized using `Mathlib`.

## Theorem Statement

Let $K$ be a nonempty convex, and compact subset of a complete normed space $E$. If a function $f: K\to K$ is continuous, then there $f$ has a fixed point, i.e. $\exists x\in K,$ s.t. $f(x)=x.$

Formal Lean Statement

```
theorem NormedSpace.exists_mem_convex_compact_isFixedPt {E : Type*}
    [NormedField E] [NormedSpace ℝ E] [CompleteSpace E]
    {s : Set E} (hcv : Convex ℝ s) (hcm : IsCompact s) (hn : s.Nonempty) (f : C(s, s)) :
    ∃ x, Function.IsFixedPt f x
```

## The Brouwer "Sorry"

The core of the proof relies on the high-dimensional generalization of the Brouwer Fixed Point Theorem. While the 1D case (the Intermediate Value Theorem) is well-supported in `Mathlib`, the $n$-dimensional case is not yet part of the library's "ready-to-use" toolkit.

And in our proof, we use such generalized Brouwer fixed point theorem without proof:

```
theorem NormedSpace.exists_mem_convex_compact_finDim_isFixedPt {E : Type*}
    [NormedAddCommGroup E] [NormedSpace ℝ E] [FiniteDimensional ℝ E]
    {s : Set E} (hcv : Convex ℝ s) (hcm : IsCompact s) (hn : s.Nonempty) (f : C(s, s)) :
    ∃ x, Function.IsFixedPt f x :=by sorry
```

Formalizing such generalized Brouwer requires either **Algebraic Topology** (Homology groups) or **Combinatorial Topology** (Sperner’s Lemma). Both paths involve significant technical details fall outside of the scope of this project's timeline.

## Reference

We mainly follows the proof in the website [proof of Schauder fixed point theorem](https://planetmath.org/ProofOfSchauderFixedPointTheorem). And we added more details to make it suitable for formalization.

## AI Usage

 Tools like **Moogle** and **Leansearch** were used exclusively for theorem discovery. All tactic code and proof structures were written manually.

## Appendix

Here is an appendix for the detailed proof of the Schauder fixed point theorem. 

### Step 1: Reduce to finite case

Given $\epsilon = \frac{1}{M+1}$, we use the compactness of $K$ to obtain a finite $\epsilon$-ball $\{x_1, \dots, x_n\} \subseteq K$ such that $K \subseteq \bigcup B_\epsilon(x_i)$. We define $K_0 = \text{conv}(\{x_1, \dots, x_n\})$, a compact convex subset of a finite-dimensional subspace spanned by $\{x_1, \dots, x_n\} \subseteq K$

### Step 2:  Define the Schauder Projection

We define the Schauder projection $g: K \to K_0$ using a partition of unity. For each $i$, let:
$$
g_i(x) = \max(0, \epsilon - \|x - x_i\|)
$$
The projection is then defined as the convex combination:
$$
g(x) = \frac{\sum_{i=1}^n g_i(x)x_i}{\sum_{i=1}^n g_i(x)}
$$
We proved that $g$ is continuous and satisfies $\|g(x) - x\| < \epsilon$. By defining $\tilde{B} = g \circ f|_{K_0}$, we obtain a map $K_0 \to K_0$, which must have a fixed point $z_M$ by the finite-dimensional Brouwer theorem.

### Step 3: Convergence Analysis

Notice that $||f(z)-z||=||f(z)-g(f(z))|| \leq \epsilon$ . 

By varying $M$, we construct a sequence $z_M$ such that $\|f(z_M) - z_M\| \leq \frac{1}{M+1}$.

Since $K$ is compact, we can always find a subsequence convergent $z_{m_j} \to x_0$.

It suffices to show that $x_0$ is a desired fixed point for function $f$.

By triangular inequality, we can prove that $f(z_{m_j}) \to x_0$. 

And by continuity, we have $f(z_{m_j}) \to f(x_0)$.

Finally, by the uniqueness of the limit, we can conclude $f(x_0) = x_0.$

### Final Remark:

Actually, the requirement of completeness is not needed in the statement of the Schauder fixed point theorem. We only need the compactness of $K$.

However, in a non-complete normed space, it is rare to find non-trivial compact subset. For example, even the closed unit ball $B = \{x:||x||\leq 1\}$ is never compact due to Riesz's Lemma.