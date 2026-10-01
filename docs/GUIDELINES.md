# Engineering & Design Guidelines: KOLINN_DEV

## 1. Design System & Token Specifications

### Color System (Nord Theme Integration)

The UI uses two distinct color modes based on the Nord palette specification:

#### Nord Dark

* **Polar Night (Backgrounds & Structural Surfaces):** `#2e3440`, `#3b4252`, `#434c5e`, `#4c566a`
* **Snow Storm (Text & Active Foreground):** `#d8dee9`, `#e5e9f0`, `#eceff4`
* **Frost (Primary Accents & UI Focus):** `#8fbcbb`, `#88c0d0`, `#81a1c1`, `#5e81ac`
* **Aurora (Badges, Alerts & Status Highlights):** `#bf616a`, `#d08770`, `#ebcb8b`, `#a3be8c`, `#b48ead`

#### Nord Light

* **Polar Night (Text & Active Foreground):** `#4c566a`, `#3b4252`, `#2e3440`
* **Snow Storm (Backgrounds & Structural Surfaces):** `#eceff4`, `#e5e9f0`, `#d8dee9`, `#c5cbd3`
* **Frost (Primary Accents & UI Focus):** `#3f7a78`, `#1f6f8b`, `#2c5178`, `#2e436e`
* **Aurora (Badges, Alerts & Status Highlights):** `#a2333d`, `#a04d2b`, `#8f6e00`, `#4f7e44`, `#7a4d8a`

### Typography & Language Font Stacks

To guarantee clean rendering across both Latin script and Burmese Unicode text, the typography stack must enforce fallback cascades:

```css
:root {
  --font-heading: 'Fira Code', 'Noto Sans Myanmar', monospace;
  --font-body: 'Fira Sans', 'Padauk', 'Pyidaungsu', sans-serif;
  
  /* Fluid Typography Scaling */
  --font-size-sm: clamp(0.8rem, 0.17vw + 0.76rem, 0.89rem);
  --font-size-base: clamp(1rem, 0.34vw + 0.91rem, 1.19rem);
  --font-size-md: clamp(1.25rem, 0.61vw + 1.1rem, 1.58rem);
  --font-size-lg: clamp(1.56rem, 1vw + 1.31rem, 2.11rem);
  --font-size-xl: clamp(1.95rem, 1.56vw + 1.56rem, 2.81rem);
}

```

### Spacing & Grid System

* **8-Pixel Scaling System:** All padding, margins, and layout gaps must use explicit increments of 8px (`8px`, `16px`, `24px`, `32px`, `48px`, `64px`).
* **Container Max-Width Constraints:**
* Minimum supported view width: `320px`
* Maximum layout boundary width: `1440px`

**Card Grid Guidelines:** Content feeds and project card grids must not exceed 3 items per row on viewports up to `1440px`. Auto-fit dynamic layouts are required (`grid-template-columns: repeat(auto-fit, minmax(300px, 1fr))`).

## 2. Interactive Component Development Guidelines

### Mouse-Tracking Glowing Glass Card

* Cards must feature a terminal glass aesthetic using backdrop blur filters and subtle borders.
* Mouse tracking glow must be executed via standard browser events updating local CSS variables (`--mouse-x`, `--mouse-y`) without heavy JavaScript libraries:

```astro
<div class="glass-card" onmousemove="this.style.setProperty('--mouse-x', `${event.clientX - this.getBoundingClientRect().left}px`); this.style.setProperty('--mouse-y', `${event.clientY - this.getBoundingClientRect().top}px`);">
  <slot />
</div>

```

### Theme Switcher Scripting

* Dark/Light mode selection must be executed in `<head>` before page rendering to avoid layout shifts or white flashes:

```html
<script is:inline>
  const theme = (() => {
    if (typeof localStorage !== 'undefined' && localStorage.getItem('theme')) {
      return localStorage.getItem('theme');
    }
    return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
  })();
  document.documentElement.classList.toggle('dark', theme === 'dark');
</script>

```

## 3. Localization & Accessibility (a11y) Standards

1. **Explicit Language Tags:** Every page layout must set the accurate root `lang` attribute dynamically based on current route context (`<html lang="en">` or `<html lang="my">`).
2. **SEO Localization (`hreflang`):** Head tags must automatically output localized alternative links for search engine crawlers:

```html
<link rel="alternate" hreflang="en" href="[https://kolinn.dev/blog/post-1](https://kolinn.dev/blog/post-1)" />
<link rel="alternate" hreflang="my" href="[https://kolinn.dev/my/blog/post-1](https://kolinn.dev/my/blog/post-1)" />

```

**Contrast Compliance:** All text combinations across Nord Light and Nord Dark palettes must strictly meet WCAG AA contrast standards (minimum 4.5:1 ratio).

## 4. Cloudflare Pages Optimization & Security Rules

1. **Static Asset Caching:** All immutable assets inside `/public/_headers` must enforce HTTP header policies:

```text
/*
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  Referrer-Policy: strict-origin-when-cross-origin
  Strict-Transport-Security: max-age=31536000; includeSubDomains; preload

/assets/*
  Cache-Control: public, max-age=31536000, immutable

/fonts/*
  Cache-Control: public, max-age=31536000, immutable

```

**Zero Unused Dependencies:** Framework components (React/Vue/Svelte) must not be introduced unless explicit interactive state requires client hydration via Astro Islands (`client:visible`).
