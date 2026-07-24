---
title: "Session 2: Regular Languages and Regular Expressions"
---

<!--
Copyright (c) 2026 Angela Villota, and collaborators from the CyED block
Licensed under the PolyForm Noncommercial License 1.0.0.
Commercial use is prohibited without prior written authorization.
-->

# Regular languages and regular expressions

Session 1 introduced languages as sets of strings. We now develop operations
that build new languages and use **regular expressions** to describe regular
languages compactly. The last part of the session connects the mathematical
notation with Python's `re` module.

## Learning objectives

After this session, you should be able to:

- compute powers, closures, and reversals of languages;
- explain the inductive definition of a regular language;
- translate between a language description and a regular expression;
- distinguish formal regular expressions from Python regex syntax;
- select appropriate Python matching operations; and
- test a pattern with positive, negative, and boundary cases.

## 1. Powers of a language

Let $A\subseteq\Sigma^*$. Its powers are defined by repeated language
concatenation:

$$
A^0=\{\varepsilon\},
\qquad
A^{n+1}=A^nA.
$$

For $n\geq1$,

$$
A^n=\{u_1u_2\cdots u_n\mid u_i\in A\text{ for }1\leq i\leq n\}.
$$

If $A=\{a,bc\}$, then

$$
A^2=\{aa,abc,bca,bcbc\}.
$$

The exponent counts how many strings from $A$ are concatenated. It does not
usually equal the length of the resulting strings because members of $A$ may
have different lengths.

## 2. Kleene and positive closure

The **Kleene closure** of $A$ contains all finite concatenations of members of
$A$, including a concatenation of zero members:

$$
A^*=\bigcup_{n=0}^{\infty}A^n.
$$

The **positive closure** uses one or more members of $A$:

$$
A^+=\bigcup_{n=1}^{\infty}A^n.
$$

Therefore,

$$
A^+=AA^*=A^*A,
\qquad
A^*=A^0\cup A^+=\{\varepsilon\}\cup A^+.
$$

:::{important}
$A^+$ means “one or more strings chosen from $A$.” If $\varepsilon\in A$, then
$\varepsilon\in A^+$. The common shortcut $A^+=A^*\setminus\{\varepsilon\}$ is
valid only when $\varepsilon\notin A$.
:::

Useful closure identities include:

$$
A^*A^*=A^*,
\qquad
(A^*)^*=A^*,
\qquad
(A^+)^*=A^*,
\qquad
(A^+)^+=A^+.
$$

### Example

For $A=\{ab\}$,

$$
A^*=\{\varepsilon,ab,abab,ababab,\ldots\}
$$

and

$$
A^+=\{ab,abab,ababab,\ldots\}.
$$

For $B=\{\varepsilon,a\}$, however, both $B^*$ and $B^+$ contain
$\varepsilon$.

## 3. Reversal of a language

The reversal of $A$ is obtained by reversing each of its strings:

$$
A^R=\{u^R\mid u\in A\}.
$$

If $A=\{ab,bba,\varepsilon\}$, then

$$
A^R=\{ba,abb,\varepsilon\}.
$$

Reversal satisfies:

$$
(AB)^R=B^RA^R,
$$

$$
(A\cup B)^R=A^R\cup B^R,
\qquad
(A\cap B)^R=A^R\cap B^R,
$$

$$
(A^R)^R=A,
\qquad
(A^*)^R=(A^R)^*,
\qquad
(A^+)^R=(A^R)^+.
$$

## 4. Regular languages

A language over $\Sigma$ is **regular** if it can be constructed using the
following rules:

1. The basic languages $\emptyset$, $\{\varepsilon\}$, and $\{a\}$ for each
   $a\in\Sigma$ are regular.
2. If $A$ and $B$ are regular, then $A\cup B$, $AB$, and $A^*$ are regular.
3. Nothing else is regular unless it follows from finitely many applications
   of these rules.

The three constructors—union, concatenation, and Kleene star—are called
**regular operations**.

### Building descriptions from structure

Let $\Sigma=\{a,b\}$. Then:

| Language | Construction |
|---|---|
| Strings containing exactly one $a$ | $\{b\}^*\{a\}\{b\}^*$ |
| Strings beginning with $b$ | $\{b\}\Sigma^*$ |
| Strings containing $ba$ | $\Sigma^*\{ba\}\Sigma^*$ |
| Strings ending with $a$ | $\Sigma^*\{a\}$ |

The construction mirrors the verbal description. “Contains $ba$,” for
example, becomes “anything, followed by $ba$, followed by anything.”

## 5. Formal regular expressions

A **regular expression** is syntax that denotes a regular language. We write
$L(R)$ for the language represented by expression $R$.

The basic expressions are:

| Expression | Language represented |
|---|---|
| $\emptyset$ | $\emptyset$ |
| $\varepsilon$ | $\{\varepsilon\}$ |
| $a$, for $a\in\Sigma$ | $\{a\}$ |

If $R$ and $S$ are regular expressions, then:

| Expression | Meaning |
|---|---|
| $R\mid S$ | Union: $L(R)\cup L(S)$ |
| $RS$ | Concatenation: $L(R)L(S)$ |
| $R^*$ | Kleene closure: $L(R)^*$ |

Parentheses group subexpressions. We use `|` for union because it matches the
notation used by most programming regex engines.

### Example: start with 2 and end with 1

Over $\Sigma=\{0,1,2\}$, the expression

$$
2(0\mid1\mid2)^*1
$$

denotes all strings that start with $2$ and end with $1$. The shortest member
is $21$.

### Example: contain consecutive ones

Over $\Sigma=\{0,1\}$,

$$
(0\mid1)^*11(0\mid1)^*
$$

denotes all strings containing `11` as a substring.

### Operator precedence

Unless parentheses say otherwise, use this order:

1. closure (`*`, `+`),
2. concatenation,
3. union (`|`).

Thus $ab^*\mid c$ means $(a(b^*))\mid c$, not $(ab)^*\mid c$.

## 6. From a description to an expression

A reliable design process is:

1. Fix the alphabet.
2. Identify mandatory prefixes, suffixes, or substrings.
3. Split the string into meaningful regions.
4. Describe the choices in each region.
5. Check the shortest valid strings.
6. Try to find a counterexample that the expression accepts incorrectly.

### Example: at most two zeros

Over $\Sigma=\{0,1\}$, strings with at most two zeros can be divided into
blocks of ones separated by optional zeros:

$$
1^*(\varepsilon\mid01^*)(\varepsilon\mid01^*).
$$

This construction allows zero, one, or two occurrences of `0`.

### Example: length at most five

Let $S=(0\mid1)$. Then

$$
\varepsilon\mid S\mid S^2\mid S^3\mid S^4\mid S^5
$$

represents all binary strings of length at most five.

## 7. Formal expressions versus Python regex

Formal-language notation and Python regex share the same central ideas, but
their syntax is not identical.

| Idea | Formal expression | Python `re` |
|---|---|---|
| Union | $a\mid b$ | `a|b` |
| Concatenation | $ab$ | `ab` |
| Zero or more | $a^*$ | `a*` |
| One or more | $aa^*$ or $a^+$ | `a+` |
| Optional | $\varepsilon\mid a$ | `a?` |
| Any symbol in a set | $0\mid1\mid2$ | `[012]` |
| Empty string | $\varepsilon$ | represented structurally, not by `ε` |

Python also provides conveniences that go beyond the minimal formal notation:

- character classes such as `[a-z]` and `[0-9]`;
- predefined classes such as `\d`, `\w`, and `\s`;
- counted repetitions such as `{3}` and `{2,5}`;
- anchors such as `^` and `$`;
- word boundaries such as `\b`;
- capturing groups, named groups, and backreferences.

:::{warning}
A Python regex is not automatically a validator. `re.search(r"\d+", text)`
asks whether a digit sequence occurs anywhere. To require the entire string to
follow a pattern, prefer `re.fullmatch`.
:::

## 8. Python matching operations

The `re` module provides several operations with different purposes:

| Operation | Use it when you need to… |
|---|---|
| `re.fullmatch` | validate the entire string |
| `re.match` | match only at the beginning |
| `re.search` | find the first match anywhere |
| `re.findall` | collect all non-overlapping matches |
| `re.finditer` | inspect every match and its position |
| `re.sub` | replace matches |
| `re.split` | split around matches |

Use raw strings for patterns: write `r"\bcat\b"`, not `"\bcat\b"`. Raw
strings keep Python string escaping from interfering with regex backslashes.

### Character classes and quantifiers

| Pattern | Meaning |
|---|---|
| `.` | Any character except newline by default |
| `[abc]` | One of `a`, `b`, or `c` |
| `[^0-9]` | One character that is not a digit |
| `\d` / `\D` | Digit / non-digit |
| `\w` / `\W` | Word / non-word character |
| `\s` / `\S` | Whitespace / non-whitespace |
| `?` | Zero or one repetition |
| `*` | Zero or more repetitions |
| `+` | One or more repetitions |
| `{n,m}` | Between $n$ and $m$ repetitions |

### Boundaries

`^` and `$` refer to line boundaries, with behavior affected by flags such as
`re.MULTILINE`. `\A` and `\Z` refer to the start and end of the entire input.
`\b` matches a transition between a word and non-word character; it does not
consume a character.

## 9. Testing a regular expression

Every pattern should be tested using at least three categories:

- **Positive cases:** ordinary strings that should match.
- **Negative cases:** strings that violate one requirement.
- **Boundary cases:** empty input, minimum or maximum lengths, adjacent
  punctuation, and repeated delimiters.

For a telephone pattern, do not test only one valid number. Include missing
digits, extra separators, leading spaces, and text after the number.

## 10. Common mistakes

1. Using `search` when the whole input must be validated.
2. Forgetting that `.` has a special meaning.
3. Writing `[cat]` when the intended alternative is `cat`.
4. Applying a quantifier only to the preceding symbol instead of a group.
5. Forgetting raw strings for patterns containing backslashes.
6. Designing only from valid examples and never testing near misses.
7. Assuming `\d` means only ASCII digits; in Python it is Unicode-aware unless
   restricted explicitly.

## Session summary

- Language powers use repeated concatenation.
- $A^*$ allows zero or more factors from $A$; $A^+$ allows one or more.
- Regular languages are generated from basic languages using union,
  concatenation, and Kleene star.
- Formal regular expressions describe languages; Python regexes implement a
  richer pattern language for text processing.
- The choice among `fullmatch`, `match`, `search`, `findall`, and `finditer` is
  part of solving the problem.

Continue with the [Session 2 practice set](exercises.md), then work through the
[executable Python notebook](regex-python.ipynb).
