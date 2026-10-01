# Architectural Specification: KOLINN_DEV

## 1. Executive Overview & System Architecture

KOLINN_DEV is a high-performance, multilingual personal engineering blog built on Astro and deployed natively to Cloudflare Pages. The system operates on a Static Site Generation (SSG) model, compiling content into static HTML, CSS, and minimal Client-Side JavaScript at build time.

### System Topology

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        DEVELOPMENT & BUILD ENGINE                      │
│                                                                        │
│   ┌────────────────┐     ┌────────────────┐    ┌───────────────────┐   │
│   │ Markdown / MDX │ ──> │ Astro SSG      │ -> │ Cloudflare Pages  │   │
│   │ Content & i18n │     │ Compiler & Zod │    │ Static Assets     │   │
│   └────────────────┘     └────────────────┘    └───────────────────┘   │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ Deployment
                                    v
┌────────────────────────────────────────────────────────────────────────┐
│                         CLOUDFLARE EDGE NETWORK                        │
│                                                                        │
│      ┌──────────────────────────────────────────────────────────┐      │
│      │               Global CDN / Edge Caching                  │      │
│      └────────────────────────────┬─────────────────────────────┘      │
│                                   │                                    │
│                 ┌─────────────────┴─────────────────┐                  │
│                 ▼                                   ▼                  │
│     ┌───────────────────────┐           ┌───────────────────────┐      │
│     │ Default Locale: /en/  │           │ Locale Route: /my/    │      │
│     │ (English Engine)      │           │ (Burmese Engine)      │      │
│     └───────────────────────┘           └───────────────────────┘      │
└────────────────────────────────────────────────────────────────────────┘
```

## 2. Core Technical Stack

| Domain | Technology Choice | Architectural Justification |
| --- | --- | --- |
| **Framework** | Astro (v4+) | Zero-JS default payload, native Islands architecture, built-in i18n routing, and first-class Cloudflare Pages compatibility. |
| **Deployment Host** | Cloudflare Pages | Global edge distribution, instant deployment previews, edge caching, and static asset serving with zero server overhead. |
| **Content Engine** | Astro Content Collections + Zod | Type-safe frontmatter schema validation across content categories and language variants. |
| **Styling** | Modern CSS Variables & CSS Modules | Zero-runtime CSS design tokens mapped to the Nord color system; fluid typography without heavy runtime libraries. |
| **Typography Engine** | Fira Code, Fira Sans, Noto Sans Myanmar | Dedicated code/body font stack with localized unicode support for Burmese script rendering. |
| **Syntax Highlighting** | Shiki (Built-in) | Build-time syntax highlighting for terminal code blocks supporting Nord themes with no client-side JS burden. |

## 3. Internationalization (i18n) Architecture

The application implements a dual-locale architecture supporting **English (`en`)** as the default language and **Burmese (`my`)** as the secondary locale.

### Routing Strategy

* **Default Locale (`en`):** Served at the domain root (e.g., `https://kolinn.dev/blog/first-post`).
* **Localized Locale (`my`):** Served with explicit locale prefixes (e.g., `https://kolinn.dev/my/blog/first-post`).

### Fallback Mechanics

* If an article exists in `en` but lacks a corresponding translation in `my`, Astro's i18n fallback engine resolves the route to the `en` source while rendering a contextual localization notice in the UI.

## 4. Asset Management Architecture

Assets are split strictly into two domains based on processing needs:

```text
               ┌─────────────────────────────────────────┐
               │           ASSET ARCHITECTURE            │
               └────────────────────┬────────────────────┘
                                    │
         ┌──────────────────────────┴──────────────────────────┐
         ▼                                                     ▼
┌─────────────────────────────────┐           ┌─────────────────────────────────┐
│     public/ (Unprocessed)       │           │   src/assets/ (Optimized)       │
├─────────────────────────────────┤           ├─────────────────────────────────┤
│ • Adaptive Favicons             │           │ • Article Content Images        │
│ • static og-default.png         │           │ • Hero Banners                  │
│ • _headers & _redirects         │           │ • Author Avatars                │
│ • Self-hosted Font Files        │           │ (WebP/AVIF auto-compression)    │
└─────────────────────────────────┘           └─────────────────────────────────┘

```

## 5. UI/UX & Theme Pipeline

1. **Theme Hydration Engine:** Flash-of-Unstyled-Content (FOUC) is prevented via a blocking inline script in the `<head>` of `BaseLayout.astro`, reading `localStorage` or falling back to `prefers-color-scheme`.
2. **Design Tokens:** Mapped using CSS custom variables supporting smooth state transitions between `Nord Dark` and `Nord Light`.
3. **Zero-Framework Micro-interactions:** Mouse-tracking glowing glass cards and shimmering title hovers are implemented via scoped standard browser scripts using standard CSS variable mutations on `mousemove`.

## 6. Cloudflare Pages Deployment & Edge Security

* **Build Target:** Pure static export (`output: 'static'`) compiling to the `dist/` directory.
* **Edge Caching Rules:** Configured via `public/_headers` to mark immutable static assets (`/assets/*`, `/fonts/*`) with `max-age=31536000, immutable`.
* **Security Headers:** Enforced HTTP strict transport security (HSTS), frame options, content-type nosniff, and granular permissions policies at the Cloudflare edge layer.
