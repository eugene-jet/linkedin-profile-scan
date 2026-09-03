# LinkedIn profile scan

**English** · [Українська](README.uk.md)

A Claude skill that audits a LinkedIn profile and hands back the rewritten text.

Most profile advice stops at the diagnosis. You learn that your About section is weak,
and then you still have to sit down and write a new one — which is the part you were
avoiding in the first place. This skill scores the profile out of 100 across twelve
checks and then drafts the replacements: three headline options, a complete About section
in your own voice, reworked Experience bullets for every role, a list of what belongs in
Featured, and three content pillars with a posting cadence you can actually keep.

Everything lands on a single published page you can come back to next week.

## What the report contains

1. **A score out of 100** with its band, broken into three section grades — Profile,
   Content and Activity — each with a bar and an explanation of what its band means.
2. **What already works**, with the evidence, so you do not accidentally undo it.
3. **What costs the most**, ranked by points recoverable per unit of effort. Rewriting
   text you already have almost always outranks starting to post, because those points
   are sitting on the page waiting to be claimed.
4. **The rewrites.** This is the deliverable. Ready to paste, drawn only from facts
   visible on the profile — where a number would strengthen a line and the number is not
   known, the draft leaves a marked blank like `[N projects]` rather than inventing one.
5. **Content pillars and a cadence.**
6. **A method note** saying which route was used, what could not be seen, and which
   metrics are therefore `null` rather than zero.

## Requirements

Claude Code, or claude.ai if you install the skill there instead.

Nothing else is strictly required. The Claude in Chrome extension is needed only for the
live-browser route described below; the other two routes work with Claude Code alone.

## Install

### As a plugin (recommended)

From your terminal:

```bash
claude plugin marketplace add eugene-jet/linkedin-profile-scan
```

```bash
claude plugin install linkedin-profile-scan@linkedin-profile-scan
```

Or, from inside a Claude Code session, run `/plugin marketplace add
eugene-jet/linkedin-profile-scan` and then `/plugin install`, which will offer the plugin
in a menu. Restart Claude Code afterwards so the skill loads.

To update later:

```bash
claude plugin update linkedin-profile-scan
```

### By copying the skill directory

If you would rather not add a marketplace, clone the repository and copy the skill into
your personal skills directory:

```bash
git clone https://github.com/eugene-jet/linkedin-profile-scan.git
```

```bash
cp -r linkedin-profile-scan/skills/linkedin-profile-scan ~/.claude/skills/
```

The skill is four plain files — a `SKILL.md`, two references and one HTML template — with
no dependencies and nothing to build. Restart Claude Code and it will be available.

### On claude.ai

Zip the `skills/linkedin-profile-scan` directory so that `SKILL.md` sits at the root of
the archive, then upload it as a skill in your Claude settings. Note that the live-browser
route is not available there, so use the data export or the paste route.

## How to use it

There is no command to remember. Ask for what you want in plain language and the skill
activates on its own:

- *"Review my LinkedIn profile."*
- *"Have a look at linkedin.com/in/someone and tell me what's wrong with it."*
- *"Here's my headline and About section — what would you change?"* followed by the
  pasted text.

It also triggers on the questions people actually ask, such as why a profile gets no
recruiter interest, or how to write a headline before starting a job search.

The report is published as an artifact and you get the link. If you only want the short
version, ask for it — you will get the three highest-value fixes in chat, and the page is
still published, because the drafted text is too long to be useful in a scrollback.

## Which route the skill uses to read the profile

Three routes, differing in what they can see. The skill picks one, tells you which, and
reports the resulting blind spots honestly.

| | Live browser | Data export | Paste / screenshots |
|---|---|---|---|
| Whose profile | own or others' | own only | any |
| Profile text | yes | yes | yes |
| Featured | yes | partial | if captured |
| Photo and banner | yes | no | if captured |
| Posts | yes | yes, complete | if captured |
| Reactions and comments per post | yes | counts only | if captured |
| Comments written elsewhere | yes | yes | rarely |
| Setup needed | Chrome extension | you export a zip | nothing |

**Live browser** is the richest and needs the Claude in Chrome extension, because
LinkedIn does not serve profile content to a logged-out browser — it shows a signup wall
instead. This is the route to use for your own profile.

**Data export** is LinkedIn's own sanctioned download, requested under Settings → Data
privacy → Get a copy of your data. No automation is involved at all. It arrives by email,
usually within minutes.

**Paste** always works and needs nothing but your willingness to copy text and take a few
screenshots. A scan from pasted text is worth far more than no scan.

## How the scoring works

Twelve checks, in three sections. Each check scores 0–5, and each section is then
reported as the share of available points earned, out of 100.

| Section | Checks | What it asks |
|---|---|---|
| Profile | 5 | Does the profile say who this person is, and prove it? |
| Content | 4 | Do they publish, and is it worth reading? |
| Activity | 3 | Do they show up in other people's threads? |

The overall grade is the weighted average across all twelve checks, so the three section
grades will not add up to it — the same way subject grades on a report card do not sum to
the GPA.

| Band | Range | What it means |
|---|---|---|
| Weak | 0–39 | Empty, or contradicting itself. A reader cannot tell what this person does. |
| Developing | 40–59 | Filled in, but only with duties and labels. Nothing proves they can do the work. |
| Good | 60–74 | It works. The niche is clear and there are specifics — but no numbers, no proof that closes the question. |
| Strong | 75–89 | Every claim is backed by a result. A recruiter sees what they did and what came of it. |
| Excellent | 90–100 | The profile brings people in by itself. Niche, proof and steady content work together. |

Two rules keep the score honest. A metric with no data is reported as `null`, never as
zero: a profile with no posts scores zero for cadence, which is a real finding, while a
profile whose activity could not be loaded scores `null`, which is a gap in the audit.
And every score is stated with its evidence — the quote, the count or the date — because
"your Experience section is weak" invites an argument and "nine of your eleven bullets
open with a duty and none contain a number" invites a fix.

Only what is publicly visible on the profile is scored. Impressions, profile views and
follower growth are private to the account owner and are deliberately out of scope; if
you own the profile you will be asked whether you want to add them as context alongside
the score.

## Languages

The skill writes the report in whatever language you are speaking to it in, and drafts
the profile text in the language of the profile itself. A Ukrainian speaker auditing an
English profile gets Ukrainian commentary and English replacement text, which is the
combination that is actually useful.

## Limits, and what this will not do

**No mass scanning.** Auditing one profile at someone's request is a favour. Iterating
over a list of profiles is surveillance, it breaks LinkedIn's user agreement, and it gets
accounts restricted. If you ask for a batch, the skill will decline and offer to do them
one at a time.

**No invented facts**, in the score or in the drafted text. Everything traces back to
something visible on the profile or something you said.

**No flattery pass.** You asked because you suspect something is wrong. A report that
scores everything 4 and suggests minor polish wastes the question.

Two things worth knowing before you point this at someone else's profile: LinkedIn's user
agreement forbids automated scraping, and any profile you open appears in that person's
"Who viewed your profile". Neither matters when you open your own profile in your own
logged-in browser, which is the normal case.

## Licence

MIT. See [LICENSE](LICENSE).
