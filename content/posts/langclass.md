---
title: "Programming Language Classifier"
date: 2019-11-18T16:22:11-07:00
draft: false
tags: ["machine learning", "classifier"]
---

In this post, I'll explain some of the details that went into
<http://langclass.kevinlui.org/>. A fun thing I did was feature importance
recovery after feature hashing. I'll explain that at the end.

# Data source

The data was retrieve from the [Rosetta Code
Project](https://rosettacode.org/wiki/Rosetta_Code) using this git repo:
<https://github.com/acmeism/RosettaCodeData>. The features are code snippets
and the labels are programming languages.

# Tokenization

We consider 2 different tokenizers.

- character-level tokenization. 
```
print('Hello World') ==> ['p', 'r', 'i', 'n' 't', '(', ...]
```

- split on non-alphanumerical. 
```
print('Hello World') ==> ['print', '(', 'Hello', 'World', ')']
```

# Vectorization

After tokenizing, we consider consider 1-grams followed by feature hashing
modulo 4098. We also do the same with 2-grams.

# Classifier

We use [lightgbm](https://lightgbm.readthedocs.io/en/latest/) with default
parameters. There's a lot of tuning to be done but that wasn't my main interest
so I'm putting it off.

# Feature Importance

A fun thing I did was feature importance extraction despite feature hashing.
After training a tree-based model on feature-hashed inputs, you can normally
only determine the feature importance of the hash values. For example, you
might discover that the hash 123 has high importance. But this does not tell
you which one of our original tokens has high importance. You can determine
which tokens hashes to 123 but it could be the case that many tokens share the
same hash value. In the next section, we share an idea for recovering the
feature importance of the original tokens.

The top features for character-level 2-grams are:

```
[';\n', ')\n', ' (', ', ', ' =', '= ', 't(', 't ', 'in', '# ']
```

The top features for 1-grams where we split on non-alphanumericals are:

```
[';', '=', '.', ':', 'print', ',', '"', '#', ')', '-']
```

The most importance feature in both cases contains a semi-colon. This is
unsurprising since some programming languages seldomly use semi-colons (for
example, Python) while some programming languages use semi-colons extensively
(for example, C).

# Mathematical Heuristic

This technique is mainly inspired by bloom-filters. For ease of exposition,
we'll focus on the character-level 2-grams case. Let $X$ be the space of
character-level 2-grams, $Y$ be the hashed space, and $H_1, H_2, H_3:X \to Y$
be 3 different hash functions. In our implementation, we take $H_i$ to be the
murmur3 hash with seed $i$.

For $x\in X$, let $f_x$ be the feature importance of $x$ if we were to train
our model using unhashed features. For $y\in Y$ and $t\in \\{1,2,3\\}$, let
$g_y ^t$ be the feature importance of $y$ using the hash function $H_t$. For
our heuristic, we assume that $g_y ^t$ is approximately $\sum\_\{x\in H_t
^{-1}(y)\} f_x$.

Our model is trained on the hashed space so we have access to $g_y ^t$. Our
goal is to understand $f_x$. We can of course approximate the solution to a
massive linear system so try to recover $f_x$ but what we're really interested
is a ranking of $f_x$. So do this, we'll use the geometric-mean of the hashed
values. In particular, let $F_x = (\prod\_\{t=1\} ^3 g\_\{H\_t(x)\} ^t)^\{1
/3\}$. Then we can show that if $f_x > f_y$, then
$\mathbb{E}(F_x)>\mathbb{E}(F_y)$.
