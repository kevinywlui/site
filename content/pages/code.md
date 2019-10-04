---
title: "Code"
menu: main
feedback: false
meta: false
weight: -230
layout: "simple-static"
type: page
---

# Sagemath

Sagemath (<http://sagemath.org>) is a free open-source mathematics software
system started by my thesis advisor [William Stein](http://wstein.org). The
Sagemath library is primarily written in Python and Cython. Most of my
contributions are written in Python.

- [Tickets
  authored](https://trac.sagemath.org/query?author=~Kevin+Lui&group=component&max=0&col=id&col=summary&col=owner&col=type&col=status&col=priority&col=reporter&order=id)
- [Tickets
  reviewed](https://trac.sagemath.org/query?reviewer=~Kevin+Lui&group=component&max=0&col=id&col=summary&col=owner&col=type&col=status&col=priority&col=reporter&order=id)

---

#### Bigger Sagemath Projects

##### GSoC

In 2016, I was able to spend the summer working on Sagemath. I collaborated
with William Stein (<https://github.com/williamstein/sage_modabvar>) to improve
the modular abelian variety functionality of Sagemath. Eventually this project
morphed into my thesis project. This code has since been merge back into
Sagemath.

##### Isomorphic Testing

As part of my thesis, I enumerated the isogeny class of simple modular abelian
varieties, up to isomorphism. Informally, two objects in mathematics are
isomorphic if they're effectively the same. A necessary step in my enumeration
is then to be able to determine when two simple modular abelian varieties are
isomorphic. The key idea is use the fact that endomorphism rings of simple
modular abelian varieties are isomorphic to orders of number field to reduce
isomorphic testing to solving norm equations. I've implemented this
functionality and have submitted a
[ticket](https://trac.sagemath.org/ticket/28275).

---

# Caleb 

`caleb` (<https://github.com/kevinywlui/caleb>) is a Python package that helps
fill in bibtex entries using <http://crossref.org>. My usual workflow in Latex
is to write `\cite{mazur:eisenstein}` and then look up a bibtex entry with
author field containing mazur and title field containing eisenstein, and then
append it to my `biblio.bib` file. The purpose of `caleb` is to do this
automatically with the <http://crossref.org> API.

At it's core, `caleb` is a pretty simple program. My primary motivation for
writing `caleb` was to learn making a `pip` installable package, using
`pytest`, setting up `travis`, and using `poetry`.

---

# zlong_alert.zsh

`zlong_alert.zsh` (<https://github.com/kevinywlui/zlong_alert.zsh>) is a zsh
plugin to alert you upon the completion of long-running commands. It began with
this blog post: <https://kevinlui.org/posts/alerting_long_commands_in_zsh/>.

---

# oeiswall

`oeiswall` (<https://github.com/kevinywlui/oeiswall>) is a quick little Python
script to make a nerdy wallpaper with data coming from OEIS. 
