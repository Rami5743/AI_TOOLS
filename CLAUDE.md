# AI_TOOLS

This repository is a **Claude Code marketplace** whose entire content is a
single **plugin** made of **skills**. There is no application code here — a
change to this repo is almost always adding, editing or removing a skill.

## Layout

```
.claude-plugin/
  marketplace.json   # marketplace manifest: lists the one plugin, source "./"
  plugin.json        # plugin manifest: name, version, "skills": ["./skills"]
skills/
  <skill-name>/
    SKILL.md         # required: YAML frontmatter + body
    ...              # optional: references/, scripts/, assets/
```

The repo is both the marketplace and the plugin: `marketplace.json` points at
`"source": "./"`, so the plugin is the repository root and
`plugin.json` picks up every directory under `skills/`.

## Adding a new skill

1. Create `skills/<skill-name>/SKILL.md`. The directory name is the skill name:
   lowercase, words separated by hyphens, and it must equal the `name` in the
   frontmatter.

2. Write the frontmatter. Only `name` and `description` are required:

   ```markdown
   ---
   name: my-skill
   description: What the skill does, and — most importantly — WHEN Claude should
     use it. Include the phrasings a user would actually say. This string is the
     only part of the skill Claude sees before deciding to invoke it, so it is
     the whole trigger.
   ---

   # My skill

   The instructions Claude should follow once the skill fires.
   ```

3. Put anything long in side files next to `SKILL.md` (`references/`,
   `scripts/`, `assets/`) and link to them from the body. Only the body of
   `SKILL.md` is loaded when the skill fires; the frontmatter `description` is
   loaded in *every* session, so keep it to what is needed to trigger.

4. Bump `version` in `.claude-plugin/plugin.json` (and the mirrored
   `metadata.version` in `.claude-plugin/marketplace.json`). Consumers pull by
   git ref, so an unbumped version makes updates invisible.

5. Validate — all three of these must pass:

   ```bash
   claude plugin validate . --strict                   # marketplace manifest
   claude plugin validate skills --strict              # every SKILL.md
   claude plugin validate .claude-plugin/plugin.json   # plugin manifest
   ```

   The third one is deliberately not `--strict`. Validating the plugin manifest
   also inspects the plugin root, and it warns that this repo's `CLAUDE.md` "is
   not loaded as project context" — true for a repo that *consumes* the plugin,
   but this `CLAUDE.md` is for whoever works *in this* repo, so the warning is
   expected. `--strict` turns it into a failure. That one warning is the only
   output any of the three may produce.

6. Check the inventory and the token cost the new skill adds to every session:

   ```bash
   claude plugin marketplace add ./
   claude plugin install ai-tools@ai-tools
   claude plugin details ai-tools@ai-tools     # lists skills + always-on tokens
   claude plugin uninstall ai-tools@ai-tools   # clean up afterwards
   claude plugin marketplace remove ai-tools
   ```

7. Commit and push to `main`. Consuming repos pick the skill up on their next
   session; nothing else has to be released.

## Removing or renaming a skill

Delete or rename the directory *and* the `name` in its frontmatter together —
they must stay equal. Bump the version, re-run the three validations.

## Rules

- No skill directory without a `SKILL.md`.
- `name` in the frontmatter == the directory name, always.
- Never add a second plugin manifest. This repo is one plugin; new capability
  is a new skill under `skills/`, not a new plugin.
- Do not commit anything that is not part of a skill or a manifest.
