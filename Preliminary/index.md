---
# Copyright (c) 2026 Angela Villota, and collaborators from the CyED block
# Licensed under the PolyForm Noncommercial License 1.0.0.
# Commercial use is prohibited without prior written authorization.
title: Python preparation
---

# Python preparation

This short preparation path reviews the Python tools used throughout the
course. It is not a complete programming course. Its purpose is to help you
read an algorithm, test an example, and express a discrete structure in code.

## Learning outcomes

After completing this section, you should be able to:

- trace assignments, conditions, loops, and function calls;
- choose an appropriate collection for a sequence, set, or mapping;
- write small functions that process strings and finite collections;
- create and inspect NumPy arrays without confusing views and copies; and
- use boolean masks and matrix operations to solve small problems.

## Recommended route

1. Complete the readiness check below without running code.
2. Work through [Python foundations](./python-foundations.ipynb).
3. Complete its three practice problems before opening the hints.
4. Continue with [NumPy foundations](./numpy-foundations.ipynb).
5. Return here and confirm every item in the readiness checklist.

Allow approximately 90 minutes for Python foundations and 60 minutes for
NumPy foundations. Run the notebooks from top to bottom at least once, then
change the examples and predict the new output before running them again.

## Readiness check

Try to answer each question on paper first.

1. What value does `"ab" * 2 + "c"` produce?
2. How is a list different from a set?
3. What does `word[-1]` select?
4. Why should a function return a value instead of only printing it?
5. If `a` is a NumPy array, what is the difference between `a[1:4]` and
   `a[1:4].copy()`?

```{dropdown} Check your reasoning

1. The result is `"ababc"`.
2. A list preserves position and may contain duplicates; a set stores unique
   hashable elements and does not provide positional indexing.
3. It selects the final character.
4. A returned value can be tested, stored, and reused by another computation.
5. Basic slicing normally creates a view that shares data with `a`; `.copy()`
   creates an independent array.
```

## Why these topics matter

| Python tool | Course application |
| --- | --- |
| Strings and slicing | words, prefixes, suffixes, and alphabets |
| Sets | languages, active NFA states, and closure operations |
| Dictionaries | transition functions and symbol tables |
| Functions | reusable recognizers and simulations |
| NumPy arrays | tables, incidence matrices, and compact computations |
| Boolean masks | selecting states or data that satisfy a property |

## Readiness checklist

Before Week 1, verify that you can do each task without copying an example:

- [ ] define a function with parameters and a return value;
- [ ] iterate over the symbols of a string;
- [ ] remove duplicates using a set;
- [ ] count values using a dictionary;
- [ ] explain a list slice using inclusive and exclusive bounds;
- [ ] create a NumPy array and inspect its `shape`, `ndim`, and `dtype`;
- [ ] filter an array using a boolean condition; and
- [ ] distinguish element-wise multiplication from matrix multiplication.

If several items remain unchecked, repeat the corresponding notebook section
and explain one example aloud in your own words.
