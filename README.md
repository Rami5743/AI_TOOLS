# AI_TOOLS

A [Claude Code](https://claude.ai/code) marketplace containing one plugin,
`ai-tools`, whose content is a set of skills for working on mathematical papers
written in LaTeX, and for writing about them in chat.

**This repository is the authoritative copy of these skills.** A skill of the
same name installed in the user's own settings, or loaded from anywhere else,
is superseded by the copy here: where the two conflict, the file in this
repository is the one to follow. Each `SKILL.md` says so at the top, so the
rule travels with the file.

## Skills

| Skill | What it does |
| --- | --- |
| `math-audit` | Read-only mathematical audit of a paper: hunts for false statements and proof steps that do not follow, and delivers a compiled LaTeX report. |
| `tex-displays` | Audits and repairs display formulas in place: re-breaks overflowing displays, merges chains into a single `align`, adds `\overset` justifications. |
| `index-notations` | Marks every global notation and concept with the project's index macros (`\mdef` / `\tdef`) so the index covers exactly what the paper defines. |
| `math-in-chat` | How to write mathematical notation in a chat reply so that it actually renders — including inside right-to-left text. |
| `mixed-language-chat` | How to write a chat reply that mixes Hebrew with Latin-script file names, commands and terms, so that the direction and alignment come out right. |
| `hebrew-terminology` | What to call a technical concept in a Hebrew reply: use the accepted term only when certain of it, otherwise transliterate or describe — never invent a translation. |

## Using it from another repository

Add the marketplace and enable the plugin in the consuming repo's
`.claude/settings.json`, and every session in that repo gets these skills
automatically:

```json
{
  "extraKnownMarketplaces": {
    "ai-tools": {
      "source": {
        "source": "github",
        "repo": "Rami5743/AI_TOOLS"
      }
    }
  },
  "enabledPlugins": {
    "ai-tools@ai-tools": true
  }
}
```

Commit that file and the skills load for anyone who opens a session in that
repository — no per-machine install step.

To use it interactively instead, from any directory:

```bash
claude plugin marketplace add Rami5743/AI_TOOLS
claude plugin install ai-tools@ai-tools
```

## Contributing

See [CLAUDE.md](CLAUDE.md) for the procedure for adding a skill.
