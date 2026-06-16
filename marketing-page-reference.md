# glibcode.com — Marketing Page Reference

Reference doc for building the glibcode.com marketing page. Pulls visual primitives from the glib-code codebase and locks layout decisions.

## What this page is

Single static page. Dead-center layout. Logo, tagline, three buttons (Download, GitHub, Docs), minimal footer. The page the Twitter launch sequence points at. Job is confirmation, not conversion.

## Stack

- **Astro** — empty starter (`npm create astro@latest`, no template)
- **Cloudflare Pages** — deploy target
- **Tailwind** — matches glib-code's styling approach
- No Vue, no component library import, no CMS

## Theme — minimal dark

Pure grayscale, no accent color. Pulled from glib-code's `minimal-dark` preset in `shared/src/theme/presets.ts`. HSL space-separated values, applied as CSS custom properties.

```css
:root {
  --background: 0 0% 9%;        /* #171717 */
  --foreground: 0 0% 95%;       /* #f2f2f2 */
  --primary: 0 0% 70%;
  --primary-foreground: 0 0% 9%;
  --muted: 0 0% 15%;
  --muted-foreground: 0 0% 60%;
  --border: 0 0% 20%;
  --ring: 0 0% 70%;
}
```

Tailwind config maps these to semantic names (`background`, `foreground`, `primary`, `muted`, `border`, `ring`) the same way glib-code does. Use `bg-background`, `text-foreground`, etc.

## Fonts

DM Sans for everything UI. System mono stack if any code-like text is needed (probably not for this page).

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
```

```css
body {
  font-family: 'DM Sans', system-ui, sans-serif;
}

.mono {
  font-family: 'SF Mono', Consolas, Menlo, monospace;
}
```

## Global styles

Match glib-code's baseline:

```css
body {
  background: hsl(var(--background));
  color: hsl(var(--foreground));
  -webkit-font-smoothing: antialiased;
  text-rendering: optimizeLegibility;
}

::selection {
  background: hsl(var(--primary) / 0.35);
}
```

## Button styling

All three buttons use the `default` variant (filled). Translated from glib-code's cva button to plain Tailwind:

```html
<a class="inline-flex items-center justify-center h-9 px-4 rounded-md
          bg-primary/90 text-primary-foreground font-medium text-sm
          hover:bg-primary
          focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background
          transition-colors">
  Download
</a>
```

Same component for all three. Same visual weight. No hierarchy via variant — they sit as a row of equal CTAs.

## Logo

Use `glibcode-wordmark.png` from glib-code's assets. PNG only, no SVG. Copy the highest-res version into the marketing repo's assets folder. Display sized appropriately — let it scale down from source rather than up.

```html
<img src="/glibcode-wordmark.png" alt="glib-code" class="h-12 md:h-16" />
```

## Layout

```
                    [wordmark]

                 [one-line tagline]

       [Download]  [GitHub]  [Docs]



                                          [footer]
```

- Full-viewport flex centered both axes
- Logo, tagline, buttons stacked with consistent gap
- Footer absolutely positioned at bottom, minimal

```html
<main class="min-h-screen flex flex-col items-center justify-center gap-8 p-8">
  <img src="/glibcode-wordmark.png" alt="glib-code" class="h-12 md:h-16" />
  <p class="text-muted-foreground text-lg">{TAGLINE}</p>
  <div class="flex gap-3">
    <a class="...">Download</a>
    <a class="...">GitHub</a>
    <a class="...">Docs</a>
  </div>
</main>

<footer class="fixed bottom-0 left-0 right-0 p-6 text-sm text-muted-foreground flex justify-between">
  <span>© 2026 glib-code</span>
  <a href="https://twitter.com/Jackhortonlol">@Jackhortonlol</a>
</footer>
```

## Open items

- **Tagline** — TBD, must match the Twitter launch pitch
- **Download button behavior** — depends on whether the Windows binary is ready at launch. If yes: link to binary. If no: "Coming soon" or email capture, same visual position.
- **Docs URL** — placeholder fine at launch
- **GitHub URL** — `https://github.com/cloudboy-jh/glib-code` (or wherever the repo lives publicly)

## Out of scope

- Demo video embed (lives in the launch tweet)
- Feature list / marketing copy / screenshots
- Scroll-down sections
- Pricing, blog, changelog
- Analytics beyond Cloudflare built-in
- OS detection on the Download button
