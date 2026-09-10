---
name: index-notations
description: >-
  Sweep a LaTeX paper and mark every global notation and concept with the
  project's index-marking macros (typically \mdef inside a formula and \tdef
  outside one), so that the index covers exactly the notations and concepts the
  paper defines. Use when asked to go over the notations of a paper, to wrap
  notations in \mdef/\tdef, to build or complete an index of notation, or to
  check that the marking is complete and correct.
---

# Marking a paper's notations and concepts for the index

The deliverable is a `main.tex` in which every *global* notation and concept is
wrapped in the project's marking macro at the place where it is **defined**, and
nowhere else, and which compiles to a PDF identical to the previous one apart
from the colouring of the marked text and the enlarged index.

## 1. Find the macros before touching the paper

Do not assume the names. Grep the class/style files (`*.sty`, the preamble) for
the marking commands and read their definitions. The usual pair is

```
\mdef[<index entry>]{<text>}   % used inside math
\tdef[<index entry>]{<text>}   % used in text
```

each of which colours its argument and writes one `\index` entry — `\mdef`
wrapping the entry in `$...$`, `\tdef` not.

**Use the plain one-argument form.** The optional argument overrides the index
entry and is almost never needed; reach for it only when the marked text cannot
serve as the entry at all — typically when one definition introduces a family of
symbols under a shared name (`\mdef[^0G,\, ^0H,\, ^0T]{^0H}`). If you find
yourself wanting it often, you are marking the wrong spans.

Note which of the two goes inside math. Putting `\tdef` in math mode, or `\mdef`
in text, breaks the entry (and often the compilation).

## 2. What to mark

Mark a notation or a concept when **all** of these hold:

- it is **defined** there — the sentence says what it is, not merely uses it;
- it is **global**: used throughout the paper, or throughout a whole section or
  appendix. A symbol that lives and dies inside one proof is not marked, however
  prominent;
- it is the **definitive** occurrence. A notation introduced informally in the
  introduction and then defined properly in the preliminaries is marked once, at
  the definition — unless the introduction is its only definition, in which case
  mark it there.

Concretely, sweep in this order:

1. every `notation` and `definition` environment;
2. free text that defines: "Throughout the paper $\bfG$ is …", "Let $G:=\bfG(F)$",
   "Fix a uniformizer $\varpi$", "we denote by … the …". These are easy to miss
   and are often the most global notations in the paper;
3. the section openings that fix an object for a whole section ("Throughout this
   section we fix a cone $L$", "Let $\chi$ be the central character of $\pi$");
4. the appendix, which usually has notation of its own.

Mark the **concept name** as well as the symbol only when the passage actually
defines the concept — either by saying what it is, or by citing where it is
defined. A bare mention does not qualify, however standard or important the
concept is.

The test is what a reader gains by following the index entry: an entry that lands
where the concept is only named has told them nothing.

- "Denote by $p$ the Chevalley map." — mark $p$ only.
- "Denote by $p$ the Chevalley map. It sends a matrix to its characteristic
  polynomial." — mark $p$ and `\tdef{Chevalley map}`.
- "Denote by $p$ the Chevalley map, see \cite{Bible}." — mark both; a citation to
  where the concept is defined counts as defining it here.

This holds whether or not the concept is the paper's own, and whatever the paper
does elsewhere: an existing mark on a merely-mentioned concept is a mistake to
fix, not a practice to follow.

## 3. What not to mark

- **A name mentioned without being defined and without a reference.** "…is
  tempered", "the contragredient $\tilde\pi$", "the (extended) Bruhat-Tits
  building of $G$": the symbol is being introduced, the concept is not. Mark the
  symbol, leave the name alone. This is the single most common over-marking
  mistake.
- **Proof-internal symbols**, including the constants a proof invents ($c$, $d$,
  $S$ the shift) and the bound variables of a statement ("let $p$ be a
  polynomial", "let $f$ be a function", "there is $c\in\Z_{>0}$" when nothing
  later refers to *that* $c$ by name).
- **Running variables** of a section — the fixed but unnamed $\pi$, the generic
  $\lambda$, $n$, $k$.
- **A second occurrence** of something already marked elsewhere.

## 4. Objects named inside a statement — conditional

Some papers have a convention in their conventions section along the lines of:

> Several statements concern the existence of objects satisfying some conditions.
> We assign a distinct name to such an object inside the formulation of the
> statement; after the statement we fix such an object and refer to it by that
> name.

**Only if the paper actually has such a convention**, also mark the objects it
covers: the name a theorem or lemma gives to the object whose existence it
asserts, when later text refers to it by that name across statements
(`\mdef{c_{asm}(\pi,\lambda)}`, `\mdef{\cR_{\cS}}`, `\mdef{c_{stab,g,\lambda}}`).
Check the criterion by grepping: if the name occurs only inside its own statement
and proof, it is proof-internal and stays unmarked.

If the paper has no such convention, skip this whole class — a `\Cref` to the
statement is how it refers to those objects, and marking them would index names
the paper does not treat as notation.

## 5. Method

- Read the whole file first and build the list of candidates with their line
  numbers and their verdict. Do not edit while reading.
- Apply the edits with a script that matches each target as an **exact string**
  and aborts if the string does not occur exactly once. Hand-editing dozens of
  sites, or `sed` on loose patterns, silently hits the wrong occurrence.
- If, while sweeping, you find an existing mark on the wrong thing — the paper
  says "Pulling back to `\tdef{$A_{\bfT}$}` we get $\Lambda^+(A_{\bfT})$" when
  what is being defined is $\Lambda^+(A_\bfT)$ — fix it and say so in your report.
  That is part of the task, not a separate edit.

## 6. Verify

The typeset text must not move. Compile the base commit and the edited file
separately and compare:

```
git show HEAD:main.tex > /tmp/base/main.tex   # plus the .sty and .bib
latexmk -pdf -interaction=nonstopmode -outdir=/tmp/build main.tex
diff <(grep -o "LaTeX Warning:.*" /tmp/base/main.log|sort|uniq -c) \
     <(grep -o "LaTeX Warning:.*" /tmp/build/main.log|sort|uniq -c)
```

Expect: identical warnings, identical overfull-box count, the same page count,
and a `main.idx` whose new entries are exactly the ones you added. Read the
sorted `.idx` — a garbled entry (a stray `\tdef` inside math, a missing optional
argument) shows up there immediately.

## 7. Report

List the judgement calls, not the mechanical wraps: what you marked in free text
rather than in a notation environment, which named objects you treated as
covered by the existence convention, and the things you deliberately left
unmarked because they are used but never defined — that last list is usually
worth the author's attention on its own.
