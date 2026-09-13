---
name: hebrew-terminology
description: >-
  What to call a technical concept when the reply is written in Hebrew — a
  mathematical object, a LaTeX or git term, anything whose established name is
  English. The rule: use the accepted Hebrew term only when you are certain of
  it, otherwise transliterate or describe, and never assemble a Hebrew word by
  translating the English meaning. Use it in every Hebrew reply that names a
  concept, and whenever the user asks "what is that supposed to be", says a word
  is not the accepted term, or tells you to transliterate instead of translating.
  This is about the choice of words, not about direction or alignment — for those
  see the mixed-language-chat skill, and for formulas the math-in-chat skill.
---

> **Precedence.** This file, from the `Rami5743/AI_TOOLS` repository, is the
> authoritative copy of this skill. Where it conflicts with a same-named skill
> installed in the user's settings, or with any other copy, this file wins.

# Naming technical concepts in a Hebrew reply

## Why an invented translation is the worst option

A technical term is a name, not a description. When you translate the English
meaning into a Hebrew word of your own making, the result is usually a real
Hebrew word that already means something else — so it does not read as an
unfamiliar coinage that the reader can decode, it reads as a *different
concept*, and the reader stops to work out what you meant. That is worse than
leaving the English word standing, and far worse than a transliteration.

The failure is not ignorance of Hebrew; it is fluency. The invented word sounds
right. `האופי` for *character* is ordinary Hebrew, it is the dictionary
translation of the everyday sense of the English word, and it is the wrong
concept — the thing meant is a homomorphism to the multiplicative group, whose
name in Hebrew mathematical speech is `קרקטר`.

## The three options, in order

**1. The accepted Hebrew term — only when you are certain.** Certainty means you
have *read* or *heard* the word used for this concept in Hebrew, not that it is a
plausible rendering. `סריג`, `מחלקות כפולות`, `טורוס מפוצל`, `אלגברת חילוק`,
`הרמה` are terms of this kind.

**2. Transliterate.** `קרקטר`, `מונואיד`, `קונוס`, `רקורסיה`, `אוניפורמיזטור`.
A transliteration is always understood, it is never mistaken for another
concept, and — because it is written in Hebrew letters — it also avoids the
direction problem that a Latin-script word inside a Hebrew line creates. There
is no cost to it. Reach for it the moment the certainty test fails.

**3. Describe.** When even the transliteration would be awkward or unheard of,
name the thing by what it is: `איבר שהוולואציה שלו 1`, `תת־חבורה שהמנה בה חסרת
פיטול`. A description is longer but never wrong.

**Never** assemble a Hebrew word from the English meaning, and never present an
invented term as if it were standard.

## The test, applied before the word is written

> Have I read this word, in Hebrew, used for *this* concept — or am I
> translating the English right now?

If the answer is the second, transliterate. Do not upgrade "sounds like it could
be the term" into "is the term"; that is exactly the mistake. And do not mix the
two registers inside one reply: if you transliterated a concept once, keep the
transliteration throughout.

## Follow the user

The user's own words are the best evidence available, and they override
everything in this file. A term the user has used is certain by definition —
adopt it and keep it. Note also *how* they use language: a user who writes
`רפרנס`, `ריפו`, `סקיל`, `פיטול` is telling you that transliteration is the
house style, and translating those same words into invented Hebrew will read as
a mistake.

## Cases seen in practice

Each of these was written in a Hebrew reply about a mathematics paper, and each
was wrong.

| written | concept meant | what it should have been |
| --- | --- | --- |
| `האופי`, `האופי המרכזי` | character, central character | `קרקטר`, `הקרקטר המרכזי` |
| `אופנים` | the summands attached to the roots of a characteristic polynomial | `הרכיב המתאים לשורש` |
| `מדד` | uniformizer | `אוניפורמיזטור`, or `איבר שהוולואציה שלו 1` |
| `הישנות` | recurrence | `רקורסיה`, `יחס רקורסיה` |
| `אלגברת קווטרניונים אי־פריקה` | the quaternion division algebra | `אלגברת החילוק של הקווטרניונים` |
| `ערכה` | valuation | `ולואציה` |
| `רוויה` | saturated (subgroup) | `תת־חבורה שהמנה בה חסרת פיטול` |

Two patterns run through the table. The first four are ordinary Hebrew words
carrying an unrelated everyday meaning — `אופי` is a personality, `מדד` is an
index or a measure — which is what makes them unrecoverable for the reader. The
last two are cases where the right move was to admit the uncertainty rather than
to pick the more scholarly-sounding option.

## When the user corrects a term

State the correction, list the other places in the same conversation where you
used the same word or made the same kind of substitution, say what you will
write instead, and continue. Do not defend the choice, do not explain at length
how it happened, and do not apologise more than once. If the corrected term
reached a file — a paper, a report, a commit message — say so explicitly and
fix it there too; if it did not, say that as well, because the user needs to
know whether anything has to be repaired.

## Checking your own reply before sending

Go through the reply and stop at every Hebrew noun that is carrying technical
weight — every word that names an object, an operation or a property rather than
doing ordinary work in the sentence. For each one, apply the test above. Where
the answer is not "I have read this word used for this concept", replace it with
a transliteration or a description before sending.
