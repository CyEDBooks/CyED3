---
title: "Session 2 Practice: Languages and Regular Expressions"
---

<!--
Copyright (c) 2026 Angela Villota, and collaborators from the CyED block
Licensed under the PolyForm Noncommercial License 1.0.0.
Commercial use is prohibited without prior written authorization.
-->

# Session 2 practice

Complete the set and language operations by hand. Use the
[Python notebook](regex-python.ipynb) only for the programming section or to
test examples after you have designed an expression.

## Part A — Language operations

### Exercise 1

Let $A=\{a,b,c\}$ and $B=\{c,d,e\}$. Compute:

1. $A\cup B^2$
2. the first three powers contributing to $(AB)^*$
3. $(BA)^R$

:::{dropdown} Hint
Compute the innermost operation first. Since each member of $B$ has length one,
$B^2$ has all nine two-symbol concatenations. $(BA)^R=A^RB^R$.
:::

:::{dropdown} Partial solution
$B^2=\{cc,cd,ce,dc,dd,de,ec,ed,ee\}$. Therefore $A\cup B^2$ contains the
three one-symbol strings from $A$ and those nine strings. For $(AB)^*$, begin
with $\{\varepsilon\}\cup AB\cup(AB)^2$ and describe rather than exhaustively
list later powers.
:::

### Exercise 2

Prove that

$$
(A\cup B)^R=A^R\cup B^R.
$$

:::{dropdown} Partial proof
Take $w\in(A\cup B)^R$. Then $w=u^R$ for some $u\in A\cup B$. Split into the
cases $u\in A$ and $u\in B$ to prove the first inclusion. Reverse the argument
for the other inclusion.
:::

### Exercise 3

Prove that $(A^R)^R=A$ and $(A^+)^R=(A^R)^+$.

:::{dropdown} Hint
The first identity follows from $(u^R)^R=u$. For the second, write a member of
$A^+$ as $u_1u_2\cdots u_n$ and reverse the concatenation.
:::

### Exercise 4

Let

$$
L_1=\{\varepsilon,0,10,11\},
\qquad
L_2=\{\varepsilon,1,01,11\}.
$$

Compute $L_1L_2$, $L_2L_1$, $L_1\cup L_2$, $L_1\cap L_2$,
$L_1\setminus L_2$, and $L_2\setminus L_1$. Describe $L_1^*$ and $L_2^*$
without trying to list every member.

:::{dropdown} Hint
There are $4\times4$ concatenation pairs, but duplicate results appear because
$\varepsilon$ is in each language. A closure is infinite here, so give a
generating description and its first few members.
:::

### Exercise 5

Let $A=\{ab,b,cb\}$ and $B=\{a,ba\}$. Compute:

$$
A\cup B^2,\qquad (B\cup A)^R,\qquad AB,
\qquad A^2\cap BA,\qquad (A^R\setminus B)^2.
$$

:::{dropdown} Partial solution
$B^2=\{aa,aba,baa,baba\}$ and $A^R=\{ba,b,bc\}$. Continue one operation at a
time, removing duplicate strings after every set construction.
:::

## Part B — Designing formal regular expressions

Use `|` for union and parentheses for grouping. Unless an exercise says
otherwise, $\Sigma=\{0,1\}$.

### Exercise 6: Fixed beginning and ending

Find expressions for:

1. strings over $\{0,1,2\}$ that start with `2` and end with `1`;
2. binary strings that start with `11`;
3. binary strings of length at least four.

:::{dropdown} Partial solution
The first expression is $2(0|1|2)^*1$. For the other two, place the required
fixed part first and use $(0|1)^*$ for an arbitrary continuation. For minimum
length, require four arbitrary symbols before allowing any continuation.
:::

### Exercise 7: Counting occurrences

Find expressions for binary strings with:

1. at most two zeros;
2. a number of zeros divisible by three;
3. at least one zero and at least one one.

:::{dropdown} Hint 1
Blocks of unrestricted ones, $1^*$, are useful when zeros are the objects being
counted.
:::

:::{dropdown} Hint 2
For a multiple of three zeros, create a repeatable block containing exactly
three zeros, with any number of ones around and between them.
:::

:::{dropdown} Partial solution
At most two zeros can be written

$$
1^*(\varepsilon|01^*)(\varepsilon|01^*).
$$

For “at least one of each,” split into the two possible orders in which the
first required `0` and `1` can appear.
:::

### Exercise 8: Required substrings

Find expressions for strings that:

1. contain `11`;
2. start or end with `00` or `11`;
3. never contain `111`.

:::{dropdown} Hint
“Contains” normally has $\Sigma^*$ on both sides of a required substring. For
the third item, build the string from blocks in which every run of ones has
length at most two.
:::

:::{dropdown} Partial solution
Item 1 is $(0|1)^*11(0|1)^*$. Item 2 is a union of a “starts with” expression
and an “ends with” expression; duplicates are harmless. For item 3, consider
repeating blocks $0$, $10$, or $110$, with an optional final `1` or `11`.
:::

### Exercise 9: Bounded length

Find a regular expression for all binary strings of length at most five.

:::{dropdown} Partial solution
Let $S=(0|1)$. Then enumerate the allowed lengths:

$$
\varepsilon|S|S^2|\underline{\hspace{4cm}}.
$$
:::

### Exercise 10: Exactly one occurrence

Find a regular expression for binary strings containing exactly one occurrence
of `000`. Overlapping occurrences count separately: `0000` contains two
occurrences of `000`.

:::{dropdown} Strategy
This is more subtle than inserting `000` between arbitrary strings. The prefix
and suffix must contain no `000`, the prefix must not end in `0`, and the suffix
must not begin with `0`; otherwise a new occurrence overlaps the required one.
First design a reusable expression for strings avoiding `000`, then impose the
boundary restrictions around the unique occurrence.
:::

## Part C — Connect theory to Python

For each task, write both a formal expression and a Python pattern. State which
Python operation you would use.

### Exercise 11: Full-string validation

Validate a nine-digit telephone number written in any of these forms:

- `555555555`
- `555-555-555`
- `555 555 555`

Require the same separator in both positions when a separator is present.

:::{dropdown} Hint
Capture an optional separator after the first group and use a backreference for
the second separator. Validation suggests `re.fullmatch`.
:::

### Exercise 12: Name lines

Match a complete line beginning with `Name:` followed by one or more alphabetic
characters or spaces. Do not accept digits after the colon.

:::{dropdown} Partial solution
A starting point is `r"Name: [A-Za-z ]+"`. Decide whether to add anchors or use
`fullmatch`, and test whether a name containing only spaces should be valid.
:::

### Exercise 13: Whole words

Explain the difference between the Python patterns `r"hello"` and
`r"\bhello\b"`. Give one string matched by the first but not the second.

:::{dropdown} Hint
Try placing letters immediately before or after `hello`.
:::

### Exercise 14: Extract structured data

From `"Meeting at 09:45, lunch at 12:30"`, extract each full time and its hour
and minute as separate groups.

:::{dropdown} Partial solution
Use two digit groups separated by a literal colon. `finditer` lets you inspect
the full match, captured groups, and location of every result.
:::

## Readiness checklist

- [ ] I can compute $A^n$, $A^*$, $A^+$, and $A^R$.
- [ ] I know when $\varepsilon$ can belong to $A^+$.
- [ ] I can build a regular expression from fixed and unrestricted regions.
- [ ] I test expressions with invalid and boundary examples.
- [ ] I can explain the difference between `fullmatch`, `match`, and `search`.
- [ ] I use raw strings for Python regex patterns.
- [ ] I can use groups, `finditer`, and substitutions.

Return to the [Session 2 study page](index.md) or continue in the
[Python notebook](regex-python.ipynb).
