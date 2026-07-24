---
title: "Session 3 Practice: Finite Automata"
---

<!--
Copyright (c) 2026 Angela Villota, and collaborators from the CyED block
Licensed under the PolyForm Noncommercial License 1.0.0.
Commercial use is prohibited without prior written authorization.
-->

# Session 3 practice

Trace machines with tables before drawing diagrams. For design problems, label
each state with what it remembers.

## Part A — DFA tracing and interpretation

### Exercise 1: Trace a parity DFA

Use the even-number-of-ones DFA from the study page. Trace and classify:

$$
\varepsilon,\quad 0,\quad 1,\quad 11,\quad 10101,\quad 111100.
$$

:::{dropdown} Hint
Begin every trace in $q_E$. A `0` preserves the state and a `1` toggles it.
:::

:::{dropdown} Partial solution
$\varepsilon$ is accepted because $q_E$ is both initial and accepting. The
trace for `10101` ends in $q_O$ after three toggles. Complete the others.
:::

### Exercise 2: Identify the language

Consider this DFA over $\{a,b\}$:

| State | `a` | `b` | Accepting? |
|---|---|---|---|
| $q_0$ | $q_1$ | $q_0$ | No |
| $q_1$ | $q_1$ | $q_2$ | No |
| $q_2$ | $q_1$ | $q_0$ | Yes |

Describe $L(M)$ in words and give a regular expression for it.

:::{dropdown} Hint
Ask what must be true about the final two symbols when the machine ends in
$q_2$.
:::

:::{dropdown} Partial solution
$q_1$ remembers that the current string ends in `a`; moving from $q_1$ to the
only accepting state requires `b`. Test `ab`, `aab`, `aba`, and `abb` before
writing the expression.
:::

### Exercise 3: Extended transitions

For the DFA in Exercise 2, calculate:

$$
\delta^*(q_0,baab),\qquad
\delta^*(q_1,baba),\qquad
\delta^*(q_2,\varepsilon).
$$

Then state whether `baab` belongs to $L(M)$.

:::{dropdown} Hint
The base rule is $\delta^*(q,\varepsilon)=q$.
:::

## Part B — DFA design

### Exercise 4: End with `00`

Design a DFA over $\{0,1\}$ that accepts exactly the strings ending in `00`.
Provide a state meaning, transition table, initial state, and accepting states.

:::{dropdown} Hint
Track the longest useful suffix: no trailing zero, one trailing zero, or at
least two trailing zeros.
:::

### Exercise 5: Divisible by three

Design a DFA accepting strings whose number of `a` symbols is divisible by
three. Symbols `b` do not affect the count.

:::{dropdown} Partial solution
Use three states for remainders $0$, $1$, and $2$ modulo three. Input `a` moves
to the next remainder; input `b` loops. The remainder-$0$ state is initial and
accepting.
:::

### Exercise 6: Contains `101`

Design a DFA over $\{0,1\}$ that accepts strings containing `101` as a
substring.

:::{dropdown} Hint
Create states for having matched no useful prefix, `1`, `10`, and the complete
substring. Once the complete substring has appeared, acceptance cannot be lost.
:::

### Exercise 7: Product-state challenge

Design a DFA for strings with an even number of `0` symbols and an odd number
of `1` symbols.

:::{dropdown} Hint
Combine two parity memories. There are four combinations: even/even,
even/odd, odd/even, and odd/odd.
:::

## Part C — NFA simulation

### Exercise 8: Active states

Use the NFA from the worked example:

| State | `a` | `b` | Accepting? |
|---|---|---|---|
| $q_0$ | $\{q_0,q_1\}$ | $\{q_0\}$ | No |
| $q_1$ | $\emptyset$ | $\{q_2\}$ | No |
| $q_2$ | $\emptyset$ | $\emptyset$ | Yes |

Trace the active-state set for `ab`, `abb`, `bab`, and `baba`.

:::{dropdown} Partial solution
For `ab`,

$$
\{q_0\}\xrightarrow{a}\{q_0,q_1\}
\xrightarrow{b}\{q_0,q_2\},
$$

so it is accepted. Continue the other traces one symbol at a time. A branch in
$q_2$ may stop while another branch continues from $q_0$.
:::

### Exercise 9: NFA acceptance logic

Explain why each statement is false:

1. An NFA accepts only if every computation ends in an accepting state.
2. An NFA rejects if one computation has no next transition.
3. Nondeterminism means the implementation should choose transitions randomly.

:::{dropdown} Partial solution
NFA acceptance is existential: one complete accepting path is sufficient. A
set-of-states implementation explores all possible destinations simultaneously.
:::

## Part D — Subset construction

### Exercise 10

Convert the NFA in Exercise 8 to a DFA.

1. Start with $\{q_0\}$.
2. Compute transitions on `a` and `b`.
3. Continue until no new reachable subsets appear.
4. Mark every subset containing $q_2$ as accepting.

:::{dropdown} Partial construction
The first transitions are

$$
\{q_0\}\xrightarrow{a}\{q_0,q_1\},
\qquad
\{q_0\}\xrightarrow{b}\{q_0\}.
$$

From $\{q_0,q_1\}$ on `b`, the union of destinations is
$\{q_0\}\cup\{q_2\}$. Add that subset as a DFA state and continue.
:::

## Part E — $\varepsilon$-closure

### Exercise 11

An $\varepsilon$-NFA has transitions

$$
q_0\xrightarrow{\varepsilon}q_1,
\quad
q_1\xrightarrow{\varepsilon}q_2,
\quad
q_2\xrightarrow{\varepsilon}q_1,
\quad
q_2\xrightarrow{a}q_3.
$$

Compute $\operatorname{EClosure}(q_0)$,
$\operatorname{EClosure}(q_2)$, and the set active after reading `a` from the
initial state.

:::{dropdown} Partial solution
The cycle does not cause an infinite set. Record visited states:

$$
\operatorname{EClosure}(q_0)=\{q_0,q_1,q_2\}.
$$

Move on `a` from every member of that set, then take another closure.
:::

### Exercise 12: Explain equivalence

In your own words, explain why allowing multiple transitions or
$\varepsilon$-transitions does not let an automaton recognize nonregular
languages.

:::{dropdown} Hint
Refer to subset construction and $\varepsilon$-closure. The explanation should
describe a conversion, not merely state that a theorem exists.
:::

## Readiness checklist

- [ ] I can trace a DFA without skipping input symbols.
- [ ] I can describe what each state remembers.
- [ ] I know that DFA acceptance is checked after all input is consumed.
- [ ] I can simulate an NFA with a set of active states.
- [ ] I can perform subset construction.
- [ ] I can calculate an $\varepsilon$-closure without looping.
- [ ] I can compare DFA, NFA, and $\varepsilon$-NFA acceptance.

Return to the [Session 3 study page](index.md) for any unchecked topic.
