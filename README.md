Deployed version: https://nus-timetable.onrender.com/

# Course Desk

A personal, single-page calendar for tracking your subjects — weekly class
timetable + assignment/exam deadlines — in one screen. No backend, no
database: everything is saved in your browser's local storage.

Pre-loaded with: `GEN2062Y`, `IS1128`, `CS4238`, `CFG1104`, `CFG3001`,
`FCS2101`. Add, edit, or remove subjects any time from the **Subjects** tab.

There are two kinds of entries, kept deliberately separate, but shown
together on one calendar:

- **Weekly classes** — lectures, tutorials, labs. Recurring by nature: add a
  class once and it shows up every week automatically, with no re-entry.
  Optionally give it a **Starts on / Ends on** date (e.g. your semester's
  teaching weeks) and it'll stop appearing on its own once that range passes
  — otherwise it just repeats indefinitely.
- **Tasks & deadlines** — assignments, exams, anything one-off with a due
  date. These don't repeat; tick the checkbox once you've finished one.

The Weekly classes tab shows both together: recurring classes in the timed
grid below, and anything due that day in a "DUE" row along the top of each
day column — so one glance tells you what's on and what's due. Tick items
off right there, or click through to edit. Deadlines move with you as you
navigate between weeks.

## Week navigation & NUS week labels

The Weekly classes tab has Prev / Today / Next controls and shows the
current week labelled the way NUS labels it — **Week 1** through **Week
13**, **Recess Week**, **Reading Week**, **Examination (Week 1/2 of 2)** —
rather than just a calendar date.

This follows NUS's standard regular-semester structure, which has been
consistent across every year checked (back to AY2022): 6 teaching weeks →
Recess Week → 7 more teaching weeks (13 total) → Reading Week → 2 weeks of
Examinations. That structure is built in and doesn't need configuring.

The one thing that **does** need setting, because it changes every semester
and I can't reliably know it in advance, is the **Week 1 start date** — set
it via **Semester settings** in the Weekly classes tab. It's pre-filled with
Monday, 10 August 2026, which is Week 1 for AY2026/2027 Semester 1 per the
official NUS registrar calendar (nus.edu.sg/registrar) — verified against
that PDF directly, not guessed. Update this one field at the start of each
new semester and every label recalculates automatically. If you're on a
programme with its own calendar (Law, Medicine, etc.), double-check your
faculty's dates — some run slightly differently.

Note: the week labelling logic doesn't know when a *second* semester begins,
so if you keep navigating forward past Semester 1's exam period it'll just
say "Vacation" indefinitely rather than rolling into Semester 2's Week 1 —
update the Week 1 date yourself when the new semester starts.

## Important: how data is stored

This app has **no server and no database** — that was your call, since
Render's free PostgreSQL tier expires 30 days after creation (verified
against Render's docs, Aug 2026) and this is meant to stay free forever.

That means your data lives only in the browser you're using, on the device
you're using it on. Concretely:

- It survives closing the tab, restarting your laptop, and redeploying the
  site.
- It does **not** sync across devices or browsers, and it's lost if you clear
  that browser's site data.
- Use the **Export backup** button regularly (e.g. once a week, or after
  adding a batch of deadlines) — it downloads a `.json` file. **Import
  backup** loads one back in, and works on a different device/browser too if
  you carry the file over.

If you later want it to sync across your phone and laptop, that needs a real
backend + database, which is a bigger step up (and a paid one, on Render).
Worth it only if the single-device limitation actually bothers you in
practice.

## Deploying to Render (free)

Render's static sites deploy from a Git repository — there's no drag-and-drop
upload — so:

1. Create a new GitHub (or GitLab/Bitbucket) repo and push this folder to it
   (just `index.html` is required):
   ```bash
   git init
   git add index.html README.md
   git commit -m "Course Desk"
   git branch -M main
   git remote add origin <your-repo-url>
   git push -u origin main
   ```
2. On [render.com](https://render.com), click **New +** → **Static Site**.
3. Connect the repo you just pushed.
4. Leave **Build Command** empty and set **Publish Directory** to `.`
   (the repo root, since `index.html` sits there directly).
5. Click **Create Static Site**. Render gives you a live
   `https://<something>.onrender.com` URL with HTTPS.

Static sites on Render's free tier don't spin down and don't expire — unlike
free web services (which sleep after 15 minutes of inactivity) or free
Postgres databases (which expire after 30 days), this should just stay up.

Any time you edit `index.html` and push to `main`, Render redeploys
automatically.

## Local use without deploying anything

You can also just open `index.html` directly in a browser (double-click it)
— it works fully offline with no server at all. Deploying to Render is only
useful if you want a stable URL you can bookmark/open from your phone too.
