---
title: "Thesis"
menu: main
feedback: false
meta: false
weight: -230
layout: "simple-static"
type: page
---

I did my Ph.D. thesis with William Stein. It was a combination of two projects.

* [thesis](../../resources/thesis.pdf)

## Arithmetic of Totally Split Modular Jacobians

A modular Jacobian variety, $J$, is said to be said to be *totally split* if it
is isogenous to a product of elliptic curves. When $J=J_0(N)$, we can
explicitly define a map $\Phi:\prod E_i \to J$, where the $E_i$'s run through the
1-dimensional abelian subvarieties of $J$.

For my thesis, I leveraged the extra description of $\Phi$ and Galois
cohomology to compute the group structure of $J_0(N)(\mathbf{Q})$ for as many
totally split rank-0 $J_0(N)$ as possible. Moreover, I was able to provably
enumerate the set of totally split $J_0(N)$.

---

## Enumeration of Isogeny Classes of Prime Level Simple Modular Abelian Varieties

Let $A\subseteq J_0$ be a simple abelian subvariety of $J_0(N)$ with $N$ prime.
For my thesis, I gave an algorithm for enumerating the odd-degree isogeny class
of $A$ when $\mathrm{num} \frac{N-1}{12}$ is squarefree, Hecke algebra of
$A$ is integrally closed, and another technical condition.

Let $G = \mathrm{Gal}(\overline{\mathbf{Q}}/\mathbf{Q})$. I began by showing
every finite odd-order $G$-submodule of $A(\overline{\mathbf{Q}})$ is a Hecke
module. This allows me to split into the case of Eisenstein and non-Eisenstein
isogenies. In the Eisenstein case, I used an idea of [Klosin and
Papikan](https://arxiv.org/abs/1706.08228). In the non-Eisenstein case, I
followed an idea of Frank Calegari and was able to bound the isogeny class by
the class group of the Hecke algebra of $A$.
