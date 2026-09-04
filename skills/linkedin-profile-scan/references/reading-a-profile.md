# Reading a profile

Three routes in. They differ in what they can see, so the route determines which metrics
are scorable and which come back `null`. Always record which route was used.

| | Live browser | Data export | Paste / screenshots |
|---|---|---|---|
| Whose profile | own or others' | own only | any |
| Profile text | yes | yes | yes |
| Featured | yes | partial | if captured |
| Recommendations | yes | yes | if captured |
| Skills, location, industry | yes | yes | if captured |
| Photo & banner | yes | no | if captured |
| Posts | yes | yes, complete | if captured |
| Reactions & comments per post | yes | counts only | if captured |
| Comments written elsewhere | yes | yes | rarely |
| Setup needed | Chrome extension | person exports a zip | nothing |

Default to the live route when the tooling is there and the person is auditing their own
profile. Fall back to paste when it is not — a scan from pasted text is worth far more
than no scan.

---

## Route 1 — Live browser

Needs Claude in Chrome, where the person's own LinkedIn session is active. The in-app
browser pane will not work: logged out, LinkedIn serves a signup wall instead of the
profile.

Check the extension is connected before promising anything. If it is not, say so and
switch to paste rather than collecting a partial profile and scoring the gaps as failures.

**The profile page**, `https://www.linkedin.com/in/<slug>/`

Text extraction returns the headline, location, connection count and the activity feed —
but it **skips About and Featured**, which sit in a part of the DOM extractors treat as
page furniture. Those two need screenshots: scroll to just below the header and capture.
The same screenshots let you judge the photo and banner, which are scored.

**Recommendations and Skills** each live on their own page as well, and both are scored:

```
https://www.linkedin.com/in/<slug>/details/recommendations/
https://www.linkedin.com/in/<slug>/details/skills/
```

Read the recommendations for who wrote them and when, not only how many there are — a
reciprocal pair written on the same day and a recommendation from a former manager are
worth very different things. On the skills page only the top three matter for scoring;
they are the only ones shown without expanding.

**Experience** lives on its own page and is worth loading separately, because the profile
itself only expands the current role:

```
https://www.linkedin.com/in/<slug>/details/experience/
```

Text extraction works well here and returns every position with its full bullet list.
Read it for two things at once: whether bullets state outcomes or only duties, and whether
the dates contradict each other. Overlapping full-time roles and a years-of-experience
count that disagrees with the headline are both common, both invisible to the profile's
owner, and both read as carelessness to a recruiter.

**Posts**, `https://www.linkedin.com/in/<slug>/recent-activity/all/`

The visible ages — `1w`, `2mo` — round hard, and `1mo` covers anywhere from 30 to 59 days.
That is fine for a 90-day cadence window but useless for anything tighter. Exact dates are
encoded in the activity id, first 41 bits being the millisecond epoch:

```js
(() => {
  const ids = new Set();
  document.querySelectorAll('[data-urn],[data-id],a[href*="activity"]').forEach(el => {
    const s = el.getAttribute('data-urn') || el.getAttribute('data-id') || el.getAttribute('href') || '';
    const m = s.match(/activity[:\-](\d{19})/);
    if (m) ids.add(m[1]);
  });
  return [...ids].map(id => ({
    id,
    date: new Date(Number(BigInt(id) >> 22n)).toISOString().slice(0, 10)
  }));
})()
```

Reaction counts render as `Name and 72 others reacted`, which means 73 people, not 72.
Reposts carry the original author's card inside them — that is how you tell an original
post from a share.

**Comments the person wrote**, `.../recent-activity/comments/`

Three traps, and together they can triple a count:

The list loads lazily and only responds to real scrolling — a scripted `window.scrollTo`
does not trigger it. Items are also recycled out of the DOM once scrolled past, so a
single extraction at the bottom shows gaps. Accumulate while scrolling.

And the timestamp shown next to a post is the **post's** age, not the comment's. What is
being measured is when this person commented; their own comment block carries its own age,
marked `Author` on their own posts and `• You` elsewhere.

Split the results three ways, because they answer different questions: comments on other
people's posts, replies under their own posts, and — of the first group — how many say
something rather than "thanks" or an emoji.

---

## Route 2 — LinkedIn's data export

The person requests it themselves: **Settings → Data privacy → Get a copy of your data**.
Small archives arrive in minutes, full ones within 24 hours. No automation, nothing that
can get an account restricted, and it is the most complete record of their own posting
history that exists.

Useful files in the archive:

| File | Carries |
|---|---|
| `Profile.csv` | headline, summary (About), industry, location |
| `Positions.csv` | every role with dates and descriptions |
| `Shares.csv` | every post with date, text and link |
| `Comments.csv` | every comment they wrote, with date and the thread |
| `Reactions.csv` | what they reacted to |
| `Skills.csv` | the skills list, in the order set on the profile |
| `Recommendations_*.csv` | recommendations received and given, with dates and authors |

What the archive cannot give: the photo, the banner, Featured, and per-post reaction
counts. Score those `null` unless the person also sends a screenshot of their profile
header, which takes them ten seconds and is worth asking for.

Ask for the folder path and read the CSVs directly. `Shares.csv` makes cadence and
originality exact rather than estimated, which is a real upgrade over the live route.

---

## Route 3 — Paste or screenshots

Always available, no setup, works for any profile the person can see.

Ask for, in this order of value:

1. A screenshot of the profile top — photo, banner, name, headline, About
2. The Experience section, pasted as text or captured
3. Featured — what is in it
4. Three to five recent posts, pasted
5. Follower and connection counts

Most people send the first two and stop. Score what arrived, mark the rest `null`, and
say in the report which sections went unexamined. Then ask for the missing pieces once —
a person who sees a half-finished report usually sends the rest.

---

## Degree of connection limits what you see

On someone else's profile, visibility depends on the connection:

- **1st degree** — the full profile
- **2nd / 3rd** — usually the full profile, sometimes without contact details
- **Out of network** — often only a name fragment, or `LinkedIn Member`

If a section is hidden rather than empty, say so. "No Featured section" and "Featured not
visible at this connection degree" lead to opposite advice, and getting it backwards makes
the whole report untrustworthy.
