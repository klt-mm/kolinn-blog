# Development Roadmap: KOLINN_DEV

## Phase 1: Core Foundation & Infrastructure Setup

**Goal:** Initialize repository, install dependencies, establish Cloudflare deployment pipelines, and build theme infrastructure.

- [ ] **1.1. Framework Initialization**
  - Initialize empty Astro v4+ project targeting static output (`output: 'static'`).
  - Configure `tsconfig.json` for strict TypeScript checking.
  - Setup self-hosted web fonts (`Fira Code`, `Fira Sans`, `Noto Sans Myanmar`) in `public/fonts/`.
- [ ] **1.2. Cloudflare Pages Setup**
  - Create repository on GitHub/GitLab and link to Cloudflare Pages dashboard.
  - Create `public/_headers` and `public/_redirects` files with security rules and cache directives.
  - Validate baseline build speed and static output directory structure.
- [ ] **1.3. Design Token Architecture**
  - Implement `src/styles/tokens.css` with Nord Dark and Nord Light color variables.
  - Build fluid typography engine and 8px spacing rules in `src/styles/base.css`.
  - Add theme initialization blocking script in `BaseLayout.astro` to eliminate FOUC.

---

## Phase 2: Design System & Primitive Components

**Goal:** Develop standard interactive UI primitives matching the terminal glass aesthetic.

- [ ] **2.1. Primitive UI Components**
  - Build core Astro components: `Container`, `Grid`, `Button`, `Badge`, `Card`, `Input`, `Divider`.
  - Implement dynamic card auto-responsive grid system (maximum 3 items per row up to 1440px).
- [ ] **2.2. Interactive Elements & Micro-interactions**
  - Implement `GlassCardGlow` mouse-tracking hover effect using lightweight CSS variable updates.
  - Build hero title and call-to-action hover shimmering effects.
  - Implement theme toggle button (`ThemeToggle.astro`) with native local storage persistence.
- [ ] **2.3. Site Navigation & Layout Frames**
  - Construct site layout scaffold (`SiteHeader`, `SiteFooter`, `PageHeader`).
  - Develop responsive `MegaMenu` supporting both desktop hover states and mobile views.

---

## Phase 3: Content Engine & Internationalization (i18n)

**Goal:** Establish Content Collections schemas and localized dynamic routing.

- [ ] **3.1. Content Collections Schema Setup**
  - Configure `src/content/config.ts` with Zod validation for four categories: `news`, `learn`, `projects`, and `releases`.
  - Create localized content folder structures (`en/` and `my/`) across all content categories.
- [ ] **3.2. i18n Routing Core**
  - Configure `astro.config.mjs` native `i18n` engine (`defaultLocale: 'en'`, `locales: ['en', 'my']`).
  - Implement helper functions in `src/utils/i18n.ts` for localized link generation and language switching.
  - Create localized language switcher dropdown component (`LanguagePicker.astro`).
- [ ] **3.3. Post Reader Enhancements**
  - Build `ArticleLayout.astro` with reading progress bar and localized table of contents.
  - Configure built-in Shiki code block renderer matching Nord Dark/Light syntax themes.

---

## Phase 4: Page Architecture & View Implementation

**Goal:** Construct all static and dynamic site views defined in the sitemap.

- [ ] **4.1. Core Static Pages**
  - Implement Localized Home Page (`src/pages/[lang]/index.astro`).
  - Build `About`, `Contact`, `Support`, and dynamic `404` error pages.
  - Construct legal page views (`Terms of Service`, `Terms of Use`, `Privacy Policy`).
- [ ] **4.2. Blog Category Hubs & Post Renderers**
  - Build localized blog landing view displaying tabbed or categorized posts.
  - Implement `[category]/[slug].astro` dynamic post route renderer supporting all content types.
  - Add fallback UI component (`LangNotice.astro`) displayed when a post is missing Burmese translation.

---

## Phase 5: Optimization, Audit & Production Release

**Goal:** Audit accessibility, performance, SEO, and launch on Cloudflare Pages.

- [ ] **5.1. SEO & RSS Feed Generation**
  - Integrate `@astrojs/rss` to automatically build localized RSS feeds (`/rss.xml` and `/my/rss.xml`).
  - Implement dynamic `sitemap.xml` generation with localized `hreflang` tags.
  - Extract and adapt vector path from `favico.svg` for seamless inline header and social card rendering.
- [ ] **5.2. Lighthouse & Performance Audit**
  - Conduct performance and accessibility audit target score: 100 on Performance, Accessibility, Best Practices, and SEO.
  - Verify zero-JS default payload compliance across all static blog pages.
- [ ] **5.3. Final Launch & Verification**
  - Trigger deployment to Cloudflare Pages main branch.
  - Set custom domain DNS mapping and verify edge header enforcement using browser developer tools.
