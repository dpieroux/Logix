# Propositional Calculus: an Introduction

[Back to README](../README.md)

Propositional logic is a branch of logic whose goal is to establish the
__validity__ of __propositional arguments__ , that is, to ensure that the
__conclusion__ of an argument built on __propositions__ cannot be false if the
assumptions supporting the conclusion, that is the argument __premisses__ are
true.

## Primitive Concepts

Propositional calculus introduces three kinds of primitive objects:
1. The two __Boolean__ values: __true__ and __false__.
2. __Atomic propositions__.
3. __Connectives__.

The two Boolean values form the Boolean set $\mathbb{B}$. Note that in logic,
they are often called the "truth values"

The atomic propositions have no property beside being distinguishable from each
other. In order to be manipulated, each atomic proposition is represented by a
different label; it is common to use a single letter for the latter. For any
argument, the atomic propositions involved are always in finite numbers.

Notation:
* All the atomic propositions under consideration for a given problem form the
  __universe__ set $\Omega$. $\Omega$ can not be empty as any argument must have
  a conclusion that must contain at least one atomic proposition.
* In the equations, true is represented by $\top$ and False by $\bot$.

Connectives are used to build recursively __compound propositions__ from the
atomic ones. In this project five connectives are considered: $\text{not}$,
$\text{and}$, $\text{or}$, $\text{imply}$, and $\text{equiv}$.

## Compound Propositions

A proposition can be atomic or compound, the latter being obtained by combining
pre-existing propositions together with a connective according the following
rules: assuming that $p$ and $q$ are pre-existing propositions, the following
expression are all compound propositions:
* $\text{not}(p)$
* $\text{and}(p, q)$
* $\text{or}(p, q)$
* $\text{implies}(p, q)$
* $\text{equiv}(p, q)$

_Note: we use functional connectives instead of the usual unary and binary
operators to avoid the discussion on the use of the parentheses._

The set $\mathcal{P}(\Omega)$ is the set of all the propositions that can be
built from the atomic propositions of $\Omega$ and the connectives above.
$\mathcal{P}(\Omega)$ is infinite, as new propositions can always be created
recursively from previous one.

## Boolean Operators
Each connective is associated to a boolean operator:
* $\text{not}$ is associated to
  $\neg:\mathbb{B}\rightarrow\mathbb{B}:x\mapsto\neg x$
* $\text{and}$ is associated to $\land:\mathbb{B}^2\rightarrow\mathbb{B}:(x,
  y)\mapsto x\land y$
* $\text{or}$ is associated to $\lor:\mathbb{B}^2\rightarrow\mathbb{B}:(x,
  y)\mapsto x\lor y$
* $\text{implies}$ is associated to
  $\Rightarrow:\mathbb{B}^2\rightarrow\mathbb{B}:(x, y)\mapsto
  x\Rightarrow y$
* $\text{equiv}$ is associated to
  $\Leftrightarrow:\mathbb{B}^2\rightarrow\mathbb{B}:(x, y)\mapsto
  x\Leftrightarrow y$

These operators are defined by the table below:

| $x$    | $y$    | $\neg x$ | $x\land y$ | $x\lor y$ | $x\Leftrightarrow y$ | $x\Rightarrow y$ |
| ------ | ------ | -------- | ---------- | --------- | -------------------- | ---------------- |
| $\top$ | $\top$ | $\bot$   | $\top$     | $\top$    | $\top$               | $\top$           |
| $\top$ | $\bot$ |          | $\bot$     | $\top$    | $\bot$               | $\bot$           |
| $\bot$ | $\top$ | $\top$   | $\bot$     | $\top$    | $\top$               | $\bot$           |
| $\bot$ | $\bot$ |          | $\bot$     | $\bot$    | $\top$               | $\top$           |

It is important at this stage to understand that the logical connectives operate
on propositions, while the Boolean operators operates on Boolean values.

## Valuations

A __valuation__ is a function $\phi:\mathcal{P}(\Omega)\rightarrow \mathbb{B}$
that returns a Boolean value for each proposition under the following
constraints: with $p$ and $q$ being any two propositions,
1. If $p$ is atomic, then $\phi(p)$ can take any value in $\mathbb{B}$.
2. $\phi(\text{not}(p)) = \neg\phi(p)$
3. $\text{and}(p, q) = (\phi(p)\land\phi(q))$
4. $\text{or}(p, q) = (\phi(p)\lor\phi(q))$
6. $\text{implies}(p, q) = (\phi(p)\Rightarrow\phi(q))$
5. $\text{equiv}(p, q) = (\phi(p)\Leftrightarrow\phi(q))$

A valuation $\phi$ is said to __satisfy__ a proposition $p$ if $\phi(p)=\top$.

A set of propositions is logically __consistent__ if there is a valuation that
satisfy them all.

Since a valuation is free to assign any of the two Boolean values to each atomic
propositions, there are $2^n$ different possible valuations for a universe set
containing $n$ atomic propositions.

## Arguments

An argument is made of two parts, a possibly-empty set of __premisses__ and a
__conclusion__. Both the premisses and the conclusion are propositions.

An argument is __valid__ if its conclusion is true under any valuation that
makes its premisses true.

## Logic Validity Proving System

### Approach

The validity of an argument in propositional calculus can be tested by a
mechanical approach.

For instance, the truth-table approach consists in considering all the possible
valuations of the argument universe, and for each of them, computing the truth
value of the premisses and the conclusion. If it found a valuation for which the
premisses are true, and the conclusion false, then the argument is invalid;
otherwise it is valid.

The major problem with the approach above is that the number of valuations to
check before declaring that an argument is valid increases as $O(2^n)$, with $n$
the number of propositions to consider. So, Logix implements another approach,
which is sketched below.

Obviously, a argument is valid if it is not invalid, and an argument is invalid
if there is a valuation for which the premisses are true, and the conclusion
false. An equivalent statement is this is: an argument is invalid if there is a
valuation for which both the premisses and the negation of the conclusion are
true, that is, if the set of the premisses and the negation of the conclusion is
consistent.

This proposition is interesting as the propositions of the premisses and the
negation of the conclusion are put on a same foot, which allows to process them
in a similar way.

Therefore, the first step of the Logix argument proving approach is to add the
negation of the conclusion to the set of the premisses and to determine if the
set so obtained is consistent or not.

Suppose now that we have two sets of propositions $P$ and $\widetilde P$ such
that $P$ is consistent if and only $\widetilde P$ is consistent. Then showing
that $P$ is consistent is equivalent to showing that $\widetilde P$ is
consistent.

Going a step further, suppose that we have a set of propositions $P$, and a
sequence of sets of propositions $P_i$ such that $P$ is consistent if and only
if there is at least a consistent $P_i$. Then proving the consistency of $P$ is
equivalent to proving the consistency of one of the $P_i$. Inversely, the
inconsistency of all the $P_i$ proves the inconsistency of $P$.

Suppose also that the $P_i$ are such that they contain only atomic propositions
and the negation of an atomic propositions, which we call __simple proposals__
in what follows. In that case, establishing the consistence is obvious: if a set
$P_i$ contains both an atomic proposition and its negation, it is inconsistent;
otherwise it is consistent.

Therefore, the approach of Logix is the following: starting from the set $P$ of
the premisses and the negation of the conclusion, $P$ is transformed by
successive iteration into a sequence of sets $P_i$ containing only atomic
propositions and negation of atomic proposition, in such a way that $P$ is
consistency only if at least one $P_i$ is consistent.

### Transformation rules

The transformation rules to go from the set of propositions $P$ to the sequence
of propositions $P_i$ containing only simple propositions are given below. They
have to be applied recursively while there are not-simple propositions left.

The rule are presented in tables, $P(n)$ being the current version of the
proposition set, and either
* $P(n+1)$ being the iterated version of $P(n)$.
* $P_1(n+1)$ and $P_2(n+1)$ being a split of $P(n)$ into a sequence of two
  proposition sets, such that, $P_n$ is consistent only if $P_1(n+1)$ or
  $P_2(n+1)$ is consistent.


#### <u>'not' rules </u>

| $P(n)$                         | $P(n+1)$                                   |
| ------------------------------ | ------------------------------------------ |
| $\text{not}(\text{not}(p))$    | $p$                                        |
| $\text{not}(\text{and}(p, q))$ | $\text{or}(\text{not}(p), \text{not}(q))$  |
| $\text{not}(\text{or}(p, q))$  | $\text{and}(\text{not}(p), \text{not}(q))$ |


#### <u>'implies' rule </u>

| $P(n)$                | $P_1(n+1)$      | $P_2(n+1)$ |
| --------------------- | --------------- | ---------- |
| $\text{implies}(p,q)$ | $\text{not}(p)$ | $q$        |


#### <u>'equivalent' rule </u>

| $P(n)$              | $P_1(n+1)$        | $P_2(n+1)$                                |
| ------------------- | ----------------- | ----------------------------------------- |
| $\text{equiv}(p,q)$ | $\text{and}(p,q)$ | $\text{and}(\text{not}(p),\text{not}(q))$ |


#### <u>'and' rules</u>

| $P(n)$             | $P(n+1)$ |
| ------------------ | -------- |
| $\text{and}(p, q)$ | $p, q$   |


#### <u>'or' rules</u>

| $P(n)$            | $P_1(n+1)$ | $P_2(n+1)$ |
| ----------------- | ---------- | ---------- |
| $\text{or}(p, q)$ | $p$        | $q$        |


## Reference

This introduction is based on the excellent [An Introduction to Formal
Logic](https://archive.org/details/an-introduction-to-formal-logic-2nd-edition-2020-peter-smith)
by Peter Smith. Note the notion of well-formed formulas (WFFs) in the book is
played by the Python syntax and the Logix API; therefore there is no need to
introduce WFFs here.