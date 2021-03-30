---
title: Propositional Logic
tags: [AI]
categories: Notes
mathjax: true
---
## Propositional Logic (命题逻辑)

### Part1

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
- `Negation Normal Form (NNF)`: A formula is said to be in NNF if it only contains `not`, `and` and `or` connectives and only atoms can be negated.

  - Every formula can be converted to NNF in linear time.
  - disjunctive normal form
  - functionally complete

### Part2

- `Literal`: a propositional variable or the negation of a propositional variable.
  (The former is positive while the latter is negative)
- `Term`: is a literal or the conjunction (and) of two or more literals.
- `Clause`: is a literal or the disjunction (or) of two or more literals.
- `Disjunctive Normal Form (DNF)`: A compound proposition is in DNF if it is a term or a disjunction of two or more terms. (i.e. an OR if ANDs).

  - Every fomula can be converted to DNF in exponential time and space
- `Conjunctive Normal Form (CNF)`: A compound proposition is in CNF if it is a clause or a conjunction of two or more clauses. (i.e. an AND of ORs).
- Propositional satisfiability is NP-hard
- 2SAT problem has linear time algo; 3SAT has no polynomial time algo: NP-hard
- `Horn clause` : is a clause which contains at most one postive literal. (General format: not A_1 or not A_2 or ... or not A_n and B, this may be rewritten as an implication: (A_1 and A_2 and ... and A_n) -> B)
- `Proof Methods` for P -> Q

  - Exhaustive proof: demonstrate for all possible cases
  - Direct proof: Assume P, prove Q
  - Proof by contradiction: Assume P and not Q; show it is false
  - Proof by contraposition: Assume not Q, prove not P
- `Inference rules` Antecedents/Consequent:
  i.e. p/(p or q) is equivalent to p -> (p or q)
- `Resolution`:
  [(l or l_1 or ... or l_n) and (not l or not l_1^{\prime} or ... or l_n^{\prime})] / (l_1 or ... or l_n or l_1^{\prime} or ... or l_n^{\prime})
  i.e. [(a or b) and (not a or c)] / (b or c)
  `(a or b) and (not a or c)->(b or c)`
- entail ( |- ) = imply ( -> )
  entailment means that one thing follows from another
- `Truth Tree` & `Assumptions`
  [Truth Tree](chrome-extension://ikhdkkncnoglghljlkmcimlnlhkeamad/pdf-viewer/web/viewer.html?file=http%3A%2F%2Fhome.sandiego.edu%2F~baber%2Flogic%2F10.TruthTrees.pdf)
  Use assumption to prove a compound proposition
-
