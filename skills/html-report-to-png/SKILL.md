---
name: html-report-to-png
description: Build a polished single-file HTML report and export it to a tight PNG using local Chrome + Playwright, with verification steps and root-safe flags.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [html, png, screenshot, report, playwright, chrome, export]
---

# HTML Report to PNG

Use this when the user wants a polished HTML/H5 page, poster, dashboard, or long-form report exported as a PNG image.

## When to use
- "Make an HTML page and export as PNG"
- "Generate a visual report / poster / long image"
- "Summarize data into a shareable infographic"
- Any task where the final deliverable should be an image, but HTML/CSS is the easiest way to design it

## Proven workflow

### 0) For recurring reports, produce a trial run before scheduling
When the user asks for a daily/recurring visual report or H5 screenshot delivery, do a one-off trial first unless they explicitly ask to schedule immediately. Generate sample pages, send the actual PNGs, and wait for approval on layout, content density, and project/item selection before creating the cron job. If a cron job was created prematurely, remove or pause it before the trial so the user can validate the format.

### 1) Gather source data first
Fetch and structure the content before designing.
Examples:
- Trending pages, rankings, metrics, summaries
- Use execute_code or terminal to parse HTML into structured JSON
- Derive key takeaways (top items, totals, category counts)

### 2) Use a design system skill if relevant
If the page should look polished, load a matching design skill such as `popular-web-designs` and optionally a template like `templates/stripe.md`.

### 3) Write a self-contained HTML file
Create a single `index.html` with inline CSS for portability.
Recommended pattern:
- Use `write_file`
- Build card-based sections with clear hierarchy
- Prefer fixed content width for image export (for example 1400–1440px)
- Use generous border radius, shadows, and background gradients for report-style visuals

Suggested output dir:
- `/tmp/<project-name>/index.html`

### 4) Do NOT rely on raw Chrome `--screenshot` first for long pages
In this environment, directly using:
- `google-chrome --headless --no-sandbox --screenshot ... --window-size=...`
often works, but can produce an oversized image with large blank space at the bottom when the chosen window height exceeds real content height.

This happened during a GitHub Trending report export:
- Raw Chrome screenshot result: `2880 x 7200`
- Large unnecessary bottom whitespace

### 5) Prefer Playwright `fullPage: true`
Use Playwright with the system Chrome executable for tighter output.
Create a small JS script like:

```js
const { chromium } = require('/root/.hermes/hermes-agent/node_modules/playwright');

(async() => {
  const browser = await chromium.launch({
    headless: true,
    executablePath: '/usr/bin/google-chrome',
    args: ['--no-sandbox', '--disable-gpu']
  });

  const page = await browser.newPage({
    viewport: { width: 1440, height: 1200 },
    deviceScaleFactor: 2
  });

  await page.goto('file:///tmp/your-report/index.html', { waitUntil: 'networkidle' });
  await page.screenshot({ path: '/tmp/your-report/output.png', fullPage: true });
  await browser.close();
})();
```

Then run:

```bash
node /tmp/your-report/capture.js
```

Why this is preferred:
- `fullPage: true` captures the actual document height
- Avoids manual guesswork for `--window-size` height
- Produces a tighter final PNG

In the GitHub Trending task, this reduced the image from:
- `2880 x 7200` → `2880 x 5390`

## Environment-specific findings

### Root + Chrome
When running Chrome headless as root, you MUST pass:
- `--no-sandbox`

Without it, Chrome fails with:
- `Running as root without --no-sandbox is not supported`

### Available tools on this machine
Validate the installed paths before capturing rather than assuming a fixed Hermes package layout:

```bash
command -v google-chrome
node -e "console.log(require.resolve('playwright'))"
```

Chrome is available at `/usr/bin/google-chrome`. Require Playwright using the path returned by `require.resolve('playwright')` or the containing package directory; the active package location can differ across Hermes installs. Node is available locally.

## Verification checklist

### 0) Check screenshot dimensions and page count
For multi-page H5 deliverables, verify every expected PNG exists and has the intended dimensions before delivery.

Example:
```bash
file /tmp/your-report/*.png
```

### 1) Check file dimensions
Use `file output.png` to confirm the PNG exists and inspect resolution.

Example:
```bash
file /tmp/your-report/output.png
```

### 2) Vision QA the exported image
Use `vision_analyze` and ask specifically:
- Is the PNG complete and readable?
- Any obvious truncation, overlap,乱码, style loss, or excessive blank space?
- Is it suitable to send directly to the user?

This catches:
- Unexpected whitespace
- Missing styles
- Cropping issues
- Overlapping cards

## Recommended delivery pattern
Return:
- The PNG via `MEDIA:/path/to/output.png`
- A short textual summary of the report’s findings
- Optionally mention the local HTML path if useful

## Pitfalls
- For recurring visual reports, validate a sample run with real screenshots before creating or enabling the schedule
- In fixed-height poster exports such as 1080×1920, inspect footer placement; browser screenshots can reveal bottom text clipped by the viewport even when the HTML looks visually balanced
- Do not assume raw Chrome screenshots are tightly cropped for long pages
- Do not forget `--no-sandbox` when Chrome runs as root
- Do not skip visual verification after export
- If the user wants a polished design, don’t stop at a plain HTML table—apply a design system and summary layout

## Minimal checklist
1. Fetch/parse source data
2. Summarize key takeaways
3. Generate polished self-contained HTML
4. Export with Playwright `fullPage: true`
5. Verify with `vision_analyze`
6. Send PNG to user
