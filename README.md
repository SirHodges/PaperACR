# ACR Training Tools

A small suite of web-based tools to help paramedics complete the Ontario **Ambulance Call Report (ACR)** accurately. Built for local use (Service No. **725**, Station **002**, CACC/ACS **940**, Base Hospital **BH02**, Ottawa-area receiving facilities).

The site is plain HTML/CSS/JavaScript — each page is a single self-contained file with everything baked in, so there is **no build step** required to edit or deploy. Just edit the file and push.

> **Live site:** _add your Cloudflare Pages URL here_

---

## What's in here

| File | What it is |
|------|------------|
| `index.html` | **Code Lookup** — the home page. Search all 546 ACR codes, build a transcription list, plus Quick reference and Other Tools. |
| `form-guide.html` | **Form Guide** — an interactive, zoomable, annotated ACR. Tap any section for an explanation. |
| `FAQ.html` | **FAQ** — common questions about completing the ACR. |

All three are cross-linked, so a user can move between them from the buttons at the top of each page.

> ⚠️ **Filenames are case-sensitive.** Cloudflare serves them exactly as named, and the cross-links expect `index.html`, `form-guide.html`, and `FAQ.html`. Don't rename or change the capitalisation.

---

## The tools

### Code Lookup (`index.html`)
- Real-time search across **546 codes** with ranked results and match highlighting.
- **★ Common** filter for the codes used on most calls.
- **Selection queue** — tap codes to build a transcription list, reorder or remove them, with a Clear/Undo safety net.
- **News banner** at the top that rotates through short announcements.
- Toolbar: **Form guide**, **FAQ** (with an "Updated" flash when recently changed), **Quick reference** (service/station/dispatch info, plus a collapsible **Hospital codes** list), and **Other Tools** (Ministry approved abbreviations, Submit a question, and reserved slots for future tools).

### Form Guide (`form-guide.html`)
- Pan/zoom view of the ACR with **Front**, **Back**, and **Refusal (code 72)** tabs.
- Tappable hotspots over each section open a plain-language explanation.
- The **Trauma** section includes a quick reference for the Location / Type / Mechanism codes.

### FAQ (`FAQ.html`)
- Tap-to-expand questions and answers covering form completion (running out of space, call numbers, Chief Complaint, problem codes) and current program notes.

---

## Hosting & deployment

The repo is connected to **Cloudflare Pages**. When you push a change to the repo, Cloudflare automatically rebuilds and deploys the site — usually within a minute. There's nothing to compile.

A few practical notes:
- The free plan allows **500 builds/month** (one build ≈ one push). You'll never come close, but batching several edits into a single push keeps the count low.
- Bandwidth is effectively unlimited for a site this size.
- After an update, give it a minute and refresh; a hard refresh (Ctrl/Cmd + Shift + R) clears your own browser cache if you want to see the change immediately.

---

## How to make common edits

Everything below is edited **directly in the HTML files** — no tools needed.

### Change the news banner messages
In `index.html`, near the top of the `<script>` block, find the `NEWS` list and edit the lines:

```js
const NEWS = [
  "All Fields on Trip sheet need to be filled out (Except UTM)",
  "Your new message here",
];
const NEWS_ROTATE_MS = 5000;   // milliseconds each message stays on screen
```

- Add, remove, or reorder lines freely. With one message it sits still; with several it cross-fades.
- Each line is treated as **HTML**, so you can include a link, e.g. an email:
  `"Email <a href='mailto:David.Hodges@Ottawa.ca'>David.Hodges@Ottawa.ca</a>"`.
- Because lines are HTML, write any literal `<`, `>`, or `&` as `&lt;`, `&gt;`, `&amp;`.

### Update the FAQ "Updated" badge
When you change the FAQ, update the date on the FAQ button in `index.html` so the amber "Updated" flash appears (it shows for 7 days, then goes quiet):

```html
<a class="tbtn primary faq" href="FAQ.html" id="faqBtn" data-updated="2026-06-11">
```

Use `YYYY-MM-DD` format with leading zeros (e.g. `2026-07-04`).

### Add a FAQ question
In `FAQ.html`, copy one `<details class="faq-item">` block and edit the question and answer text. There's a comment marking the spot.

### The "Now with hospital codes!" note
Driven by `data-moved="YYYY-MM-DD"` on the Quick reference button in `index.html`. It pulses for **14 days** then hides itself. Delete the attribute to remove it entirely.

### Add or change an Other Tools tile
In `index.html`, find the `openOtherTools()` function and edit the `opts` list. A `null` is a blank reserved slot; replace one with either a link tile or an action tile:

```js
{t:"Submit a question", sub:"External link to MS Forms", href:"https://..."},  // opens a link
{t:"Some tool", act:"abbrev"},                                                  // runs a built-in action
```

---

## Important notes

- **The Form Guide explanations are a first pass.** They were drafted from the form itself and general practice — **not** verified against the Ontario ACR Completion Manual or the BLS/ALS Patient Care Standards. Review them against authoritative sources before relying on them for official training.
- **Local codes** (facility destination numbers; Hospital Treatment during Offload Delay codes 3430–3434; PPE codes 408.x) are a local reference layer and are **not** part of the provincial code list. They're maintained here separately.

---

## Contact

Ideas to make these tools better? Email **David.Hodges@Ottawa.ca**.
