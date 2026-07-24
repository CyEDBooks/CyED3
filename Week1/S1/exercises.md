---
title: "Session 1 Practice: Alphabets, Strings, and Languages"
---

<!--
Copyright (c) 2026 Angela Villota, and collaborators from the CyED block
Licensed under the PolyForm Noncommercial License 1.0.0.
Commercial use is prohibited without prior written authorization.
-->

# Session 1 practice

Use this page after reading [Alphabets, strings, and languages](index.md). The
problems begin with direct applications and progress toward definitions and
proofs. Write complete arguments rather than only a final expression.

## How to use the hints

1. Attempt the problem without help.
2. If you are stuck, open only the first hint.
3. Compare your approach with the partial solution.
4. Complete the missing reasoning yourself.

The partial solutions intentionally leave some steps unfinished.

## Part A — Foundations

### Exercise 1: Building $\Sigma^*$

Let $\Sigma=\{a,b,c,d\}$. Define $\Sigma^*$ and list all its strings of length
at most one. How many strings does $\Sigma^3$ contain?

:::{dropdown} Hint 1
Start with $\Sigma^*=\bigcup_{k=0}^{\infty}\Sigma^k$. Remember that
$\Sigma^0$ has one member.
:::

:::{dropdown} Partial solution
The first two levels are

$$
\Sigma^0=\{\varepsilon\},
\qquad
\Sigma^1=\{a,b,c,d\}.
$$

Thus $\Sigma^*$ begins with $\{\varepsilon,a,b,c,d,aa,ab,\ldots\}$.
Since each of the three positions has four possible symbols,
$|\Sigma^3|=\underline{\hspace{2cm}}$.
:::

### Exercise 2: Membership and notation

Let $\Sigma=\{0,1\}$. Decide whether each statement is true or false and
justify your answer.

1. $0101\in\Sigma^*$
2. $\varepsilon\in\Sigma^+$
3. $\emptyset\subseteq\Sigma^*$
4. $\{\varepsilon\}\subseteq\Sigma^*$
5. $2\in\Sigma^*$

:::{dropdown} Hint
Keep the symbols $\varepsilon$, $\emptyset$, $\in$, and $\subseteq$ distinct.
The symbol $\in$ asks about one member; $\subseteq$ compares two sets.
:::

:::{dropdown} Partial solution
Statements 1, 3, and 4 are true. Statement 2 is false because positive closure
excludes the empty string. For statement 5, ask whether $2$ is made only from
symbols in $\Sigma$.
:::

### Exercise 3: Counting strings

An alphabet has six symbols.

1. How many strings have length exactly four?
2. How many strings have length at most four?
3. How many nonempty strings have length at most four?

:::{dropdown} Hint
Use $|\Sigma^k|=|\Sigma|^k$. “At most four” includes lengths $0,1,2,3,4$.
:::

:::{dropdown} Partial solution
The first answer is $6^4$. The second is

$$
6^0+6^1+6^2+6^3+6^4.
$$

For the third answer, remove the contribution of $\Sigma^0$.
:::

## Part B — Operations on strings

### Exercise 4: Recursive string powers

Given $u\in\Sigma^*$ and $n\in\mathbb N$, give a recursive definition of
$u^n$. Then use it to expand $u^4$ without exponent notation.

:::{dropdown} Hint
A recursive definition needs a base case and a rule that reduces the exponent.
The identity for string concatenation is $\varepsilon$.
:::

:::{dropdown} Partial solution
Begin with

$$
u^0=\varepsilon.
$$

For the recursive step, define $u^{n+1}$ using $u^n$ and one additional copy of
$u$. Applying the rule repeatedly gives $u^4=\underline{\hspace{3cm}}$.
:::

### Exercise 5: Recursive length

Give a recursive definition of $|u|$ for $u\in\Sigma^*$. Use your definition
to calculate $|abba|$ one symbol at a time.

:::{dropdown} Hint
What is the length of $\varepsilon$? What happens when one symbol $a\in\Sigma$
is appended to a string $u$?
:::

:::{dropdown} Partial solution
The base case is $|\varepsilon|=0$. The recursive rule has the form

$$
|ua|=\underline{\hspace{2cm}}.
$$

Then write $abba$ as $((\varepsilon a)b)b)a$ and apply the rule four times.
:::

### Exercise 6: Recursive reverse

Give a recursive definition of $u^R$. Use it to find $(abcab)^R$.

:::{dropdown} Hint
When the last symbol $a$ is removed from $ua$, where must that symbol appear in
the reversed string?
:::

:::{dropdown} Partial solution
Use $\varepsilon^R=\varepsilon$ as the base case. The recursive step begins

$$
(ua)^R=a\,\underline{\hspace{1.5cm}}.
$$

For $abcab$, the final $b$ becomes the first symbol of the answer.
:::

### Exercise 7: Reverse of a concatenation

Prove that, for all $u,v\in\Sigma^*$,

$$
(uv)^R=v^R u^R.
$$

Use induction on the length of $v$.

:::{dropdown} Hint 1 — Base case
Set $v=\varepsilon$. Simplify both sides using the identity property and
$\varepsilon^R=\varepsilon$.
:::

:::{dropdown} Hint 2 — Inductive step
Assume $(uv)^R=v^Ru^R$. Append a symbol $a\in\Sigma$ to $v$ and examine
$(u(va))^R$.
:::

:::{dropdown} Partial proof
For $v=\varepsilon$,

$$
(u\varepsilon)^R=u^R=\varepsilon^Ru^R.
$$

For the inductive step, suppose the identity holds for $v$. Then

$$
(u(va))^R=((uv)a)^R
=a(uv)^R
=a(v^Ru^R).
$$

Use the recursive definition to identify $a v^R$ with $(va)^R$, then finish
the equality.
:::

### Exercise 8: Prefixes, suffixes, and substrings

Let $w=automata$.

1. List every prefix of $w$.
2. List every suffix of $w$.
3. Give three substrings that are neither prefixes nor suffixes.
4. How many prefixes does a string of length $n$ have if $\varepsilon$ and the
   whole string are included?

:::{dropdown} Hint
A prefix is determined by where you stop reading from the left. Include the
choice of stopping before the first symbol.
:::

:::{dropdown} Partial solution
The prefixes begin $\varepsilon,a,au,aut,\ldots$ and end with $automata$.
There is one prefix for each possible prefix length $0,1,\ldots,n$.
:::

## Part C — Languages

### Exercise 9: Describing languages

Use set-builder notation to describe each language over $\Sigma=\{0,1\}$.

1. All strings of length five.
2. All strings that begin with $1$.
3. All strings that end with $00$.
4. All strings of even length.
5. The language containing only the empty string.

:::{dropdown} Hint
A useful template is

$$
\{w\in\Sigma^*\mid \text{property of }w\}.
$$
:::

:::{dropdown} Partial solution
The first and fourth languages may be written as

$$
L_1=\{w\in\Sigma^*\mid |w|=5\},
$$

$$
L_4=\{w\in\Sigma^*\mid |w|=2k\text{ for some }k\geq0\}.
$$

For items 2 and 3, express each string as a fixed part concatenated with an
arbitrary string from $\Sigma^*$.
:::

### Exercise 10: Boolean operations

Let

$$
A=\{\varepsilon,a,ab,ba\},
\qquad
B=\{a,b,ba\}.
$$

Compute:

1. $A\cup B$
2. $A\cap B$
3. $A\setminus B$
4. $B\setminus A$

:::{dropdown} Partial solution
$A\cap B$ contains the strings that appear in both sets, so

$$
A\cap B=\{a,ba\}.
$$

Compute the other operations by checking membership string by string. Do not
repeat members in a set.
:::

### Exercise 11: Language concatenation

Let $A=\{a,ab\}$ and $B=\{\varepsilon,b\}$. Compute $AB$ and $BA$. Are they
equal?

:::{dropdown} Hint
Create all four ordered pairs in $A\times B$. Concatenate in the order “member
of $A$ followed by member of $B$.” Repeat with $B\times A$.
:::

:::{dropdown} Partial solution
Because $\varepsilon$ is the identity,

$$
AB=\{a\varepsilon,ab\varepsilon,ab,abb\}.
$$

Simplify the strings and remove duplicates. Then calculate $BA$ independently;
do not assume concatenation is commutative.
:::

### Exercise 12: When can $AB=BA$?

Give an alphabet $\Sigma$ and two different languages $A$ and $B$ over
$\Sigma$ such that $AB=BA$. Verify the equality by calculating both sides.

:::{dropdown} Hint 1
Try languages whose strings are all powers of the same basic string.
:::

:::{dropdown} Hint 2
A very small example can use singleton languages $A=\{u\}$ and $B=\{v\}$.
Choose different strings $u$ and $v$ that commute under concatenation.
:::

:::{dropdown} Partial solution
Over $\Sigma=\{a\}$, consider two different singleton languages containing
different powers of $a$. Complete this construction and verify that the only
string in $AB$ is also the only string in $BA$.
:::

## Part D — Proof practice

### Exercise 13: Associativity of language concatenation

Prove that

$$
A(BC)=(AB)C.
$$

:::{dropdown} Hint
Prove both inclusions. Start with an arbitrary $w\in A(BC)$ and unpack the
definition of language concatenation.
:::

:::{dropdown} Partial proof
Suppose $w\in A(BC)$. Then there are $u\in A$ and $x\in BC$ such that $w=ux$.
Because $x\in BC$, there are $v\in B$ and $z\in C$ such that $x=vz$. Hence

$$
w=u(vz)=(uv)z.
$$

Since $uv\in AB$, it follows that $w\in(AB)C$. This proves one inclusion.
Prove the reverse inclusion by starting with $w\in(AB)C$ and reversing the
argument.
:::

### Exercise 14: Identity language

Prove that

$$
A\{\varepsilon\}=\{\varepsilon\}A=A.
$$

:::{dropdown} Hint
Write out the set-builder definition of each concatenation and use
$u\varepsilon=\varepsilon u=u$.
:::

:::{dropdown} Partial solution

$$
A\{\varepsilon\}
=\{u\varepsilon\mid u\in A\}
=\{u\mid u\in A\}
=A.
$$

Write the corresponding three-line argument for $\{\varepsilon\}A$.
:::

### Exercise 15: Empty language

Prove that

$$
A\emptyset=\emptyset A=\emptyset.
$$

:::{dropdown} Hint
For a string to belong to $A\emptyset$, one of its factors would need to be an
element of $\emptyset$. Can such a factor exist?
:::

:::{dropdown} Partial solution
Assume for contradiction that $w\in A\emptyset$. By the definition of
concatenation, there must be strings $u\in A$ and $v\in\emptyset$ with $w=uv$.
But no $v\in\emptyset$ exists. Therefore $A\emptyset$ has no members. Adapt the
same argument to $\emptyset A$.
:::

### Exercise 16: Distributivity

Prove that

$$
A(B\cup C)=AB\cup AC.
$$

:::{dropdown} Hint
After writing $w=uv$ with $u\in A$ and $v\in B\cup C$, split into the cases
$v\in B$ and $v\in C$.
:::

:::{dropdown} Partial solution
Membership in $B\cup C$ gives two cases. In the first, $u\in A$ and $v\in B$,
so $uv\in AB$; in the second, $uv\in AC$. This establishes membership in
$AB\cup AC$. Complete the reverse inclusion by taking an arbitrary member of
$AB\cup AC$.
:::

## Challenge problems

### Challenge 1

For a finite alphabet with $m$ symbols, derive a formula for the number of
strings whose length is at most $n$. Simplify the sum when $m\neq1$.

### Challenge 2

Find a counterexample showing that language concatenation does not generally
distribute over intersection; that is, find $A,B,C$ for which

$$
A(B\cap C)\neq AB\cap AC.
$$

### Challenge 3

Prove that if $u$ is a prefix of $v$ and $v$ is a prefix of $u$, then $u=v$.

## Readiness checklist

You are ready for the next session if you can do each task without consulting
the notes:

- [ ] Explain the difference between $\emptyset$, $\varepsilon$, and
  $\{\varepsilon\}$.
- [ ] Enumerate $\Sigma^k$ for a small alphabet and value of $k$.
- [ ] Use recursive definitions for length, power, and reverse.
- [ ] Identify prefixes, suffixes, and substrings.
- [ ] Describe an infinite language with set-builder notation.
- [ ] Compute the concatenation of two finite languages.
- [ ] Prove a language identity by element-wise reasoning.

Return to the [Session 1 study page](index.md) to review any unchecked topic.
