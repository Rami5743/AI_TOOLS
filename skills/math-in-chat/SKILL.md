---
name: math-in-chat
description: >-
  How to write mathematical notation in a chat reply so that it actually renders,
  with rules established by testing against a real client — the delimiters that
  work, why a lone letter silently fails, and what right-to-left text does to a
  formula. Use this whenever a reply will contain mathematical notation of any
  kind — a formula, an equation, a group or space named by a symbol, an index, an
  exponent, a Greek letter. Use it especially when the conversation is in Hebrew,
  Arabic, or any other right-to-left language, where notation placed inside a
  sentence comes out silently scrambled. Also use it when the user says a formula
  "did not render", "came out as code", "is reversed", "the subscript is on the
  wrong side", or quotes raw LaTeX back at you from your own previous reply.
---

# Writing mathematics in chat so it renders

Notation in a chat reply passes through a markdown renderer before the reader sees
it, and that renderer is fussy in ways that are invisible from the writing side. A
formula you were sure about can arrive as literal text, or with its subscript on the
wrong side of the letter. The reader then has to decode it, which defeats the point
of writing notation at all.

The rules below were established by testing candidate forms against a real client and
asking the reader which ones came out right. Follow them and the reader sees what you
meant. They cost nothing when the renderer happens to be forgiving.

## The five rules

### 1. Delimiters: parentheses and brackets, not dollars

Inline notation goes in `\(` and `\)`. A displayed formula goes on a line of its own,
in `\[` and `\]`, with a blank line before and after.

Dollar signs arrive as raw text. This surprises people because `$...$` is the habit
from LaTeX source, but the renderer here does not accept it.

The exception is LaTeX **code you are handing the user to paste** into their own
document. That is source, not rendered output, and should follow whatever their
project uses — commonly `$` and `$$`. Keep the two straight: what you render for
reading, and what they copy for compiling, are different artifacts.

### 2. A math span must contain at least one backslash command

The renderer only treats `\(...\)` as mathematics if it finds a command inside. This
is a sensible heuristic on its part — plain parentheses around a word are ordinary
prose, and it declines to typeset them. But it means:

- `\(t\)` arrives as the literal text `(t)`
- `\[G_0\]` arrives as the literal text `[G_0]`
- `\(\lambda\)` renders, because `\lambda` is a command

So a bare Latin letter needs a wrapper. Write `\(\mathit{G}\)`, not `\(G\)`. Greek
letters, operators, and anything else already spelled with a backslash need nothing
extra. This applies to displayed formulas exactly as to inline ones — a display
holding nothing but `G_0` fails just as an inline span would.

`\,` or `\!` before the letter also make it render, but each adds visible space.
`\mathit{...}` is the clean form.

### 3. Leave a space before the opening delimiter

If `\(` is glued to the character before it, the renderer does not see it. This bites
hardest in Hebrew, where one-letter prefixes attach through a maqaf: writing the
equivalent of "in-T" as `ב־\(\mathit{T}\)` fails outright.

Use a whole word and a space instead — "inside the torus", then a space, then the
symbol. The sentence reads better anyway.

### 4. Right-to-left text reorders a formula, so give it its own line

In a paragraph whose direction is right-to-left, the bidirectional algorithm moves
the pieces of an inline formula. A superscript lands on the wrong side of its base —
`G^{ad}` comes out with the `ad` on the left, and `\varpi^{\tilde\lambda}` looks as
though the accent sits on the base rather than the exponent. The formula renders; it
just says something else.

What survives, and what does not:

- **A single symbol inline is fine.** There is nothing to reorder. `\(\mathit{G}\)`
  or `\(\lambda\)` sits correctly inside right-to-left prose.
- **Anything with a subscript, superscript or accent must go on its own line**, as a
  display. A display is a block, so the paragraph's direction does not reach inside it.
- **A paragraph that begins with Latin script is left-to-right** and carries inline
  formulas freely, even if a right-to-left word appears later in it. So an
  English-language paragraph is a safe place for dense inline notation.

Note that what decides this is the direction of the **paragraph**, fixed by its first
strong character — not the language of the individual sentence. An English sentence
that follows a Hebrew label on the same line is inside a right-to-left paragraph and
will be reordered.

### 5. Keep an inline span short; display anything else

Inline spans have a failure that resisted diagnosis: a long expression combining
several indexed symbols with brackets and a function value fell through as text,
while each of its ingredients rendered fine on its own. What was ruled out by
testing: a leading bracket, length by itself, the number of subscripts, and
square brackets combined with parentheses — each of those renders. Something about
the full combination crosses a threshold that was not isolated.

Rather than gamble on where the line falls, keep inline for what is reliably safe:

- **Inline**: a single symbol, or a short expression of two or three tokens —
  a group with one index, a short equality, a membership.
- **Display**: everything else, and everything you are unsure about.

A display has never failed, at any length. When a sentence would need a long
expression inline, that is usually a sign the expression deserves its own line
anyway — the reader has an easier time with it there.

## Spacing around an inline span in right-to-left prose

The space between a right-to-left word and an inline formula tends to be swallowed,
so the symbol ends up touching the word. Put the space inside the formula instead,
where it is part of the rendered output and cannot be eaten:

    ...the torus \(\mathit{T}\,\), and therefore...

In right-to-left prose the symbol sits to the *left* of the word preceding it, so the
gap you need is at the formula's right edge — which is its **end** in writing order.
Hence the trailing `\,`. Put one at each end if both sides look tight.

## Writing around the constraints

Rule 4 pushes every indexed expression onto its own line, which can leave a
right-to-left paragraph chopped up. Three moves keep the prose readable:

- **Say the symbol in words.** "The group of level minus n", "the dominant element".
  Prose that names its objects is often clearer than prose studded with letters.
- **Show the statement, not the symbol.** Instead of naming two groups in two tiny
  displays to contrast them, display the one equation in which the contrast lives.
- **Switch the paragraph to English** when the discussion is notation-dense. An
  English paragraph takes inline formulas without complaint, and that is usually
  better than a right-to-left paragraph interrupted every clause by a display.

## Things already tried that do not work

Do not spend the reader's patience rediscovering these. All were tested and all fail:

- Unicode direction controls around the formula — isolates (LRI/PDI) or marks (LRM) —
  do not stop the reordering.
- Putting the formula last on the line does not help.
- A `dir` attribute is not honoured.
- A left-to-right mark at the *start* of the line does render the formula correctly,
  but it flips the whole paragraph to left-to-right and the right-to-left text is
  then left-aligned, which is worse than the problem.
- Unicode sub- and superscript characters typed as text (`G₀`, `G₋ₙ`, `Gᵃᵈ`) render,
  but they are ugly, they do not exist for most symbols, and a sign rather than a
  letter still flips (`T⁺` comes out reversed).
- Symbols in a code span are not mathematics and read as code; the reader will say so.

## Checking your own reply

Before sending, scan for these, in rough order of how often they bite:

- A `\(` or `\[` whose content has no backslash in it anywhere.
- A `\(` glued to the preceding character.
- A subscript, superscript or accent inside a right-to-left paragraph.
- A `$` outside a code box.
- A math span sitting immediately against a code span — that breaks the parsing of
  everything after it, so the rest of the reply arrives as raw text. Keep command
  names in a code span of their own, and never write `\backslash` inside math.

If the reader tells you something did not render, do not paper over it by removing the
notation. Work out which of these it was, fix that, and say which one it was — the
reader is debugging their client alongside you, and the diagnosis is worth as much as
the corrected text.
