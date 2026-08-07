---
title: "Session 4: Summary and Concepts (Regex)"
---

<!--
Copyright (c) 2026 Angela Villota, and collaborators from the CyED block
Licensed under the PolyForm Noncommercial License 1.0.0.
Commercial use is prohibited without prior written authorization.
-->

# Regex in Python, in depth — summary and concepts

This page consolidates the worked examples from the
[executable notebook](regex-python-summary.ipynb) into a single reference,
with every piece of code shown together with the output it produces. It
expands the character-class, grouping, and backreference material from
[S2 · Regex in Python](../../Week1/S2/regex-python.ipynb) into a deeper,
step-by-step walkthrough. Each construct is built up one small piece at a
time; every concept ends with an exercise and its collapsed solution.

## 1. Character classes `[...]`, step by step

A character class matches **exactly one character** taken from the set (or
range) written inside the brackets. It never matters how many characters are
listed — `[...]` always consumes one character per match attempt, never
more. To match several characters you still need a quantifier such as `+`
or `{m,n}` *after* the class.

### 1.1 A literal set of characters

```python
vowels = r"[aeiou]"
word = "regular expressions"

print(re.findall(vowels, word))
# ['e', 'u', 'a', 'e', 'e', 'i', 'o']
```

Each bracket pair matches one character; `findall` walks the whole string
and reports every position where that one-character class succeeds.

### 1.2 Ranges

Inside `[]`, `a-z` is a **range**: every character whose code point lies
between `a` and `z`. A range only forms when `-` sits *between* two
characters; at the start or end of the class, `-` is a literal dash.

```python
values = ["Cat42", "007", "he-llo", "UPPER-CASE"]

for v in values:
    lowers = re.findall(r"[a-z]", v)
    digits = re.findall(r"[0-9]", v)
    with_dash = re.findall(r"[a-z-]", v)  # '-' at the end stays literal
    print(f"{v!r:15} lowers={lowers!s:22} digits={digits!s:14} with_dash={with_dash}")

# 'Cat42'         lowers=['a', 't']             digits=['4', '2']     with_dash=['a', 't']
# '007'           lowers=[]                     digits=['0', '0', '7'] with_dash=[]
# 'he-llo'        lowers=['h', 'e', 'l', 'l', 'o'] digits=[]             with_dash=['h', 'e', '-', 'l', 'l', 'o']
# 'UPPER-CASE'    lowers=[]                     digits=[]             with_dash=['-']
```

### 1.3 Negation with `^`

`^` immediately after the opening `[` negates the class: it matches one
character that is **not** in the set. Anywhere else inside the brackets,
`^` is a literal caret.

```python
text = "room 12b, floor 3!"

not_digit = re.findall(r"[^0-9]", text)[:10]
literal_caret = re.findall(r"[0-9^]", "3^2 = 9")

print("first 10 non-digits:", not_digit)
print("digits or literal caret:", literal_caret)

# first 10 non-digits: ['r', 'o', 'o', 'm', ' ', 'b', ',', ' ', 'f', 'l']
# digits or literal caret: ['3', '^', '2', '9']
```

### 1.4 What loses its special meaning inside `[]`

Most regex metacharacters (`.`, `*`, `+`, `?`, `(`, `)`, `|`) are **literal**
inside a character class — you do not need to escape them there. Only `]`,
`\`, `^` (in first position), and `-` (when it would form a range) keep a
special meaning.

```python
price_text = "Prices: $5.00, $12.50, (discount)."

dots_and_parens = re.findall(r"[.()]", price_text)
digit_runs = re.findall(r"[0-9.]+", price_text)

print("literal . ( ) matches:", dots_and_parens)
print("digit-or-dot runs:    ", digit_runs)

# literal . ( ) matches: ['.', '.', '(', ')', '.']
# digit-or-dot runs:     ['5.00', '12.50', '.']
```

### Exercise 1 — build your own character class

Write **one** character class for a single hexadecimal digit (`0`-`9`,
`a`-`f`, `A`-`F`), then combine it with `+` so `hex_run_pattern` pulls out
every run of one-or-more hexadecimal characters from `text` below.

```python
text = "IDs: 1A2B3C, XYZW, ff00, GG42, mix99"

hex_run_pattern = r"(?!)"  # TODO: one character class + a quantifier

print(re.findall(hex_run_pattern, text))
```

:::{dropdown} Solution
```python
hex_run_pattern = r"[0-9a-fA-F]+"
print(re.findall(hex_run_pattern, text))
# ['D', '1A2B3C', 'ff00', '42', '99']
```

`[0-9a-fA-F]` matches one hex digit; `+` repeats it. The class does not know
about word boundaries, so it also finds hex-looking fragments *inside*
ordinary tokens: the `D` in `IDs` is captured on its own because `I` and `S`
are not hex digits, and `GG42` only contributes `42` because `G` is outside
`a`-`f`/`A`-`F`. This is a common regex gotcha — if you only want whole hex
codes, anchor the class with `\b` or require a fixed length, e.g.
`r"\b[0-9a-fA-F]{6}\b"`.
:::

## 2. What is a capturing group?

Parentheses `(...)` do two things at once:

1. **Grouping** — treat everything inside as one unit, so a following
   quantifier (`*`, `+`, `?`, `{m,n}`) or the `|` alternation applies to the
   whole unit, not just the character right before it.
2. **Capturing** — by default, remember exactly which text matched inside
   the parentheses, so it can be retrieved afterward with `.group(n)` or
   reused later in the *same* pattern with `\n`.

Every `(...)` that is not marked non-capturing gets its own number, assigned
in the order its **opening** parenthesis appears, left to right, starting at
1. Group `0` always means "the whole match," and it always exists — even in
a pattern with no parentheses at all.

```python
pattern = re.compile(r"(\d{3})-(\d{4})")
m = pattern.fullmatch("555-1234")

print("group(0):", m.group(0))
print("group(1):", m.group(1))
print("group(2):", m.group(2))
print("groups():", m.groups())

# group(0): 555-1234
# group(1): 555
# group(2): 1234
# groups(): ('555', '1234')
```

`match.group(0)` (equivalently `match.group()`) is always the entire match.
`group(1)` is the text captured by the first `(...)`, `group(2)` by the
second, and so on.

### 2.1 Numbering follows the opening parenthesis, even when groups nest

```python
pattern = re.compile(r"((\d{2})(\d{2}))")
m = pattern.fullmatch("1234")

print("group(1) — outer, ( opens first:", m.group(1))
print("group(2) — first inner, ( opens second:", m.group(2))
print("group(3) — second inner, ( opens third:", m.group(3))
print("groups():", m.groups())

# group(1) — outer, ( opens first: 1234
# group(2) — first inner, ( opens second: 12
# group(3) — second inner, ( opens third: 34
# groups(): ('1234', '12', '34')
```

Group 1 is the *outer* parenthesis because its `(` appears first in the
pattern, even though it closes last. Numbering always tracks the position of
the opening `(`, never the closing `)` and never the nesting depth.

### 2.2 `findall` with groups — a trap worth knowing

- No groups in the pattern → `findall` returns the whole matched text.
- Exactly one group → `findall` returns only that group's text.
- Two or more groups → `findall` returns a tuple per match, one entry per
  group. The whole match is **not** included unless you wrap it in its own
  group too.

```python
text = "2026-08-06 and 2025-01-15"

no_group = re.findall(r"\d{4}-\d{2}-\d{2}", text)
one_group = re.findall(r"\d{4}-(\d{2})-\d{2}", text)
two_groups = re.findall(r"(\d{4})-(\d{2})-(\d{2})", text)

print("no group:  ", no_group)
print("one group: ", one_group)
print("two groups:", two_groups)

# no group:   ['2026-08-06', '2025-01-15']
# one group:  ['08', '01']
# two groups: [('2026', '08', '06'), ('2025', '01', '15')]
```

If you need both the whole match and the individual parts, use `finditer`
instead: it always yields a `Match` object exposing `.group(0)` plus every
subgroup, regardless of how many groups the pattern has.

### 2.3 Named groups `(?P<name>...)`

A named group still gets a number (in the same left-to-right order), but can
also be retrieved by name — which keeps code readable when a pattern has
several groups.

```python
pattern = re.compile(r"(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})")
m = pattern.fullmatch("2026-08-06")

print("by number:", m.group(1), m.group(2), m.group(3))
print("by name:  ", m.group("year"), m.group("month"), m.group("day"))
print("groupdict:", m.groupdict())

# by number: 2026 08 06
# by name:   2026 08 06
# groupdict: {'year': '2026', 'month': '08', 'day': '06'}
```

### Exercise 2 — numbering, `findall`, and named groups

`text` below contains several `HH:MM:SS` durations. Using
`duration_pattern`, defined with three **named** groups `h`, `m`, and `s`:

1. call `duration_pattern.findall(text)` and predict its shape *before*
   running the cell;
2. then use `duration_pattern.finditer(text)` to print each full match
   together with its `groupdict()`.

```python
text = "Splits: 01:02:03, 00:59:59"

duration_pattern = re.compile(r"(?!)")  # TODO: three named groups h, m, s

print("findall:", duration_pattern.findall(text))

for match in duration_pattern.finditer(text):
    print(match.group(0), "->", match.groupdict())
```

:::{dropdown} Solution
```python
duration_pattern = re.compile(r"(?P<h>\d{2}):(?P<m>\d{2}):(?P<s>\d{2})")

print("findall:", duration_pattern.findall(text))
# findall: [('01', '02', '03'), ('00', '59', '59')]
# Three groups -> each match becomes a 3-tuple; the names do not change
# findall's shape, only how you can later look values up.

for match in duration_pattern.finditer(text):
    print(match.group(0), "->", match.groupdict())
# 01:02:03 -> {'h': '01', 'm': '02', 's': '03'}
# 00:59:59 -> {'h': '00', 'm': '59', 's': '59'}
```

`findall` still returns plain tuples, in group order, even though the
groups are named — names only add a lookup key on the `Match` object
itself, available through `finditer` (or `search`/`fullmatch`), never
through `findall`.
:::

## 3. How `()` works in detail

Parentheses change **scope**: they decide what a quantifier or an
alternation applies to. Compare a pattern with no grouping to the same
pattern with parentheses added.

```python
text = "hahaha"

no_group = re.fullmatch(r"ha+", text)
grouped = re.fullmatch(r"(?:ha)+", text)

print("ha+       ->", no_group)
print("(?:ha)+   ->", grouped)

# ha+       -> None
# (?:ha)+   -> <re.Match object; span=(0, 6), match='hahaha'>
```

`ha+` means "an `h`, then one-or-more `a`'s" — the `+` binds only to the
character immediately before it, `a`. It does **not** match `"hahaha"`,
which needs repeats of the two-character block `ha`. Wrapping the block in
parentheses, `(?:ha)+`, makes the `+` apply to the *whole* group instead.

### 3.1 Non-capturing groups `(?:...)`

The example above used `(?:...)` on purpose: it groups **without**
capturing. Use it whenever you need parentheses only for scope — repeating
or alternating several symbols together — and do not need that text back
afterward.

```python
with_capture = re.compile(r"(ab)+")
without_capture = re.compile(r"(?:ab)+")

text = "ababab xyz"

m1 = with_capture.search(text)
m2 = without_capture.search(text)

print("capturing    -> group(0):", m1.group(0), "groups():", m1.groups())
print("noncapturing -> group(0):", m2.group(0), "groups():", m2.groups())

# capturing    -> group(0): ababab groups(): ('ab',)
# noncapturing -> group(0): ababab groups(): ()
```

Both patterns match exactly the same text — `(?:...)` changes only what
gets *recorded*, never what gets *matched*. This matters most once a
pattern mixes grouping-only parentheses with parentheses you do want to
capture, because every capturing `(` shifts the numbers of every group that
follows it.

```python
messy = re.compile(r"(Monday|Tuesday|Wednesday) at (\d{1,2}):(\d{2})")
clean = re.compile(r"(?:Monday|Tuesday|Wednesday) at (\d{1,2}):(\d{2})")

text = "Tuesday at 9:15"

print("messy.groups():", messy.fullmatch(text).groups())  # day, hour, minute
print("clean.groups():", clean.fullmatch(text).groups())  # hour, minute only

# messy.groups(): ('Tuesday', '9', '15')
# clean.groups(): ('9', '15')
```

The weekday alternation is the same in both patterns; making it
non-capturing in `clean` removes a group nobody needed, so `group(1)` and
`group(2)` land directly on the hour and the minute.

### Exercise 3 — grouping for scope, not for capture

A "laugh" string is valid when it is made of two-or-more repetitions of the
literal block `ha` and *nothing else* — `"haha"` and `"hahaha"` are valid,
`"hahah"` and `"ha"` are not. You do not need the repeated text back, so use
a **non-capturing** group.

```python
laugh_cases = [
    ("haha", True),
    ("hahaha", True),
    ("hahah", False),
    ("ha", False),
    ("hahahaha", True),
]

laugh_pattern = r"(?!)"  # TODO: non-capturing group + quantifier

for value, expected in laugh_cases:
    actual = re.fullmatch(laugh_pattern, value) is not None
    print(f"{value!r:10} expected={expected} actual={actual}")
```

:::{dropdown} Solution
```python
laugh_pattern = r"(?:ha){2,}"

for value, expected in laugh_cases:
    actual = re.fullmatch(laugh_pattern, value) is not None
    assert actual == expected
    print(f"{value!r:10} expected={expected} actual={actual}")

# 'haha'     expected=True actual=True
# 'hahaha'   expected=True actual=True
# 'hahah'    expected=False actual=False
# 'ha'       expected=False actual=False
# 'hahahaha' expected=True actual=True
```

`(?:ha)` groups the two-character block so `{2,}` can repeat the *block*
two or more times. `"hahah"` fails because after two full `ha` blocks
(`"haha"`) a lone `h` is left over, and `fullmatch` requires the entire
string to be consumed. `(?:...)` is enough here — nothing about this
pattern needs the matched text captured back out.
:::

## 4. Backreferences: `\1`, and does `\2`, `\3`, ... exist?

A backreference is written `\N` inside the **pattern itself** (never inside
a `sub` replacement string, which uses a different syntax). It means:
"match, right here, the exact same text that group `N` already captured
earlier in this same match attempt." A backreference does not re-run the
group's sub-pattern — it demands the literal characters that were captured,
verbatim.

**Yes, `\2`, `\3`, ... exist.** Every capturing group gets its own
backreference: `\1` for group 1, `\2` for group 2, `\3` for group 3, and so
on — there is no limit at `\1`. Python's `re` module even accepts two-digit
references (`\10`, `\11`, ...) once that many groups actually exist in the
pattern; referencing a group number that does **not** exist is a
compile-time error, not a silent failure to match.

### 4.1 One backreference: reusing a captured separator

```python
phone_pattern = re.compile(r"\d{3}([ -]?)\d{3}\1\d{3}")

phone_cases = ["555555555", "555-555-555", "555 555 555", "555-555 555"]

for value in phone_cases:
    print(f"{value!r:15} ->", bool(phone_pattern.fullmatch(value)))

# '555555555'     -> True
# '555-555-555'   -> True
# '555 555 555'   -> True
# '555-555 555'   -> False
```

Group 1 captures whatever separator (or nothing) appears the first time;
`\1` then forces the *second* separator to be identical to the first —
`"555-555 555"` fails because `-` and a space do not match.

### 4.2 Two independent backreferences, `\1` and `\2`

```python
double_pattern = re.compile(r"(\w+) \1 (\w+) \2")

double_cases = [
    "echo echo test test",
    "echo echo test other",
    "echo other test test",
]

for value in double_cases:
    print(f"{value!r:22} ->", bool(double_pattern.fullmatch(value)))

# 'echo echo test test'  -> True
# 'echo echo test other' -> False
# 'echo other test test' -> False
```

`\1` and `\2` are independent: each refers to *its own* group, and each must
match separately. The first case succeeds because both repeated pairs
match; the other two fail one of the two independent checks.

### 4.3 Three backreferences, `\1`, `\2`, `\3` — closing tags in reverse order

```python
tag_pattern = re.compile(r"<(\w+)><(\w+)><(\w+)>.*?</\3></\2></\1>")

tag_cases = [
    "<div><section><p>text</p></section></div>",
    "<div><section><p>text</p></div></section>",
]

for value in tag_cases:
    print(f"{value!r:55} ->", bool(tag_pattern.fullmatch(value)))

# '<div><section><p>text</p></section></div>'             -> True
# '<div><section><p>text</p></div></section>'             -> False
```

The three opening tags are captured by groups 1, 2, and 3, in that order.
The closing tags must appear in **reverse** order — `\3` (innermost) first,
then `\2`, then `\1` (outermost) — which is exactly how properly nested tags
close. Swapping `</div>` and `</section>` in the second case breaks the
reversed order and the match fails.

### 4.4 Named backreferences: `(?P=name)`

A named group can be referenced by name instead of by number, using
`(?P=name)` — it behaves exactly like `\N` for that group's number, just
more readably.

```python
named_pattern = re.compile(r"(?P<word>\w+) (?P=word)")

print(named_pattern.fullmatch("echo echo"))
print(named_pattern.fullmatch("echo bravo"))

# <re.Match object; span=(0, 9), match='echo echo'>
# None
```

### Exercise 4 — find every doubled word

Sentences sometimes contain an accidental doubled word, such as "is is" or
"the the." Build `dup_pattern` with **one** capturing group and a
backreference so that `dup_pattern.finditer(sentence)` reports every
doubled word (case-insensitively) together with the word itself.

```python
sentence = "This is is a test test of the the detector."

dup_pattern = re.compile(r"(?!)")  # TODO: one group + \1, case-insensitive

for match in dup_pattern.finditer(sentence):
    print(match.group(0), "->", match.group(1))
```

:::{dropdown} Solution
```python
dup_pattern = re.compile(r"\b(\w+)\b\s+\b\1\b", re.IGNORECASE)

for match in dup_pattern.finditer(sentence):
    print(match.group(0), "->", match.group(1))

# is is -> is
# test test -> test
# the the -> the
```

`(\w+)` captures one word; `\1` demands the identical text again after some
whitespace. `re.IGNORECASE` makes the *comparison* case-insensitive, but
`\1` still requires the exact characters captured — it does not "restart"
the `\w+` sub-pattern, it just checks the literal text again.
:::

## 5. Putting it together: classes + groups + backreferences

**Design an ID validator.** A valid ID is three blocks of four hexadecimal
characters, separated by either `-` or `:`, where **both** separators in a
single ID must be the same character (`"1A2B-3C4D-5E6F"` and
`"1a2b:3c4d:5e6f"` are valid; `"1A2B-3C4D:5E6F"`, which mixes separators, is
not).

```python
id_cases = [
    ("1A2B-3C4D-5E6F", True),
    ("1a2b:3c4d:5e6f", True),
    ("1A2B-3C4D:5E6F", False),
    ("1A2B_3C4D_5E6F", False),
    ("1A2B-3C4D-5E6G", False),
]

id_pattern = r"(?!)"  # TODO: hex class + capturing separator + backreference

for value, expected in id_cases:
    actual = re.fullmatch(id_pattern, value) is not None
    print(f"{value!r:16} expected={expected} actual={actual}")
```

:::{dropdown} Solution
```python
id_pattern = r"[0-9A-Fa-f]{4}([-:])[0-9A-Fa-f]{4}\1[0-9A-Fa-f]{4}"

for value, expected in id_cases:
    actual = re.fullmatch(id_pattern, value) is not None
    assert actual == expected
    print(f"{value!r:16} expected={expected} actual={actual}")

# '1A2B-3C4D-5E6F' expected=True actual=True
# '1a2b:3c4d:5e6f' expected=True actual=True
# '1A2B-3C4D:5E6F' expected=False actual=False
# '1A2B_3C4D_5E6F' expected=False actual=False
# '1A2B-3C4D-5E6G' expected=False actual=False
```

`[0-9A-Fa-f]{4}` is the character-class concept from Section 1, repeated
three times. `([-:])` is a capturing group (Section 2) around a *class* of
two literal separators, so `\1` (Section 4) can force the second separator
to match the first exactly. `"1A2B_3C4D_5E6F"` fails at the character-class
level (`_` is outside `[-:]`); `"1A2B-3C4D:5E6F"` fails at the
backreference (`:` ≠ `-`); `"1A2B-3C4D-5E6G"` fails because `G` is outside
the hex range.
:::

## Reflection

Before moving on, answer these questions for yourself:

1. What is the difference between what `(...)` and `(?:...)` *match*, and
   what they *record*?
2. Why does group numbering depend on the position of `(`, not `)`, and not
   nesting depth?
3. Give an example where `\2` is genuinely necessary — one backreference
   would not be enough.
4. When does `findall` silently change shape on you, and how would you
   notice before it causes a bug?

## References

Python Software Foundation. *re — Regular expression operations.* Python
3 documentation.

---

Continue with the [Week 2 overview](../index.md), try the ideas here
interactively in the [executable notebook](regex-python-summary.ipynb),
revisit [S2 · Regex in Python](../../Week1/S2/regex-python.ipynb) for the
operations recap, or move on to [S3 · Finite automata](../S3/index.md).
