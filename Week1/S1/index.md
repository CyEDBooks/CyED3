---
title: "Session 1: Alphabets, Strings, and Languages"
---

<!--
Copyright (c) 2026 Angela Villota, and collaborators from the CyED block
Licensed under the PolyForm Noncommercial License 1.0.0.
Commercial use is prohibited without prior written authorization.
-->

# Alphabets, strings, and languages

Formal languages give us a precise way to describe structured data. A compiler
uses them to recognize valid programs, a web form uses them to validate input,
and a search tool uses them to identify patterns in text. Before studying
regular expressions and finite automata, we need a small vocabulary for talking
about symbols, strings, and sets of strings.

This session develops that vocabulary. Read the examples actively: pause before
each answer, write down your prediction, and then compare it with the explanation.

## Learning objectives

After completing this session, you should be able to:

- distinguish an alphabet, a string, and a language;
- calculate string lengths and alphabet powers;
- explain the difference between $\Sigma^*$ and $\Sigma^+$;
- concatenate and reverse strings;
- identify prefixes, suffixes, and substrings;
- represent finite and infinite languages using set notation; and
- apply Boolean operations and concatenation to languages.

## 1. Alphabets

An **alphabet** is a finite, nonempty set of symbols. We usually denote an
alphabet by the Greek letter $\Sigma$ (sigma).

Examples include:

| Alphabet | Meaning |
|---|---|
| $\Sigma=\{0,1\}$ | Binary symbols |
| $\Sigma=\{a,e,i,o,u\}$ | Lowercase vowels |
| $\Sigma=\{A,C,G,T\}$ | DNA bases |
| Printable ASCII characters | Symbols commonly used in plain text |

The members of an alphabet do not need to be letters. They may be digits,
punctuation marks, tokens, or any other objects we agree to treat as indivisible
symbols.

:::{important}
An alphabet is a **set**, so order and repetition do not matter. For example,
$\{a,b,a\}=\{a,b\}$.
:::

### Check your understanding

Which of the following are valid alphabets?

1. $\{0,1\}$
2. $\emptyset$
3. The set of all real numbers
4. $\{\texttt{if},\texttt{else},\texttt{while}\}$

:::{dropdown} Answer
Items 1 and 4 are alphabets: each is finite and nonempty. The empty set fails
the nonempty requirement, and the set of real numbers is infinite.
:::

## 2. Strings

A **string** (or **word**) over $\Sigma$ is a finite sequence of symbols from
$\Sigma$. Unlike a set, a string is ordered and may repeat symbols.

If $\Sigma=\{a,b\}$, then

$$
aba,\qquad ababaaa,\qquad aaaab
$$

are strings over $\Sigma$. The strings $aba$ and $aab$ are different because
their symbols occur in a different order.

The statement $w\in\Sigma^*$ means that $w$ is a string over $\Sigma$. We will
define $\Sigma^*$ formally in a later section.

### The empty string

The **empty string** contains no symbols. It is written $\varepsilon$ (epsilon)
or sometimes $\lambda$. This material uses $\varepsilon$ unless a source or
exercise uses $\lambda$.

The empty string is not the same object as the empty set:

$$
\varepsilon \neq \emptyset,
\qquad
\{\varepsilon\} \neq \emptyset.
$$

- $\varepsilon$ is a string containing zero symbols.
- $\{\varepsilon\}$ is a language containing one string.
- $\emptyset$ is a language containing no strings.

This distinction will matter when we concatenate languages.

### String length

The **length** of a string $w$, written $|w|$, is the number of symbol positions
in the string. For example,

$$
|aba|=3,\qquad |baaa|=4,\qquad |\varepsilon|=0.
$$

Notice that $10101$ has length $5$, even though it uses only two distinct
symbols. Length counts positions, not distinct values.

A recursive definition of length is:

$$
|\varepsilon|=0,
\qquad
|ua|=|u|+1
$$

for every $u\in\Sigma^*$ and $a\in\Sigma$. The second rule says that appending
one symbol increases the length by one.

## 3. Powers and closures of an alphabet

For a nonnegative integer $k$, $\Sigma^k$ is the set of all strings of length
$k$ over $\Sigma$:

$$
\Sigma^k=\{w\mid w\text{ is a string over }\Sigma\text{ and }|w|=k\}.
$$

Let $\Sigma=\{0,1\}$. Then:

| Power | Strings |
|---|---|
| $\Sigma^0$ | $\{\varepsilon\}$ |
| $\Sigma^1$ | $\{0,1\}$ |
| $\Sigma^2$ | $\{00,01,10,11\}$ |
| $\Sigma^3$ | $\{000,001,010,011,100,101,110,111\}$ |

If $|\Sigma|=m$, then $|\Sigma^k|=m^k$. This counts the $m$ independent
choices available at each of the $k$ positions.

### Kleene star

The **Kleene star** of $\Sigma$ is the set of every finite string over $\Sigma$:

$$
\Sigma^*=\bigcup_{k=0}^{\infty}\Sigma^k.
$$

For $\Sigma=\{0,1\}$,

$$
\Sigma^*=\{\varepsilon,0,1,00,01,10,11,000,\ldots\}.
$$

Every member of $\Sigma^*$ is finite, although $\Sigma^*$ itself is usually an
infinite set.

### Positive closure

The **positive closure** contains every nonempty string over $\Sigma$:

$$
\Sigma^+=\bigcup_{k=1}^{\infty}\Sigma^k
=\Sigma^*\setminus\{\varepsilon\}.
$$

Therefore,

$$
\Sigma^*=\Sigma^+\cup\{\varepsilon\}.
$$

:::{warning}
$\Sigma^*$ always contains $\varepsilon$. The star does not mean “one or more”;
it means “zero or more.” Positive closure, $\Sigma^+$, means “one or more.”
:::

## 4. Operations on strings

### Concatenation

The **concatenation** of strings $u$ and $v$ is formed by writing $v$
immediately after $u$. It is written $u\cdot v$ or simply $uv$.

For example, if $u=ab$ and $v=baa$, then

$$
uv=abbaa,
\qquad
vu=baaab.
$$

This example also shows that concatenation is generally **not commutative**:
$uv$ need not equal $vu$.

The empty string is the identity for concatenation:

$$
u\varepsilon=\varepsilon u=u.
$$

Concatenation is associative:

$$
u(vw)=(uv)w.
$$

Because of associativity, we normally write $uvw$ without parentheses.

### Powers of a string

String powers represent repeated concatenation. They can be defined recursively:

$$
u^0=\varepsilon,
\qquad
u^{n+1}=u^n u.
$$

If $u=ab$, then $u^0=\varepsilon$, $u^1=ab$, and $u^3=ababab$.

Do not confuse $u^n$ with $\Sigma^n$: the first is one repeated string; the
second is a set containing all strings of a particular length.

### Reverse

The **reverse** of $u=a_1a_2\cdots a_n$ is

$$
u^R=a_na_{n-1}\cdots a_1.
$$

For example, $(abca)^R=acba$ and $\varepsilon^R=\varepsilon$.

The reverse also has a useful recursive definition:

$$
\varepsilon^R=\varepsilon,
\qquad
(ua)^R=a\,u^R.
$$

One important consequence is

$$
(uv)^R=v^R u^R.
$$

The order of the two factors reverses along with the symbols.

## 5. Parts of a string

Let $u,v\in\Sigma^*$.

- $v$ is a **substring** of $u$ if there are strings $x$ and $y$ such that
  $u=xvy$.
- $v$ is a **prefix** of $u$ if there is a string $w$ such that $u=vw$.
- $v$ is a **suffix** of $u$ if there is a string $w$ such that $u=wv$.

A proper prefix or suffix is different from the whole string.

Consider $u=bcbaadb$:

| Prefixes | Suffixes |
|---|---|
| $\varepsilon$ | $\varepsilon$ |
| $b$ | $b$ |
| $bc$ | $db$ |
| $bcb$ | $adb$ |
| $bcba$ | $aadb$ |
| $bcbaa$ | $baadb$ |
| $bcbaad$ | $cbaadb$ |
| $bcbaadb$ | $bcbaadb$ |

Every prefix and suffix is also a substring, but not every substring is a
prefix or suffix. For example, $baa$ is a substring of $bcbaadb$, but it is
neither a prefix nor a suffix.

:::{note}
The empty string is a prefix, suffix, and substring of every string. Every
string is also a prefix, suffix, and substring of itself.
:::

## 6. Languages

A **language** over $\Sigma$ is any subset of $\Sigma^*$:

$$
L\subseteq\Sigma^*.
$$

A language may be finite or infinite. Examples over $\Sigma=\{a,b,c\}$ include:

$$
L_1=\{a,aba,aca\},
$$

$$
L_2=\{a,aa,aaa,\ldots\}=\{a^n\mid n\geq1\},
$$

and

$$
L_3=\{\varepsilon\}\cup\{ab^na\mid n\geq0\}.
$$

Two boundary cases are especially important:

- $\emptyset$ is the empty language; it contains no strings.
- $\Sigma^*$ is the language of all strings over $\Sigma$.

### Describing a language

A finite language can be listed explicitly. An infinite language needs a rule,
property, grammar, regular expression, automaton, or another finite description.

For example, over $\Sigma=\{0,1\}$,

$$
E=\{w\in\Sigma^*\mid |w|\text{ is even}\}
$$

describes the infinite language of all even-length binary strings.

## 7. Operations on languages

Because languages are sets, they support the usual set operations. If
$A,B\subseteq\Sigma^*$, then:

| Operation | Meaning |
|---|---|
| $A\cup B$ | Strings in $A$ or $B$ (or both) |
| $A\cap B$ | Strings in both $A$ and $B$ |
| $A\setminus B$ | Strings in $A$ but not in $B$ |
| $\overline{A}=\Sigma^*\setminus A$ | Strings over $\Sigma$ that are not in $A$ |

The universe for a complement must be clear. Changing $\Sigma$ can change
$\overline{A}$.

### Language concatenation

The concatenation of languages $A$ and $B$ is

$$
AB=\{uv\mid u\in A\text{ and }v\in B\}.
$$

For example, if $A=\{a,ab\}$ and $B=\{b,ba\}$, then

$$
AB=\{ab,aba,abb,abba\}.
$$

In contrast,

$$
BA=\{ba,bab,baa,baab\},
$$

so $AB\neq BA$ in this example.

Language concatenation satisfies several useful laws:

$$
A\emptyset=\emptyset A=\emptyset,
$$

$$
A\{\varepsilon\}=\{\varepsilon\}A=A,
$$

$$
A(BC)=(AB)C,
$$

and

$$
A(B\cup C)=AB\cup AC,
\qquad
(B\cup C)A=BA\cup CA.
$$

:::{tip}
To compute a concatenation of finite languages, pair every string from the
first language with every string from the second, concatenate each pair, and
then remove duplicates because a language is a set.
:::

## 8. Common mistakes

Before moving on, make sure you can explain why each statement is incorrect:

1. “$\emptyset$ and $\{\varepsilon\}$ are the same language.”
2. “$|10101|=2$ because it contains two symbols.”
3. “$\Sigma^+$ contains $\varepsilon$.”
4. “String concatenation is commutative.”
5. “Every substring is a prefix.”
6. “A language must be finite because its alphabet is finite.”

:::{dropdown} Corrections
1. $\emptyset$ has no members; $\{\varepsilon\}$ has one member.
2. Length counts positions, so $|10101|=5$.
3. $\Sigma^+$ contains only nonempty strings.
4. In general, $uv\neq vu$.
5. A substring may occur strictly inside a string.
6. A finite alphabet can generate infinitely many finite strings.
:::

## 9. Session summary

The central hierarchy is:

$$
\text{symbols}\longrightarrow\text{strings}\longrightarrow\text{languages}.
$$

- An alphabet $\Sigma$ is a finite, nonempty set of symbols.
- A string is a finite sequence of symbols from $\Sigma$.
- $\varepsilon$ is the unique string of length zero.
- $\Sigma^*$ contains all finite strings over $\Sigma$; $\Sigma^+$ excludes
  $\varepsilon$.
- A language is any subset of $\Sigma^*$.
- Languages support set operations and concatenation.

Continue with the [Session 1 practice set](exercises.md). Work on each problem
before opening its hint.
