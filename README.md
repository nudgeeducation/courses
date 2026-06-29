# courses.nudgeeducation.online

A single-page, filterable course-finder for NEO's core placements plus holiday programmes and afterschool clubs. Static, no build step — just `index.html` and a `CNAME`. Brand and voice mirror the live `nudgeeducation.online` site.

## What's here

- `index.html` — the whole site (HTML + CSS + JS in one file). Course data lives in the `COURSES` array near the bottom; add, edit or remove a card by editing that array.
- `CNAME` — tells GitHub Pages to serve the site at `courses.nudgeeducation.online`.
- `README.md` — this file.

## 1. Publish on GitHub Pages

You can either add these files to a **new repo** (e.g. `nudgeeducation/courses`) or a folder in an existing one.

1. Create the repo under the `nudgeeducation` org and upload `index.html` and `CNAME` to the root.
2. **Settings → Pages →** Source: *Deploy from a branch*, Branch: `main`, Folder: `/ (root)`. Save.
3. Under **Custom domain** it should already read `courses.nudgeeducation.online` (from the CNAME file). Tick **Enforce HTTPS** once the certificate is issued.

## 2. Point the subdomain (GoDaddy DNS)

Add **one CNAME record** alongside your existing NEO records:

| Type | Name | Value | TTL |
|---|---|---|---|
| CNAME | `courses` | `nudgeeducation.github.io` | 1 hour |

> Use `nudgeeducation.github.io` (the org Pages host), **not** the repo URL. Allow up to an hour for DNS + the HTTPS certificate. This mirrors how `curriculum.` and `demo.` are already set up.

## 3. Wire up the expressions-of-interest form

The "Register interest" buttons currently point at the main NEO Register form as a safe default. To capture **which programme** each person wants, create a dedicated form and swap one line.

### Create the Google Form (≈5 minutes)

New form titled **"NEO Courses & Clubs — Register your interest"**. Suggested questions:

1. **Your name** — short answer (required)
2. **Your email** — short answer (required)
3. **Are you a…** — multiple choice: *Parent / carer* · *Learner (16+)* · *Local Authority commissioner / professional* · *Nudge colleague* (required)
4. **Which course, club or programme are you interested in?** — short answer *(this is the one the website pre-fills — see below)*
5. **Learner's age** — multiple choice: *11–13* · *14–16* · *17–18*
6. **Whereabouts are you / which Local Authority?** — short answer
7. **Anything you'd like us to know?** — paragraph (optional)
8. **Would you like to come to an open event?** — Yes / No

Link responses to a Google Sheet (Responses tab → Sheet icon) so the team can see interest by programme.

### Point the site at it

In `index.html`, find this line near the bottom and replace the URL:

```js
const EOI_FORM_URL = "https://forms.gle/UC84rtVbJELHpQdF7"; // Clubs & holiday programmes EOI form
const EOI_FORM_CORE_URL = "https://forms.gle/gSecpMw7GJYNNNk6A"; // Core placements EOI form
```

Two forms are wired: the **three Core placements** use `EOI_FORM_CORE_URL`; **all clubs and holiday programmes** use `EOI_FORM_URL`. Every "Register interest" link opens in a new tab. The site appends `?course=<programme name>` automatically (harmless on a `forms.gle` short link; it only pre-fills once you supply a full pre-filled link — see below).

### (Optional) Make the programme pre-fill

To have question 4 arrive pre-filled with the programme name: in the form, **⋮ → Get pre-filled link**, type any placeholder into "Which course…", submit, and copy the resulting link. It contains `entry.XXXXXXX=placeholder`. Then change the `eoiLink()` function in `index.html` so it builds that URL with `entry.XXXXXXX=` + the course title instead of `?course=`. Happy to do this step for you once the form exists.

## Editing the courses

Each card is one object in the `COURSES` array:

```js
{ t:"Title", type:"Holiday programme", area:"Creativity & expression",
  cs:["Creativity"], ages:"Ages 11–16", stage:"KS3–KS4",
  fmt:"Live online, weekly", d:"One or two sentence description." }
```

- `type` must be exactly `Core offer`, `Holiday programme` or `Afterschool club` (drives the filter and the badge).
- `cs` is one or more Cornerstones: Connection, Movement, Creativity, Reflection, Rest, Nutrition.
- The Interest area, Cornerstone and Age filters populate themselves from the data — no separate list to maintain.

## A note on consistency

This page mirrors the live site's public voice ("named practitioner", "qualified teachers", "voluntary Ofsted accreditation") so it reads as part of the same site. If the canonical terminology (Practitioner-Mentor, Educators, OEAS) is adopted, update the live site and this page together.

No prices are shown anywhere, in line with the "procurement-ready — request a pricing brief" approach on the main site.
