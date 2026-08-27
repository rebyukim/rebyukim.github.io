# Feedback wall — setup & workflow

This adds a "Working feedback wall" section to `index.html`, right after the Contact
section: a public wall of testimonials on the left, and a submission form on the
right. It's built for a static site (GitHub Pages has no server), so it works like
this:

1. Someone fills out the form (first/last name, company, position title, email,
   comment, and a 1–5 star rating on three proficiencies: **Client & Stakeholder
   Relations**, **Strategic Execution**, **Communication**).
2. The submission goes to **Formspree** (a free form-backend service) — it does
   **not** appear on the site automatically.
3. You review it in your Formspree dashboard / inbox.
4. If you want it on the wall, you copy it into `testimonials.json` in this repo
   (format below) and push. It'll appear on the site on the next deploy.

This keeps the wall spam-free and fully under your control — nothing goes public
without you choosing to add it.

## One-time setup (required before the form works)

1. Go to [formspree.io](https://formspree.io) and create a free account.
2. Create a new form. Formspree gives you an endpoint like
   `https://formspree.io/f/abcd1234`.
3. Open `index.html`, find this line (search for `YOUR_FORM_ID`):
   ```html
   <form id="feedbackForm" class="feedback-form" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
   ```
   Replace `YOUR_FORM_ID` with your real form ID.
4. Commit and push. That's it — submissions will now land in your Formspree
   dashboard and be emailed to you.

Until step 3 is done, the form shows a friendly "not connected yet" message
instead of silently failing.

## Adding an approved entry to the wall

Open `testimonials.json` and add an object to the array:

```json
{
  "firstName": "Jane",
  "lastName": "Doe",
  "company": "Acme Corp",
  "title": "VP of Customer Experience",
  "comment": "Rebecca led our post-purchase CX overhaul end to end...",
  "ratings": { "relations": 5, "execution": 5, "communication": 4 },
  "date": "2026-09-01"
}
```

- `ratings` values are 1–5.
- Order in the array = display order (newest first is typical — put new entries
  at the top).
- `date` isn't currently shown on the card, but keeping it lets you sort/curate
  later.

The wall shows a "No feedback yet" empty state until the array has at least one
entry — nothing fake is pre-loaded.

## Notes

- The star-rating inputs in the form are pure CSS (no JS needed for the UI
  itself); only the fetch/submit logic and the wall-rendering are scripted, both
  inline in `index.html`'s existing `<script>` block.
- All submitted content is HTML-escaped when rendered, so a comment can't break
  the page layout or inject markup.
- If you'd rather have entries appear live the moment someone submits — no manual
  copy-paste step — that requires swapping Formspree for a small database (e.g.
  Supabase) with a public read/insert policy. Ask if you want that built instead;
  it's a bigger change (needs a free Supabase account and a bit more setup).
