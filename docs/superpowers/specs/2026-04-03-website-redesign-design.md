# Aracon Software Website Redesign — Design Spec

## Overview

Redesign the Aracon Software website from its current light/blue theme to a premium "Command Center" dark theme. The site positions Aracon as a company (not a personal brand) offering Forward-Deployed Engineering services with AI specialization. Single-page static site built with Astro + Tailwind CSS.

## Design Direction

**Theme:** "Command Center" — dark, technical, premium. Inspired by Vercel, Linear, Palantir aesthetics.

**Identity:** Company-first. "We" language throughout. Founder appears only in a brief credibility section lower on the page.

**Tone:** Direct and confident. Short sentences. No fluff. Work speaks for itself.

**No pricing on the site.** Pricing is discussed in conversations.

## Color Palette

| Role | Color | Hex |
|---|---|---|
| Background (deep) | Near-black with blue undertone | `#0A0F1C` |
| Surface / cards | Slightly lighter | `#141824` |
| Card border | Subtle | `#1E2436` |
| Primary text | Off-white | `#E8ECF4` |
| Secondary text | Muted | `#8892A8` |
| Tertiary text | Dim | `#6B7280` |
| Accent (primary) | Electric cyan | `#00D4FF` |
| Accent (secondary) | Soft violet | `#7C5CFC` |
| Accent (warm) | Amber gold | `#F5A623` |
| Footer background | Deepest dark | `#080B14` |

## Typography

Already installed in the project — no changes needed.

- **Headlines:** Bricolage Grotesque Variable, weight 600-700, tight letter-spacing (-0.02em)
- **Body:** Inter Variable, weight 400, relaxed line-height
- **Labels/tags:** Inter Variable, weight 500-600, uppercase, wide letter-spacing (0.15-0.2em)

## Visual Techniques

All CSS-only, no JavaScript animation libraries.

### Grain Texture Overlay
SVG noise filter as a `::before` pseudo-element on the page background at ~4% opacity. Prevents the dark background from feeling flat.

### Animated Gradient Orb (Hero)
Large, blurred radial gradient (cyan-to-violet) that drifts subtly behind hero text using CSS `@keyframes`. Slow animation (15-20s cycle), subtle movement.

### Glassmorphism Cards
Cards use `backdrop-blur-xl bg-white/[0.03] border border-white/[0.06]`. Frosted glass effect on dark background.

### Gradient Text
Hero headline uses `bg-gradient-to-r from-cyan-400 to-violet-500 bg-clip-text text-transparent` for the accent line.

### Glow Effects
Subtle box-shadow on CTA buttons: `shadow-[0_0_30px_rgba(0,212,255,0.15)]`. "Lit from within" effect.

### Hover States
Cards: border transitions from `border-white/[0.06]` to `border-cyan-400/30` on hover with `transition-all duration-300`.

## Page Structure

Single page with 6 sections (navbar, hero, services, about, CTA, footer), all on the dark background. Navigation via anchor links.

### Section 1: Navbar

Fixed top navigation bar.

- **Left:** Aracon logo (cyan square with "A") + "ARACON" text
- **Right:** Services, About, Contact anchor links
- **Background:** Transparent, with slight blur on scroll (optional)

### Section 2: Hero

Centered layout with animated gradient orbs behind text.

- **Eyebrow:** None (clean, let the headline speak)
- **Headline:** "Production AI systems." (white) + line break + "Shipped, not pitched." (gradient cyan-to-violet)
- **Subtitle:** "We deploy engineers who build real AI infrastructure — from readiness audits to production systems that create capabilities your team didn't have before."
- **CTA:** Single button — "Get in Touch" (gradient cyan-to-violet background, dark text). Links to `mailto:pvillega@aracon.com`
- **Background effects:** Two gradient orbs (one cyan top-center, one violet bottom-right), grain texture overlay

### Section 3: Services

Uniform bento grid with glassmorphism cards. Section header centered above.

- **Header:** "What we do" (uppercase label) + "Our Services" (heading)
- **Layout:** 3 cards top row + 2 cards bottom row (centered)
- **Card style:** Each card has a gradient icon (32x32 rounded square), title, and one-line description
- **All cards same size**, equal visual weight

**The 5 service cards:**

1. **AI Readiness Audit** — Icon gradient: cyan→violet. "We assess your data, infrastructure, and team. You get a prioritized roadmap with clear next steps."
2. **Production AI Systems** — Icon gradient: violet→amber. "End-to-end AI infrastructure that ships. Model serving, RAG pipelines, LLM integration, monitoring."
3. **Agent Orchestration** — Icon gradient: cyan→emerald. "Multi-agent systems that scale your operations 10x. Design, build, deploy, monitor."
4. **Embedded Engineering** — Icon gradient: amber→red. "Engineers deployed into your team on retainer. We integrate, we ship, we transfer knowledge."
5. **Production Engineering** — Icon gradient: gray→white. "Distributed systems, data pipelines, APIs, infrastructure. The foundation everything runs on."

### Section 4: About / Founder Credibility

Company-first section with brief founder info.

- **Header:** "Who we are" (uppercase label) + "Built on 15+ years of shipping." (heading)
- **Layout:** Horizontal flex — small photo (80px, rounded-xl) on left, bio content on right
- **Content:**
  - Name: "Pere Villega"
  - Title: "Founder & Principal Engineer" (in cyan accent color)
  - Bio (3 lines max): "Aracon was founded on a simple principle: AI should ship, not sit in notebooks. We bring distributed systems expertise and production engineering discipline to every engagement."
  - Links: Two small bordered buttons — "LinkedIn" and "Blog" (both open in new tab)
- **Below bio:** Subtle horizontal rule, then company legal info centered: "ARACON SOFTWARE S.L.U. · Barcelona, Spain"

### Section 5: CTA

Final conversion section with gradient glow emphasis.

- **Headline:** "Ready to build something" (white) + "real?" (gradient text)
- **Subtitle:** "Tell us what you're working on. We'll tell you how we can help."
- **CTA:** Single button — "Get in Touch" (gradient background with glow shadow). Links to `mailto:pvillega@aracon.com`
- **Background:** Subtle elliptical gradient glow behind the button area (cyan + violet, very low opacity)

### Section 6: Footer

Minimal footer bar on deepest dark background.

- **Left:** Small Aracon logo + "ARACON SOFTWARE" text, below it: "NIF: B21636643 · Barcelona, Spain · © 2026"
- **Right:** Three outbound links — "Company LinkedIn", "Pere LinkedIn", "Blog" (all with ↗ indicator, open in new tab)

## Outbound Links

- Company LinkedIn (navbar area or footer — needs company LinkedIn URL)
- Pere Villega's LinkedIn: in About section and footer
- Personal blog/site: in About section and footer
- Email: pvillega@aracon.com (hero CTA and final CTA)

## SEO & Meta

- **Title:** "ARACON SOFTWARE — Applied AI Engineering"
- **Description:** "Forward-deployed engineers who build production AI systems. From readiness audits to agent orchestration — shipped, not pitched."
- **OG Image:** Needs updating to match new dark theme (current one shows old Astroship template)

## Responsive Behavior

- **Desktop (lg+):** Full layout as described. Services grid 3+2.
- **Tablet (md):** Services grid 2+2+1. About section stacks vertically. Hero text smaller.
- **Mobile (sm):** Services grid 1 column. Footer stacks vertically. Hero headline 2xl. Full-width CTA buttons.

## What Changes From Current Site

- **Theme:** Light blue/slate → dark navy/black with cyan+violet accents
- **Content framing:** Personal brand (Pere) → company brand (Aracon)
- **Services:** 9 generic cards → 5 focused FDE/AI cards in bento grid
- **Layout:** Standard sections → glassmorphism cards, gradient orbs, grain texture
- **Typography:** Same fonts (Bricolage Grotesque + Inter), used more boldly
- **Hero:** Left-aligned with two CTAs → centered with single punchy CTA
- **Tone:** Consultative/generic → direct, confident, builder-focused

## What Stays The Same

- **Framework:** Astro 5.5.2 + Tailwind CSS v4
- **Fonts:** Bricolage Grotesque + Inter (already installed)
- **Single page structure** with anchor navigation
- **Static site** deployed to GitHub Pages at aracon.com
- **Existing assets:** logo.png, linkedin_profile.jpeg (used in About section)
- **Company legal info:** ARACON SOFTWARE S.L.U., NIF: B21636643, Barcelona, Spain

## Out of Scope

- Blog/content pages (future work)
- Contact form (email CTA only)
- Pricing section
- Case studies / testimonials
- Animation libraries (CSS-only effects)
- New font installations
