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

## One-time setup — done ✅

The form is already connected to your Formspree endpoint
(`https://formspree.io/f/mkjnwelb`), so submissions land straight in your
Formspree dashboard and inbox. Nothing left to configure.

If you ever need to reconnect it to a different Formspree form, open
`index.html` and update the `action` attribute on this line:
```html
<form id="feedbackForm" class="feedback-form" action="https://formspree.io/f/mkjnwelb" method="POST">
```

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
  "linkedin": "https://www.linkedin.com/in/jane-doe/",
  "date": "2026-09-01"
}
```

- `ratings` values are 1–5.
- `linkedin` is optional — if present, a small "LinkedIn ↗" link shows under the
  person's name/title on their card, opening in a new tab. The email address
  from the form submission is never published on the wall (there's no field for
  it in this schema) — if you want a person's card to link out, add their
  LinkedIn URL here instead of an email.
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
