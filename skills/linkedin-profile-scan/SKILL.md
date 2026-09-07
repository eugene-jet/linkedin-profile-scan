---
name: linkedin-profile-scan
description: Scan a LinkedIn profile and return a scored audit plus concrete rewritten text — headline options, a redrafted About, reworked Experience bullets, what to put in Featured, and the recommendation requests to send. Works on the person's own profile or on someone else's by URL. Use this skill whenever someone asks to review, audit, check, improve, rate or "scan" a LinkedIn profile, asks why their profile gets no views or no recruiter interest, asks how to write their LinkedIn headline or About section, wants feedback before a job search, or wants to help a mentee or colleague improve their profile — even if they do not say the word "audit" and even if they only paste a LinkedIn URL with "what do you think?". Also use it when someone asks what is wrong with their personal brand on LinkedIn.
---

# LinkedIn profile scan

## What this does

Reads a LinkedIn profile, scores it out of 100 across three sections, and — this is the
part that matters — hands back the rewritten text. A score alone changes nothing. Someone
who learns their About section is weak still has to sit and write a new one, which is the
step they were already avoiding. So the report ends with drafted replacements they can
paste, not advice about what to think about.

Everything scored here is visible on the profile itself. Impressions, profile views and
follower growth are private to the account owner, so they are deliberately out of scope —
see [When the person owns the profile](#when-the-person-owns-the-profile) for what to do
when the owner is present and willing to share them.

## Step 1 — Get the profile in front of you

Read `references/reading-a-profile.md`. Three input routes, and the right one depends on
whose profile it is and what tools are available. Pick one and say which you used — the
routes differ in what they can see, and a reader should know which blind spots apply.

Briefly:

- **Live, in the person's own browser** — richest, needs Claude in Chrome
- **LinkedIn's own data export** — fully sanctioned, no automation, owner only
- **Pasted text or screenshots** — always works, needs the person to do the copying

A note worth passing on if you are driving a browser: LinkedIn's user agreement forbids
automated scraping, and profiles you open appear in that person's "Who viewed your
profile". Neither matters much when someone opens their own profile in their own logged-in
browser, which is what this skill normally does. Both matter when auditing a stranger at
volume, so do not build that.

## Step 2 — Score

Read `references/profile-rubric.md` for the fourteen metrics and their bands.

| Section | Checks | What it asks |
|---|---|---|
| Profile | 7 | Does the profile say who this person is, prove it, and get found? |
| Content | 4 | Do they publish, and is it worth reading? |
| Activity | 3 | Do they show up in other people's threads? |

Each check scores 0–5. A section's grade, and the overall, are the share of available
points earned, reported out of 100 — so a section with fewer checks still lands on the
same 0–100 scale, and the bands in the rubric already sit on it. Report the overall as a
single number out of 100; it is the weighted average across all fourteen checks, so the
three section grades will not simply add up to it, which is expected.

Two habits keep the score honest:

**A metric with no data is `null`, never 0.** A profile whose activity tab is empty
because the person posts nothing scores 0 for cadence — that is a real finding. A profile
whose activity you could not load scores `null` — that is a gap in the audit. Conflating
them tells someone they failed at something nobody checked.

**Say what the evidence was.** Every score needs the quote, the count or the date beside
it. A person reading "your Experience section is weak" argues. A person reading "nine of
your eleven bullets start with a verb describing a duty, and none contain a number" fixes
it.

## Step 3 — Write the replacements

This is the deliverable. For every section that scored below 4, produce actual text, not
guidance. `references/profile-rubric.md` carries the patterns — headline formula, About
structure, the bullet shape that turns a duty into an outcome.

What "actual text" means:

- **Headline** — three options, differing in emphasis, each under 220 characters
- **About** — one complete draft, in their voice, using only facts visible in their profile
- **Experience** — every role in the profile, checked and addressed; recent roles
  rewritten bullet by bullet, older ones given a targeted strengthening line. Never skip a
  role, and never invent an outcome for one too old to have a recorded number. Pair the
  original text with the rewrite for every role — show the current bullets, then the
  replacement, so the reader sees the contrast rather than a rewrite they have to trust
  blind. In the report this is a before block and an after block under each role; the
  before block carries the bullets exactly as they stand on the profile
- **Featured** — a specific list of what to put there, drawn from what they actually have
- **Recommendations** — the request messages to send, addressed to specific people already
  visible in the profile. The recommendation itself is the one thing here nobody can draft
  for them, so draft the ask instead
- **Searchability** — the terms the profile is missing and the top three Skills to set

Where a number would strengthen a line but you do not have it, leave a marked blank like
`[N projects]` rather than inventing one. A fabricated metric in someone's professional
profile is worse than an empty one, and they are the only person who can fill it.

### Which language to write in

Three languages are in play here and they are not the same one.

**The drafted profile text follows the profile.** If the profile is in English, the new
headline, About and bullets are in English — that is the text a recruiter will read, and
it does not change language because the conversation happens in another one.

**Everything else follows the reader.** Your commentary, the verdict, the reasons, and
the report page's own headings and labels are written in the language the person is
writing to you in. The template in `assets/report-template.html` ships with English
headings; translating them is expected, not optional. A Ukrainian speaker reading a
report about their English profile should meet English only inside the drafted text.

**Metric keys, LinkedIn field names and URLs stay verbatim.** `headline_clarity`,
`About`, `Featured`, `Experience` are names of things, not words to translate.

## Step 4 — Deliver

Build the report as an artifact from `assets/report-template.html`. It is a complete page
with `<!--SLOT:*-->` markers; replace each marker with the corresponding content and
publish. Everything the person needs to act on should be on that one page, because a
report split between chat and a page gets half-read.

The `SCORECARD` slot shows the person every criterion the profile was judged on: all
fourteen checks, grouped by section, each with its raw 0–5 score and the one line of
evidence behind it. You already produced both in Step 2 — the scorecard is where they land
on the page, so a reader who sees "Profile 54" can tell which checks earned it. Keep the
metric keys (`headline_clarity` and the rest) verbatim and translate the labels and
evidence like the rest of the page, and show a metric you could not read as a null row
rather than a zero, exactly as the `null`-versus-0 rule in Step 2 requires.

Title the artifact with the person's name and what it is, for example
`Elena's LinkedIn profile — scan`. Give it a one-line description naming the score.

If publishing an artifact is not available in the environment you are running in, do not
abandon the page — fill the same template, write it to an HTML file, and tell the person
the path and that they can open it in a browser. The report is the deliverable; the
artifact is only the most convenient way to hand it over.

If the person only wants the short version, give them the three highest-value fixes in
chat and still publish the page — the drafted text is too long to be useful in a
scrollback, and they will want it again next week.

## When the person owns the profile

Ask whether they want to add their private analytics — impressions, profile views,
follower growth, engagement rate. They are on the profile under Analytics → Show all, and
they unlock the half of the picture this scan cannot see: whether the profile is actually
being found and whether the content travels.

If they say yes, and a fuller monthly-tracking instrument is installed, hand off to it
rather than re-deriving the maths here. If not, report the analytics as
context alongside the 0–100 public-profile score instead of folding them into it — mixing
a public score with private numbers makes two profiles incomparable, which defeats the
point of having a score at all.

## What this skill will not do

**No mass scanning.** Auditing a profile at someone's request is a favour. Iterating over
a list of profiles is surveillance, breaks LinkedIn's terms, and gets the account
restricted. If asked for a batch, say so and offer to do them one at a time as they come up.

**No invented facts.** Not in the score, not in the drafted text. Everything traces to
something visible on the profile or something the person told you.

**No flattery pass.** The person asked because they suspect something is wrong. A report
that scores everything 4 and suggests minor polish wastes the ask. If the profile is
genuinely strong, say which two things are still costing them and why.
