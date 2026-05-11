# CLAUDE.md — Seth Looper Portfolio (seth-looper repo)

Single-page static portfolio at `https://looperworks.github.io/seth-looper/`. Built specifically for Seth's application to the **Dartmouth Design Lab Instructor** role (reports to Prof Beth Ames Eagle / DIAD). The live `sethlooper.com` domain still serves the *old* serif design from a separate `main` branch on a different repo — this site is the redesign.

## Where it lives

- **GitHub**: `looperworks/seth-looper` (public)
- **Deploy URL**: `https://looperworks.github.io/seth-looper/` (the account has `thresholdarch.com` set as a verified custom domain at the user level, so this URL 301-redirects to `https://thresholdarch.com/seth-looper/` — both serve the same site)
- **Pages source**: `main` branch / root
- **Local working branch**: `claude/priceless-feynman-c6a997` (push as `main` to the `seth-looper` remote)
- **Push remote**: `seth-looper` → `https://github.com/looperworks/seth-looper.git`

## File map

- `index.html` — the entire site (HTML + inline CSS + inline JS)
- `images/gallery/*.webp` — every project carousel image, all 2500px wide max, 85% quality
- `images/*.jpg` — older site assets (some still referenced in Tools/Teaching previews)
- `sitemap.xml`, `robots.txt` — both point at `https://looperworks.github.io/seth-looper/`
- `favicon-*.png`, `favicon.svg`, `apple-touch-icon.png` — favicons; **referenced with relative paths** (`href="favicon.ico"`, not `/favicon.ico`) so they work under the `/seth-looper/` subpath
- **No CNAME file** — deliberately removed so this repo doesn't try to claim `sethlooper.com`

## Voice / writing rules (load-bearing — recruiters will read this)

The user's authentic prose (the Hero bio, the Threshold "Space for the In-Between" copy) uses these patterns; my rewrites must match.

1. **Zero em dashes in user-facing prose.** Use commas, periods, semicolons, parentheses, colons. The hero bio and the Threshold statement both have 0 em dashes. Statements I drafted previously averaged 1.7 per 100 words and read AI-y. After a full purge they are now 0.
2. **First-person voice where it fits.** "I prototype", "I built", "We started", "I wanted X to argue for…" Not "Mr. Looper has experience prototyping."
3. **No AI cliché vocabulary**: delve, tapestry, robust, seamless, leverage, harness, elevate, unlock, foster, synergy, holistic, transformative, cutting-edge, navigate the complex, realm of, journey.
4. **No AI rhetorical patterns**: "not only X but also Y", "isn't just X, it's Y", "a wide range of", "it's worth noting", "plays a crucial role".
5. **Concrete numbers over adjectives**. "Mentored 30+ consultants" beats "Mentored a diverse team."
6. **Argument-driven closers.** The bio's signature is naming the argument: *"Students learn to make by making, and to think by iterating."* Project statements often close the same way ("the argument is structural…", "the argument is a quiet one about scope…").

If you rewrite anything, run the em-dash + AI-tell scan from the audit script pattern (see "Audit utility" section at end).

## Page structure

```
Hero (no title text — name only appears in nav)
├─ Two-column block-text: ~3+3 paragraphs of bio
Making (#making, 6 blocks)
├─ Roofscape Haus
├─ Micro Housing II
├─ Lens Bridge & Urban Deck
├─ Regular Randomness
├─ Nodes Puzzle
└─ Urban Blossom — Hydrology, Shanghai
Tools (#tools, 5 blocks — order matters)
├─ Dialogue by Design (in development; no carousel)
├─ DartWorld
├─ Narrative by Design
├─ Threshold (free public toolkit; thresholdarch.com)
└─ Synapse
Teaching (#teaching, 1 block)
└─ Portfolio as Narrative @ Kent State (ARCH 66995, Spring 2026)
Contact (footer, no nav anchor)
```

Floating nav (right column, sticky): just "Seth Looper" (the brand mark, underlined) + an "INDEX" label + numbered links 01 Making / 02 Tools / 03 Teaching. **No Contact link** — kept the nav minimal.

## Block pattern (Olga Pipnik–style)

Every project block follows this exact structure:

```html
<article class="block">
  <div class="block-images carousel">
    <figure><img src="images/gallery/PROJECT-NN.webp" alt="..."></figure>
    ... more figures, all 2500w max webp ...
  </div>
  <div class="block-caption">
    <div class="meta-item">
      <span class="meta-label">Project:</span>Project Name
      <details class="block-statement">
        <summary>Project statement</summary>
        <p>...2–3 paragraphs of statement...</p>
      </details>
    </div>
    <div class="meta-item"><span class="meta-label">Keywords:</span>Comma, Separated, Tags</div>
    <div class="meta-item"><span class="meta-label">Tools:</span>Comma, Separated, Tools</div>
  </div>
</article>
```

The first block in each section gets `id="making"` / `id="tools"` / `id="teaching"` for anchor links.

### Notable patterns
- **Tools blocks** put a clickable project link in the Project name (e.g., `<a href="https://thresholdarch.com/">thresholdarch.com →</a>`), sometimes two stacked links separated by `<br>`.
- **Architecture projects (Roofscape Haus, Micro Housing II, Lens Bridge & Urban Deck)** add a stacked credit line: `<br>in collaboration with <strong>Axi:Ome</strong>` — matches the resume's "Selected Projects (in collaboration with Axi:Ome)" framing.
- **Statements are always collapsed by default** behind `<summary>Project statement</summary>` for image-first browsing.

## Carousel mechanics

- Images: WebP, max 2500px wide, ~85% quality. Source PNGs typically 1600×1000 or 3000+ px wide; converted with `cwebp -q 85 -resize 2500 0`.
- Bleed: each carousel is `width: 100vw` with `margin-left: -32px` and inner `padding: 32px` so the first image aligns with the body's left content edge.
- Scroll arrows: JS at the bottom of the file injects sticky `←` `→` buttons into every `.block-images.carousel`. Don't hand-edit them.

## Deploy workflow

This is a one-line workflow after any commit:

```bash
git push seth-looper claude/priceless-feynman-c6a997:main
```

Pages rebuilds in ~30–60 seconds. To wait for the build to finish before testing:

```bash
until [ "$(gh api repos/looperworks/seth-looper/pages/builds/latest --jq .status 2>/dev/null)" = "built" ]; do sleep 5; done
```

## Audit utility

Before any major prose change ships, re-run an em-dash + AI-tell scan. The pattern (used during this redesign):

```python
import re
text = open('index.html').read()
for marker_match in re.finditer(r'<!-- (?:MAKING|TOOLS|TEACHING) — ([^>]+?) -->', text):
    # extract each <details class="block-statement"> body
    # count em dashes — target is 0 per statement
    # scan for AI tells from the list above
```

The user's voice has zero em dashes in user-facing prose. Anything above 0.5 per 100 words is a red flag.

## Open items / future passes

- The Hero column 3 is empty (grid is `auto auto 1fr`). Could host a "currently" status line, a portrait, or contact info — left open intentionally.
- Resume PDF is at `~/Desktop/Dartmouth Design Lab Application/Looper_Seth_Resume_DesignLabInstructor.pdf` — not embedded in the site (deliberate, user sends it separately).
- The `Documentaries` block (Vimeo educational films) was deleted earlier in the redesign. If the page ever needs more "strong written/verbal communication" evidence for this JD, the Vimeo embeds were strong.
- `urban-blossom` project doesn't have a confirmed year in the data; intentionally left out of the meta row.
- The Dartmouth application materials live separately in `~/Desktop/Dartmouth Design Lab Application/` (cover-letter.md, resume.md, the PDF resume).
