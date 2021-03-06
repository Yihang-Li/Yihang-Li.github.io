---
title: Propositional Logic
tags: [AI]
categories: Notes
mathjax: true
---
## Propositional Logic (命题逻辑)

> - A `proposition` is a declarative sentence that is either true or false
> - `propositional logic`, `propositional variables`

- `True Tables` define the semantics of the operators: i.e. : '&' , 'or', '->' ......
  - For Implication '->' : If p, then q
    - [Reference](https://math.stackexchange.com/questions/48161/in-classical-logic-why-is-p-rightarrow-q-true-if-both-p-and-q-are-false) Only 1 -> 0 = 0, other three cases are 1
    - when p is false, q may be either true or false, because the statement says nothing about the truth value of q.
    - q unless not p means if not p is false, then q must be true: i.e. （小航找不到女朋友）除非（他主动一点）， 即如果他不主动，他就找不到女朋友。
    - p -> q $\equiv$ not p or q
  - Conjunction (logical and); Disjunction (logical or) (associative and communicative law, distributivity)
  - Negation (logical not) (double-negation elimination)
  - Converse(p->q: q->p), Inverse（p->q: not p -> not q）, Contrapositive(p->q: not q -> not p)
  - Equivalence (bi-directional implication) : iff, if and only if, <->
- True table to evaluate `compound statement`
- Logical Operator `Precedence`
- `Tautology`(compound proposition that is always true), i.e. A or not A
- `Contradiction`(compound proposition that is always false), i.e. A and not A
- `Contingency`(neither a tautology nor a contradiction)
- `Satisfiable`(if there is at least one assignment of truth values to the variables that makes the statement true)
- `Logical equivalence`: Two compound propositions, p and q, are logically equivalent if p <->q is a tautology. Notation: p $\equiv $ q
  i.e. : De Morgan's Laws; *p->q $\equiv$ not p or q*; Distribution Law: p or (q and r) $\equiv$ (p or q) and (p or r)
- `Constructing` New Logical Equivalences
- `Bit Operations`  i.e.: or, and, xor
- Satisfiability is a `key technology` in AI and computer science (Homework: use truth table to show the three compound statements here are satisfiable.)
-
