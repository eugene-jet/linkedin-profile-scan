# Working on this repository

This repository is a Claude Code plugin that ships exactly one skill, and it doubles as
its own marketplace. Both manifests live in `.claude-plugin/` with `source: "./"`, so the
`skills/` directory serves a plugin install and a manual `cp -r` into `~/.claude/skills/`
without any duplicated files. Keep that arrangement — moving the skill under a nested
plugin directory would break the manual install path documented in the README.

There is no build step and no dependencies. The skill is four plain files.

## Things that are duplicated on purpose, and must be changed together

**The band table** — the five rows explaining what Weak through Excellent mean — appears
in four places: `references/profile-rubric.md`, the slot documentation in
`assets/report-template.html`, `README.md` and `README.ua.md`. The rubric is the source of
truth. When the wording changes there, change it in the other three in the same commit; a
report whose legend disagrees with the rubric that produced it is worse than no legend.

**The version** appears in both `.claude-plugin/plugin.json` and the plugin entry in
`.claude-plugin/marketplace.json`. `claude plugin tag` refuses to tag a release when the
two disagree, which is the intended safety net, but it only fires at release time. The badge
at the top of both READMEs is not a third copy — it reads `plugin.json` from `main` at
render time, so it cannot drift, but it does mean that file's path is load-bearing and
renaming it would silently break the badge rather than fail a build.

**The route visibility matrix** — which of the three input routes can see what — exists
in `references/reading-a-profile.md` and again, condensed, in both READMEs. The reference
is the source of truth. This one has already drifted once: adding the `recommendations`
and `searchability` metrics put two new rows in the reference and left the READMEs a row
short, so a reader comparing them saw two different answers to the same question. When a
scored input is added, it needs a row in all three and a place in the Route 3 ask list,
which is the only route where the data arrives because somebody was asked for it.

**The two READMEs** are a full translation of each other, not a summary and a translation.
A section added to one belongs in the other. The language switch at the top of each links
to the other file by name.

## Language rules for the skill files

Everything under `skills/` is written in English, including placeholder markers and the
report template's headings. This is deliberate: the skill instructs Claude to translate
the report's own chrome into whatever language the reader is using, so hardcoding one
language in the files themselves would fight that instruction.

Before committing a change to the skill, confirm nothing crept back in:

```bash
grep -rniE '[а-яіїєґ]' skills/
```

It should print nothing.

## Before pushing

```bash
claude plugin validate .
```

To test the whole install path end to end, add the working copy as a local marketplace by
absolute path, install from it, then remove it again — a relative `.` is rejected by
`claude plugin marketplace add`.
