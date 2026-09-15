# Pack prototype — deploy and collect

One file, no build step, no backend. `index.html` is the whole thing.

---

## Step 1 — make somewhere for answers to land (10 min)

A Google Form with **one long-answer question**. That's all it needs — the prototype sends the entire session as a single block of text, so you never map thirty fields.

1. Create a form. Add one question, type **Paragraph**, call it `Session data`.
2. In the form editor, click the **⋮** menu (next to Send) → **Get pre-filled link**.
3. Type any placeholder into the question — `TEST` will do — then **Get link** → **Copy link**.
4. You'll have a URL like:
   `https://docs.google.com/forms/d/e/1FAIpQLSd.../viewform?usp=pp_url&entry.1847562093=TEST`
   Both values you need are in it: the long form id, and the `entry.` number.
5. Open `index.html` and fill in the config block near the top of the `<script>`:

```js
var FORM_ACTION = "https://docs.google.com/forms/d/e/XXXXXXXX/formResponse";
var FORM_FIELD  = "entry.1234567890";
```

Two things to get right: the action URL ends in **`formResponse`**, not `viewform` — change that last word by hand. And take the entry id on its own, without the `=TEST`.

6. Link a response spreadsheet from the form's Responses tab. Every submission appends a row.
7. Test it yourself before sending the link out: run through the prototype, tap **Send my answers**, and check a row appears in the sheet. A silent post gives no error if the field id is wrong, so verify once rather than discovering it after fifteen people.

**If Get pre-filled link isn't in the menu:** open the live form, right-click the answer box, choose **Inspect**, and read the `name` attribute off the `<textarea>` — `name="entry.1847562093"`. Searching the page source for `entry.` no longer works reliably; Google renders the fields with JavaScript now.

Testers now finish the prototype, tap **Send my answers**, and it posts silently without them ever leaving the page.

### Later, for testers without a VPN

Google is blocked on mainland connections. When you start testing local owners, make the equivalent form on 金数据 or 腾讯问卷 and use the other config line instead:

```js
var FORM_LINK = "https://jinshuju.net/f/XXXXXX?field_1=";
```

That opens their form with the session prefilled; the tester just presses submit. Works fine domestically.

If both are blank, sessions stay on the tester's device and they can copy the text to you manually. Nothing is ever lost.

---

## Step 2 — put it online (10 min)

**GitHub Pages** is the one to use. Free, permanent, and a public repo is one more thing in your portfolio.

```bash
git init
git add index.html README.md
git commit -m "Pack prototype v1"
git branch -M main
git remote add origin git@github.com:<you>/pack-prototype.git
git push -u origin main
```

Then: repo **Settings → Pages → Source: Deploy from a branch → main / (root)**. Your link appears within a minute or two at `https://<you>.github.io/pack-prototype/`.

Alternatives if you'd rather not use GitHub: Cloudflare Pages (direct upload, no git), or Netlify (drag the folder onto their deploy page). All three are reachable with a VPN and unreliable without one.

---

## Step 3 — send it

Paste the link into WeChat with a short message. Something like:

> I'm testing an idea for a Shanghai dog-owner app. It's a fake prototype — five minutes, and there are a few questions at the end. Brutal honesty appreciated. [link]

Don't explain the idea before they open it. What they do without your explanation is the data.

---

## Reading the results

- **Remote testers** → rows in your Google Sheet, one per session.
- **In-person sessions on your own phone** → triple-tap the "Pack." wordmark to open the host view, then **Copy as CSV** for a proper spreadsheet, or **Copy as text** to read through.

The host view also shows a running tally — how many would make a profile, turn up, invite a friend, or pay.

---

## Changing the prototype

Everything lives in `index.html`:

- `PEOPLE` — the 14 fake Shanghai profiles. Swap them freely; nothing else depends on them.
- `score()` — the matching formula (30% dog compatibility, 25% distance, 20% interests, 15% activity, 10% schedule). Change the weights and the match screen updates itself, including the breakdown bars.
- `QUESTIONS` — the end-of-session questions. Adding one automatically adds a CSV column.
- `VENUES`, `GROUPS` — the suggested places and the small-group listings.

Change one thing at a time between batches of testers, or you won't know what caused the difference.
