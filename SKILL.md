# SKILL: Update the Her Rising Worth website

Instructions for any Claude (or human) making changes to herrisingworth.com.

## What this site is
- Marketing site for Her Rising Worth — neuroscience-informed recovery coaching for women healing from toxic relationships. Owner: Kourtenay (startophealth@gmail.com).
- One page, one file: `index.html`. No build step, no framework, no CMS.

## Where it lives
- **Repo:** `github.com/startophealth/her-rising-worth`, branch `main`
- **Hosting:** GitHub Pages (Settings → Pages → Deploy from branch → `main` / root)
- **Live URLs:** https://herrisingworth.com (custom domain via `CNAME` file in repo) and https://startophealth.github.io/her-rising-worth/
- **DNS:** Cloudflare. Root `@` → four A records `185.199.108.153 / .109.153 / .110.153 / .111.153`; `www` → CNAME `startophealth.github.io`. Records set to "DNS only" (grey cloud).

## How to deploy ("go live")
1. Edit `index.html` locally / in the working project.
2. Push the complete file to `main` as `index.html` (e.g. GitHub API push, `git push`, or the GitHub web editor). Use a descriptive commit message.
3. GitHub Pages rebuilds automatically in ~1 minute. Changes are NOT live until this push happens.
4. Never delete or modify: `CNAME` (custom domain binding), `portrait.b64.a.txt`, `portrait.b64.b.txt` (see below).

## Hard constraints on index.html
- Single standalone HTML file: **inline CSS in one `<style>` block, vanilla JS in `<script>` blocks, Google Fonts CDN only**. No frameworks, no external CSS/JS files, no localStorage.
- Fonts: Cormorant Garamond (headings) + Inter (body).
- Palette (CSS vars in `:root`): sage `#8A9B7E`, terracotta `#C17A5A`, cream `#F5F0E8`, dark `#3A3630`, sage-soft `#EDF0EA`.
- Keep the safety disclaimer (crisis/DV hotline block near the footer) — legally/ethically required, do not remove.
- Copy tone: warm, validating, science-grounded. IMPORTANT legal nuance: the owner is NOT practicing counseling through this business. Never phrase credentials as active practice — use "trained in psychology & counseling", "draws on counseling training", past-tense biography ("I spent 8 years as a mental health counselor"). Do not write "licensed", "therapist", or anything implying therapy services.

## About photo (special mechanism)
The portrait in the About section is NOT an <img src="file.jpg">. To keep pushes small and reliable, the WebP image is base64-encoded and split across two text files in the repo root:
- `portrait.b64.a.txt` + `portrait.b64.b.txt`
- A script in index.html fetches both, concatenates, strips whitespace, and sets `#portraitImg.src` as a data URI.
If replacing the photo: encode new image as base64 WebP (~60-70KB max), split roughly in half at a clean boundary, overwrite both .txt files (line breaks are fine — the loader strips whitespace). Keep the element id `portraitImg`.

## Integrations (don't break these)
### Cal.com booking modal
- Event: `herrisingworth/90-minute-clarity-session` ($150, 90 min).
- All "Book" buttons are `<a>` tags with `data-cal-link`, `data-cal-namespace="90-minute-clarity-session"`, `data-cal-config`, AND an href to the cal.com page as no-JS fallback.
- A document-level click handler calls `preventDefault()` on `a[data-cal-link]` — required because Safari otherwise opens the href in a new tab alongside the modal. Keep it.
- New booking buttons: copy the exact attribute set from an existing one.

### MailerLite quiz capture
- The 5-question quiz ("What's your self-worth foundation?") scores 0–10 → three results: Survival Mode (0–3), The In-Between (4–7), Ready to Rise (8–10).
- On submit, name+email POST to a MailerLite embedded-form endpoint per result (account 2496746):
  - Survival Mode → form `192397034699883992`
  - The In-Between → form `192397134865106904`
  - Ready to Rise → form `192397193626256736`
- Endpoints look like `https://assets.mailerlite.com/jsonp/2496746/forms/<ID>/subscribe`, posted as FormData with `fields[name]`, `fields[email]`, `ml-submit=1`, `anticsrf=true`.
- Automations/result emails live inside MailerLite (owner manages). Don't test-submit with fake emails — it creates real subscribers.

## Design system notes
- Sections alternate cream / sage / sage-soft backgrounds, joined by curved SVG dividers (`.curve`) — when reordering sections, update each divider's background color (the outgoing section) and path fill (the incoming section) so the curves stay seamless.
- Scroll-reveal: elements get `.rev` and an IntersectionObserver adds `.rev-in`. Respects `prefers-reduced-motion`.
- `html,body{overflow-x:clip}` guards against horizontal scroll from decorative blobs — keep it.
- Quiz box intentionally overlaps into the next section (negative margin + `#offers{padding-top:170px}`).

## Contact / accounts
- Email everywhere on site: startophealth@gmail.com
- Domain registrar/DNS: Cloudflare (owner's account). GitHub: `startophealth`. MailerLite account: 2496746. Cal.com: `herrisingworth`.
