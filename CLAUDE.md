# Website Design Recreation

## Workflow

When the user provides a brand template PDF, desktop and/or mobile reference screenshots, and a text prompt describing the website:

1. **Generate** a single `index.html` file using Tailwind CSS (via CDN). Include all content inline — no external files unless requested.
2. **Screenshot** the rendered page using Puppeteer (`npx puppeteer screenshot index.html --fullpage` or equivalent). If the page has distinct sections, capture those individually too. If a mobile reference was provided, also capture a mobile-viewport screenshot (375px wide).
3. **Compare** your screenshot against the reference image. Check for mismatches in:
   - Spacing and padding (measure in px)
   - Font sizes, weights, and line heights
   - Colors (exact hex values)
   - Alignment and positioning
   - Border radii, shadows, and effects
   - Responsive behavior
   - Image/icon sizing and placement
4. **Fix** every mismatch found. Edit the HTML/Tailwind code.
5. **Re-screenshot** and compare again.
6. **Repeat** steps 3–5 until the result is within ~2–3px of the reference everywhere.

Do NOT stop after one pass. Always do at least 2 comparison rounds. Only stop when the user says so or when no visible differences remain.

## Mobile Version Workflow

When the user provides both a desktop screenshot **and** a mobile screenshot:

1. **Implement both layouts** in the same `index.html` using Tailwind responsive prefixes (`sm:`, `md:`, `lg:`, etc.). The desktop layout applies at `md:` and above; the mobile layout applies below `md:`.
2. **Screenshot desktop** at full width (default Puppeteer viewport, typically 1280px).
3. **Screenshot mobile** at 375px wide (iPhone-sized). Use Puppeteer's `--viewport` flag or equivalent to set width=375, height=812.
4. **Compare both screenshots** against their respective reference images independently. Note mismatches for each breakpoint separately.
5. **Fix mismatches** for both layouts. Changes to shared markup must not break either breakpoint — use responsive prefixes to scope fixes.
6. **Re-screenshot both viewports** and compare again.
7. **Repeat** until both desktop and mobile match their references within ~2–3px.

Key things to check for mobile specifically:
- Navigation collapses to a hamburger menu or stacked links if shown that way in the reference
- Single-column layout where the desktop shows multi-column
- Font sizes, button sizes, and tap target sizes match the mobile reference
- Images resize or reflow correctly
- Padding/margin adjustments for smaller screens

## Phase 1 Confirmation

Once both desktop and mobile layouts match their references within ~2–3px, **pause and confirm** with the user:

> "The layout matches the reference. Ready to apply the brand kit and content?"

Do NOT proceed to Phase 2 until the user confirms.

## Phase 2: Brand & Content

Using the matched `index.html` as a template (do not change layout or structure):

1. **Apply brand colors** — replace all colors with the exact hex values from the brand PDF.
2. **Apply brand typography** — update font families, weights, and sizes to match the brand PDF. Import fonts via Google Fonts or CDN if needed.
3. **Update text copy** — rewrite all headings, body text, labels, and CTAs to match the website description from the prompt. Keep the same structure and hierarchy.
4. **Update button styles** — apply brand colors and typography to all interactive elements.
5. **Screenshot** both desktop and mobile viewports again to verify the brand changes look correct.
6. **Be ready for minor adjustments** — after confirming with the user, make any small tweaks requested.

Rules for Phase 2:
- Do NOT change layout, spacing, or structure — only colors, fonts, and text content
- Use exact hex values from the brand PDF, not approximations
- Keep all placeholder images (`placehold.co`) unless the user provides real assets
- Do not invent content not described in the prompt

## Technical Defaults

- Use Tailwind CSS via CDN (`<script src="https://cdn.tailwindcss.com"></script>`)
- Use placeholder images from `https://placehold.co/` when source images aren't provided
- Mobile-first responsive design
- Single `index.html` file unless the user requests otherwise

## CSS Style Reference

The user may provide CSS code extracted directly from the original website to aid in accurately copying styles. When this is provided:

- **Treat it as the source of truth** for colors, spacing, fonts, borders, shadows, and any other styles it covers
- **Extract exact values** (hex codes, px/rem values, font names, etc.) and apply them directly — do not approximate
- Use it to resolve any ambiguity between the reference screenshot and your implementation
- If CSS variables are used (e.g., `--color-primary`), resolve them to their actual values before applying

## Rules

- Do not add features, sections, or content not present in the reference image
- Match the reference exactly — do not "improve" the design
- If the user provides CSS classes or style tokens, use them verbatim
- Keep code clean but don't over-abstract — inline Tailwind classes are fine
- When comparing screenshots, be specific about what's wrong (e.g., "heading is 32px but reference shows ~24px", "gap between cards is 16px but should be 24px")
