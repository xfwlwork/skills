---
name: html-report-to-png
description: "Use when exporting an HTML report, poster, dashboard, or long page to a verified PNG."
version: 1.1.0
author: Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [html, png, screenshot, report, playwright, chrome, export]
---

# HTML Report to PNG

Create a polished HTML artifact and export it to a PNG with browser rendering. Use this workflow for reports, posters, dashboards, infographics, long-form pages, and H5-style visual deliverables.

The skill is platform-neutral. It does not require Hermes, a particular project layout, a design skill, or a specific visual style.

## Requirements

- An HTML file, preferably self-contained with inline CSS.
- A JavaScript runtime such as Node.js.
- Playwright or an equivalent browser automation library.
- A Chromium-based browser, normally Chrome or Chromium.
- Optional: a vision or image inspection tool for visual QA.

Check the local environment before writing the capture script:

```bash
node --version
node -e "console.log(require.resolve('playwright'))"
command -v google-chrome || command -v chromium || command -v chromium-browser
```

If Playwright or a browser is unavailable, install them through the host project's package manager or use an available browser automation tool. Do not assume a fixed global path.

## Workflow

### 1. Gather and structure source data

Retrieve and verify the source data before designing the page.

- Parse source pages, feeds, APIs, or local files first.
- Preserve source URLs and attribution when external material is used.
- Derive key takeaways, totals, categories, and other values programmatically.
- Keep source data separate from presentation code when the report is generated repeatedly.

### 2. Choose the visual direction

Follow the user's brand guide, design system, or stated visual direction. The rendering workflow works with any HTML/CSS style.

For a portable artifact, prefer a single HTML file with inline CSS and only use external assets when the target environment can access them.

### 3. Build the HTML artifact

Recommended practices:

- Use a stable content width suitable for the output, often 1200–1600 CSS pixels.
- Define print/export colors explicitly.
- Use semantic sections and headings.
- Give tables, cards, charts, images, and videos enough space to render clearly.
- Add responsive behavior for narrow screens when the HTML will also be viewed interactively.
- For long pages, use consistent section spacing and avoid layout elements that depend on viewport height.

Suggested layout:

```text
project/
├── index.html
├── assets/
└── capture.mjs
```

### 4. Capture with Playwright

Resolve the Playwright package and browser executable for the current machine. The following is a portable example; replace the import and executable path with values discovered in the environment.

```js
import { chromium } from 'playwright';

const browser = await chromium.launch({
  headless: true,
  // Add '--no-sandbox' only when the browser runs as a privileged user
  // and the local browser requires it.
  args: process.getuid?.() === 0 ? ['--no-sandbox', '--disable-gpu'] : ['--disable-gpu']
});

const page = await browser.newPage({
  viewport: { width: 1440, height: 1200 },
  deviceScaleFactor: 2
});

await page.goto('file:///absolute/path/to/index.html', {
  waitUntil: 'domcontentloaded'
});
await page.waitForLoadState('networkidle').catch(() => {});
await page.evaluate(async () => {
  if (document.fonts?.ready) await document.fonts.ready;
  await Promise.all([...document.images].map(img => img.complete
    ? Promise.resolve()
    : new Promise(resolve => { img.addEventListener('load', resolve); img.addEventListener('error', resolve); })
  ));
});
await page.screenshot({ path: 'output/report.png', fullPage: true });
await browser.close();
```

`fullPage: true` captures the document's actual height and avoids the large blank area that can result from guessing a window height for long pages.

For network-hosted media, allow enough time for loading and inspect failed requests. For reproducible reports, download or vendor important assets locally.

### 5. Verify the output

Run a file and dimension check:

```bash
file output/report.png
```

For automated checks, verify that:

- the PNG exists and is non-empty;
- its dimensions match the intended width and expected content height;
- the page was not truncated;
- fonts, images, charts, and SVGs loaded;
- there is no unexpected blank area at the bottom;
- no cards, tables, labels, or diagrams overlap;
- external links and media have source attribution where required.

Use a vision or image inspection tool when available. Ask it to inspect readability, cropping, overlap, missing assets, excessive whitespace, and mobile/desktop layout quality.

### 6. Deliver both formats when useful

Return the HTML source for editing and the PNG for sharing. For recurring reports, preserve the capture script and document the runtime requirements.

## Common pitfalls

- A raw browser screenshot with an arbitrary height can create large bottom whitespace.
- External fonts and media may fail in offline or sandboxed environments.
- Root or otherwise privileged browser sessions may require `--no-sandbox`; use that flag only when required by the local browser setup.
- A screenshot can succeed while remote images silently fail, so inspect the rendered page.
- Long SVGs or fixed-height containers can be clipped; test the exported dimensions.
- A visually polished page still needs data validation and source attribution.
- For a recurring visual report, make and inspect a trial export before scheduling repeated runs.

## Minimal checklist

1. Retrieve and verify source data.
2. Create the HTML artifact in the requested visual style.
3. Discover the local Playwright and browser paths.
4. Wait for fonts and media before capturing.
5. Export with `fullPage: true`.
6. Check dimensions and perform visual QA.
7. Deliver the HTML and PNG paths.
