# Project Directory Structure: KOLINN_DEV

```text
KOLINN_DEV/
├── .github/
│   └── workflows/
│       └── ci.yml                 # Code linting & typechecking workflow
├── public/
│   ├── favicon.svg                # Main adaptive theme logo
│   ├── favicon.ico                # Fallback legacy icon
│   ├── apple-touch-icon.png       # iOS home icon
│   ├── og-default.png             # Fallback social share card
│   ├── _headers                   # Cloudflare Pages security & caching headers
│   ├── _redirects                 # Edge redirect rules
│   └── fonts/                     # Self-hosted web fonts
│       ├── fira-code/             # Fira Code variable fonts
│       ├── fira-sans/             # Fira Sans font files
│       └── noto-sans-myanmar/     # Noto Sans Myanmar font files
├── src/
│   ├── assets/                    # Processed & optimized assets
│   │   ├── images/                # Site hero graphics & design assets
│   │   └── posts/                 # Content-specific post illustrations
│   ├── components/                # Modular UI Design System
│   │   ├── primitives/            # Reusable core design elements
│   │   │   ├── Button.astro
│   │   │   ├── Card.astro
│   │   │   ├── Container.astro
│   │   │   ├── Grid.astro
│   │   │   ├── Badge.astro
│   │   │   ├── Input.astro
│   │   │   └── Divider.astro
│   │   ├── content/               # Domain-specific content components
│   │   │   ├── ArticleCard.astro
│   │   │   ├── ProjectCard.astro
│   │   │   ├── ReleaseCard.astro
│   │   │   ├── TableOfContents.astro
│   │   │   ├── CodeBlock.astro
│   │   │   ├── Callout.astro
│   │   │   └── LangNotice.astro
│   │   ├── layout/                # Structural layout frames
│   │   │   ├── SiteHeader.astro
│   │   │   ├── SiteFooter.astro
│   │   │   ├── PageHeader.astro
│   │   │   └── MegaMenu.astro
│   │   └── interactive/           # Client-side micro-interaction drivers
│   │       ├── ThemeToggle.astro
│   │       ├── GlassCardGlow.astro
│   │       └── LanguagePicker.astro
│   ├── content/                   # Typed Markdown & MDX repository
│   │   ├── config.ts              # Zod collection schemas definition
│   │   ├── news/                  # Category: News articles
│   │   │   ├── en/
│   │   │   └── my/
│   │   ├── learn/                 # Category: Guides & Tutorials
│   │   │   ├── en/
│   │   │   └── my/
│   │   ├── projects/              # Category: Case studies
│   │   │   ├── en/
│   │   │   └── my/
│   │   └── releases/              # Category: Product releases
│   │       ├── en/
│   │       └── my/
│   ├── layouts/                   # Global page scaffolds
│   │   ├── BaseLayout.astro       # Root HTML document wrapper
│   │   ├── ArticleLayout.astro    # Blog post reader wrapper
│   │   └── PageLayout.astro       # General marketing/legal page wrapper
│   ├── pages/                     # Dynamic file-based routing core
│   │   ├── index.astro            # Root entry (Redirects or default EN home)
│   │   ├── 404.astro              # Custom localized 404 page
│   │   ├── [lang]/                # i18n dynamic route scope ('en' | 'my')
│   │   │   ├── index.astro        # Localized Homepage
│   │   │   ├── about.astro        # Localized About Page
│   │   │   ├── contact.astro      # Localized Contact Page
│   │   │   ├── support.astro      # Localized Support Page
│   │   │   ├── blog/
│   │   │   │   ├── index.astro    # Main blog hub
│   │   │   │   └── [category]/
│   │   │   │       └── [slug].astro # Dynamic post renderer
│   │   │   └── legal/
│   │   │       ├── terms-of-service.astro
│   │   │       ├── terms-of-use.astro
│   │   │       └── privacy-policy.astro
│   ├── styles/                    # Design System CSS
│   │   ├── tokens.css             # Nord colors, typography scales, tokens
│   │   ├── base.css               # CSS reset and fluid setup
│   │   ├── components.css         # Shared utility utility classes
│   │   └── utilities.css          # Fluid layout utilities
│   └── utils/                     # Functional utilities & business logic
│       ├── i18n.ts                # Route resolution & translation keys
│       ├── content.ts             # Filtering & sorting helper functions
│       └── formatting.ts          # Date & locale formatters
├── astro.config.mjs               # Core Astro framework configuration
├── tsconfig.json                  # TypeScript configuration
└── package.json                   # Project dependencies & operational scripts
```

## Content Schema Definition (`src/content/config.ts`)

```typescript
import { defineCollection, z } from 'astro:content';

const postBaseSchema = z.object({
  title: z.string().max(100),
  description: z.string().max(200),
  pubDate: z.date(),
  updatedDate: z.date().optional(),
  author: z.string().default('KOLINN'),
  featuredImage: z.string().optional(),
  draft: z.boolean().default(false),
  tags: z.array(z.string()).default([]),
  lang: z.enum(['en', 'my']),
});

export const collections = {
  news: defineCollection({ type: 'content', schema: postBaseSchema }),
  learn: defineCollection({ type: 'content', schema: postBaseSchema }),
  projects: defineCollection({ type: 'content', schema: postBaseSchema }),
  releases: defineCollection({ type: 'content', schema: postBaseSchema }),
};
