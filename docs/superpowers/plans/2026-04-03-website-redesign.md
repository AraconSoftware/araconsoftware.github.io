# Website Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesign the Aracon Software website from a light/blue generic consulting theme to a premium dark "Command Center" theme with company-first FDE positioning.

**Architecture:** Modify existing Astro components in-place. Update global.css with dark theme custom properties and utility classes (grain texture, gradient orb animation). Rewrite each component's markup and Tailwind classes to match the new dark glassmorphism design. No new dependencies needed.

**Tech Stack:** Astro 5.5.2, Tailwind CSS v4, astro-icon, astro-navbar, astro-seo

**Important:** Do NOT commit changes. The user wants to review and compare before/after.

**Note:** The Company LinkedIn URL (`https://www.linkedin.com/company/aracon-software/`) in the footer is a guess — update with the real URL if different.

---

## File Map

| File | Action | Responsibility |
|---|---|---|
| `src/styles/global.css` | Modify | Dark theme base styles, grain texture, gradient orb animation, custom color properties |
| `src/layouts/Layout.astro` | Modify | Update SEO meta, dark body background, update title/description |
| `src/components/navbar/navbar.astro` | Modify | Dark navbar with cyan logo square, transparent background, light text |
| `src/components/hero.astro` | Modify | Centered layout, gradient headline, single CTA, gradient orbs |
| `src/components/features.astro` | Modify | Rename to services concept, 5 FDE cards in uniform bento grid with glassmorphism |
| `src/components/logos.astro` | Modify | Company-first about section with small founder photo and bio |
| `src/components/cta.astro` | Modify | Dark CTA with gradient glow, "Ready to build something real?" |
| `src/components/footer.astro` | Modify | Minimal dark footer, company info left, outbound links right |
| `src/components/container.astro` | Modify | Keep as-is but may need dark-compatible adjustments |
| `src/pages/index.astro` | Modify | Same structure, no changes needed (components handle the redesign) |
| `src/pages/404.astro` | Modify | Dark theme 404 page |
| `src/components/ui/link.astro` | Modify | Dark theme link styles |

---

### Task 1: Global CSS — Dark Theme Foundation

**Files:**
- Modify: `src/styles/global.css`

- [ ] **Step 1: Replace global.css with dark theme base styles, grain texture, and orb animation**

Replace the entire contents of `src/styles/global.css` with:

```css
@import 'tailwindcss';

@plugin '@tailwindcss/typography';

@theme {
  --font-sans:
    Bricolage Grotesque Variable, Inter Variable, Inter, ui-sans-serif,
    system-ui, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji',
    'Segoe UI Symbol', 'Noto Color Emoji';

  --color-bg-deep: #0A0F1C;
  --color-bg-surface: #141824;
  --color-bg-footer: #080B14;
  --color-border-card: #1E2436;
  --color-text-primary: #E8ECF4;
  --color-text-secondary: #8892A8;
  --color-text-tertiary: #6B7280;
  --color-accent-cyan: #00D4FF;
  --color-accent-violet: #7C5CFC;
  --color-accent-amber: #F5A623;
}

@layer base {
  *,
  ::after,
  ::before,
  ::backdrop,
  ::file-selector-button {
    border-color: var(--color-border-card);
  }

  body {
    background-color: var(--color-bg-deep);
    color: var(--color-text-primary);
  }
}

/* Grain texture overlay — applied via class="grain" on a container */
.grain::before {
  content: '';
  position: fixed;
  inset: 0;
  z-index: 50;
  pointer-events: none;
  opacity: 0.04;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
}

/* Animated gradient orb */
@keyframes orb-drift {
  0%, 100% { transform: translate(0, 0) scale(1); }
  33% { transform: translate(30px, -20px) scale(1.05); }
  66% { transform: translate(-20px, 15px) scale(0.95); }
}

.gradient-orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  animation: orb-drift 18s ease-in-out infinite;
  pointer-events: none;
}

.gradient-orb--cyan {
  background: radial-gradient(circle, rgba(0, 212, 255, 0.12) 0%, transparent 70%);
}

.gradient-orb--violet {
  background: radial-gradient(circle, rgba(124, 92, 252, 0.08) 0%, transparent 70%);
  animation-delay: -6s;
}

/* Glassmorphism card */
.glass-card {
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.06);
  backdrop-filter: blur(16px);
  border-radius: 12px;
  transition: border-color 0.3s ease;
}

.glass-card:hover {
  border-color: rgba(0, 212, 255, 0.3);
}

/* Gradient text utility */
.text-gradient {
  background: linear-gradient(90deg, var(--color-accent-cyan), var(--color-accent-violet));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* CTA button glow */
.btn-glow {
  box-shadow: 0 0 30px rgba(0, 212, 255, 0.15);
  transition: box-shadow 0.3s ease, transform 0.2s ease;
}

.btn-glow:hover {
  box-shadow: 0 0 40px rgba(0, 212, 255, 0.25);
  transform: translateY(-1px);
}
```

- [ ] **Step 2: Verify the dev server starts without errors**

Run: `npm run dev`
Expected: Astro dev server starts. Page renders with dark background. Existing components will look broken (light text on dark = expected, we'll fix them next).

---

### Task 2: Layout — SEO & Dark Body

**Files:**
- Modify: `src/layouts/Layout.astro`

- [ ] **Step 1: Update Layout.astro with new SEO meta and dark body**

Replace the entire contents of `src/layouts/Layout.astro` with:

```astro
---
import { SEO } from "astro-seo";
import Footer from "@/components/footer.astro";
import Navbar from "@/components/navbar/navbar.astro";
import "@fontsource-variable/inter/index.css";
import "@fontsource-variable/bricolage-grotesque";
import "../styles/global.css";

export interface Props {
  title: string;
}

const canonicalURL = new URL(Astro.url.pathname, Astro.site).toString();

const resolvedImageWithDomain = new URL(
  "/opengraph.jpg",
  Astro.site
).toString();

const { title } = Astro.props;

const makeTitle = title
  ? title + " | " + "ARACON SOFTWARE"
  : "ARACON SOFTWARE — Applied AI Engineering";
---

<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="generator" content={Astro.generator} />

    <SEO
      title={makeTitle}
      description="Forward-deployed engineers who build production AI systems. From readiness audits to agent orchestration — shipped, not pitched."
      canonical={canonicalURL}
      openGraph={{
        basic: {
          url: canonicalURL,
          type: "website",
          title: `ARACON SOFTWARE — Applied AI Engineering`,
          image: resolvedImageWithDomain,
        },
        image: {
          alt: "ARACON SOFTWARE — Applied AI Engineering",
        },
      }}
    />
  </head>
  <body class="grain">
    <Navbar />
    <slot />
    <Footer />
    <style is:global>
      img {
        content-visibility: auto;
      }
    </style>
  </body>
</html>
```

- [ ] **Step 2: Verify dev server still runs**

Run: `npm run dev`
Expected: Page loads with grain texture overlay visible (very subtle noise on dark background). SEO title updated in browser tab.

---

### Task 3: Navbar — Dark Theme

**Files:**
- Modify: `src/components/navbar/navbar.astro`

- [ ] **Step 1: Replace navbar with dark theme version**

Replace the entire contents of `src/components/navbar/navbar.astro` with:

```astro
---
import Container from "@/components/container.astro";
import { Astronav, MenuItems, MenuIcon } from "astro-navbar";

const menuitems = [
  {
    title: "Services",
    path: "#services",
  },
  {
    title: "About",
    path: "#about",
  },
  {
    title: "Contact",
    path: "#contact",
  },
];
---

<Container>
  <header class="flex flex-col lg:flex-row justify-between items-center my-5">
    <Astronav>
      <div class="flex w-full lg:w-auto items-center justify-between">
        <a href="/" class="flex items-center gap-2">
          <div class="w-8 h-8 bg-[var(--color-accent-cyan)] rounded-md flex items-center justify-center text-sm font-extrabold text-[var(--color-bg-deep)]">
            A
          </div>
          <span class="text-[var(--color-text-primary)] font-semibold tracking-wide text-sm">
            ARACON
          </span>
        </a>
        <div class="block lg:hidden">
          <MenuIcon class="w-4 h-4 text-[var(--color-text-secondary)]" />
        </div>
      </div>
      <MenuItems class="hidden w-full lg:w-auto mt-2 lg:flex lg:mt-0">
        <ul class="flex flex-col lg:flex-row lg:gap-3">
          {
            menuitems.map((item) => (
              <li>
                <a
                  href={item.path}
                  class="flex lg:px-3 py-2 items-center text-[var(--color-text-secondary)] hover:text-[var(--color-text-primary)] transition-colors text-sm">
                  <span>{item.title}</span>
                </a>
              </li>
            ))
          }
        </ul>
      </MenuItems>
    </Astronav>
  </header>
</Container>
```

- [ ] **Step 2: Check navbar renders correctly**

Run: `npm run dev`
Expected: Cyan "A" square logo, "ARACON" text in off-white, nav links in muted gray. Mobile hamburger icon visible on small screens.

---

### Task 4: Hero — Centered, Punchy, Single CTA

**Files:**
- Modify: `src/components/hero.astro`

- [ ] **Step 1: Replace hero with centered dark theme version**

Replace the entire contents of `src/components/hero.astro` with:

```astro
---

---

<main class="relative pt-24 pb-16 md:pt-32 md:pb-24 overflow-hidden">
  <!-- Gradient orbs -->
  <div class="gradient-orb gradient-orb--cyan w-[500px] h-[400px] -top-[100px] left-1/2 -translate-x-1/2"></div>
  <div class="gradient-orb gradient-orb--violet w-[350px] h-[350px] -bottom-[120px] -right-[60px]"></div>

  <div class="relative z-10 text-center max-w-3xl mx-auto px-5">
    <h1 class="text-4xl md:text-5xl lg:text-6xl font-bold tracking-tight leading-[1.1]">
      <span class="text-[var(--color-text-primary)]">Production AI systems.</span>
      <br />
      <span class="text-gradient">Shipped, not pitched.</span>
    </h1>
    <p class="text-base md:text-lg mt-6 text-[var(--color-text-secondary)] max-w-2xl mx-auto leading-relaxed">
      We deploy engineers who build real AI infrastructure — from readiness
      audits to production systems that create capabilities your team didn't
      have before.
    </p>
    <div class="mt-10">
      <a
        href="mailto:pvillega@aracon.com?subject=Inquiry"
        class="inline-flex items-center justify-center px-7 py-3 bg-gradient-to-r from-[var(--color-accent-cyan)] to-[var(--color-accent-violet)] text-[var(--color-bg-deep)] rounded-lg font-semibold text-sm btn-glow">
        Get in Touch
      </a>
    </div>
  </div>
</main>
```

- [ ] **Step 2: Verify hero renders**

Run: `npm run dev`
Expected: Centered headline with "Production AI systems." in white and "Shipped, not pitched." in cyan-to-violet gradient. Single gradient CTA button with glow. Gradient orbs drifting subtly in background.

---

### Task 5: Services — Uniform Bento Grid

**Files:**
- Modify: `src/components/features.astro`

- [ ] **Step 1: Replace features with 5-card uniform bento grid**

Replace the entire contents of `src/components/features.astro` with:

```astro
---

const services = [
  {
    title: "AI Readiness Audit",
    description:
      "We assess your data, infrastructure, and team. You get a prioritized roadmap with clear next steps.",
    gradient: "from-[#00D4FF] to-[#7C5CFC]",
    icon: "◆",
  },
  {
    title: "Production AI Systems",
    description:
      "End-to-end AI infrastructure that ships. Model serving, RAG pipelines, LLM integration, monitoring.",
    gradient: "from-[#7C5CFC] to-[#F5A623]",
    icon: "⚙",
  },
  {
    title: "Agent Orchestration",
    description:
      "Multi-agent systems that scale your operations 10x. Design, build, deploy, monitor.",
    gradient: "from-[#00D4FF] to-[#10B981]",
    icon: "★",
  },
  {
    title: "Embedded Engineering",
    description:
      "Engineers deployed into your team on retainer. We integrate, we ship, we transfer knowledge.",
    gradient: "from-[#F5A623] to-[#EF4444]",
    icon: "⚖",
  },
  {
    title: "Production Engineering",
    description:
      "Distributed systems, data pipelines, APIs, infrastructure. The foundation everything runs on.",
    gradient: "from-[#8892A8] to-[#E8ECF4]",
    icon: "▭",
  },
];
---

<div id="services" class="py-16 md:py-24 scroll-mt-24">
  <div class="text-center mb-12">
    <p class="text-[var(--color-text-secondary)] text-xs font-semibold uppercase tracking-[0.2em] mb-3">
      What we do
    </p>
    <h2 class="text-3xl md:text-4xl font-bold text-[var(--color-text-primary)] tracking-tight">
      Our Services
    </h2>
  </div>

  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4 max-w-4xl mx-auto">
    {
      services.map((service) => (
        <div class="glass-card p-6">
          <div
            class={`w-9 h-9 bg-gradient-to-br ${service.gradient} rounded-lg flex items-center justify-center mb-4`}>
            <span class="text-[var(--color-bg-deep)] text-sm">{service.icon}</span>
          </div>
          <h3 class="text-[var(--color-text-primary)] font-semibold text-sm mb-2">
            {service.title}
          </h3>
          <p class="text-[var(--color-text-secondary)] text-xs leading-relaxed">
            {service.description}
          </p>
        </div>
      ))
    }
  </div>
</div>
```

- [ ] **Step 2: Verify services grid renders**

Run: `npm run dev`
Expected: "What we do" label + "Our Services" heading centered. 5 glassmorphism cards in a 3+2 grid on desktop, 2-column on tablet, 1-column on mobile. Each card has a gradient icon square, title, and description. Cards highlight border on hover.

---

### Task 6: About — Company-First Founder Section

**Files:**
- Modify: `src/components/logos.astro`

- [ ] **Step 1: Replace logos with company-first about section**

Replace the entire contents of `src/components/logos.astro` with:

```astro
---
import { Picture } from "astro:assets";
import profileImage from "@/assets/linkedin_profile.jpeg";
---

<div id="about" class="py-16 md:py-24 max-w-2xl mx-auto scroll-mt-24">
  <div class="text-center mb-10">
    <p class="text-[var(--color-text-secondary)] text-xs font-semibold uppercase tracking-[0.2em] mb-3">
      Who we are
    </p>
    <h2 class="text-3xl md:text-4xl font-bold text-[var(--color-text-primary)] tracking-tight">
      Built on 15+ years of shipping.
    </h2>
  </div>

  <div class="flex flex-col sm:flex-row gap-6 items-center sm:items-start">
    <Picture
      src={profileImage}
      alt="Pere Villega"
      widths={[160, 240]}
      sizes="80px"
      class="w-20 h-20 rounded-xl object-cover border border-[var(--color-border-card)] shrink-0"
      loading="lazy"
      format="webp"
    />
    <div class="text-center sm:text-left">
      <h3 class="text-[var(--color-text-primary)] font-semibold text-base">
        Pere Villega
      </h3>
      <p class="text-[var(--color-accent-cyan)] text-xs mb-3">
        Founder & Principal Engineer
      </p>
      <p class="text-[var(--color-text-secondary)] text-sm leading-relaxed mb-4">
        Aracon was founded on a simple principle: AI should ship, not sit in
        notebooks. We bring distributed systems expertise and production
        engineering discipline to every engagement.
      </p>
      <div class="flex gap-3 justify-center sm:justify-start">
        <a
          href="https://www.linkedin.com/in/perevillega/"
          target="_blank"
          rel="noopener noreferrer"
          class="px-3 py-1.5 border border-[var(--color-border-card)] rounded-md text-[var(--color-text-secondary)] hover:text-[var(--color-text-primary)] hover:border-[var(--color-accent-cyan)]/30 transition-colors text-xs">
          LinkedIn ↗
        </a>
        <a
          href="https://perevillega.com"
          target="_blank"
          rel="noopener noreferrer"
          class="px-3 py-1.5 border border-[var(--color-border-card)] rounded-md text-[var(--color-text-secondary)] hover:text-[var(--color-text-primary)] hover:border-[var(--color-accent-cyan)]/30 transition-colors text-xs">
          Blog ↗
        </a>
      </div>
    </div>
  </div>

  <div class="mt-10 pt-6 border-t border-[rgba(255,255,255,0.05)] text-center">
    <p class="text-[var(--color-text-tertiary)] text-xs">
      ARACON SOFTWARE S.L.U. · Barcelona, Spain
    </p>
  </div>
</div>
```

- [ ] **Step 2: Verify about section renders**

Run: `npm run dev`
Expected: "Who we are" label + heading centered. Small rounded photo on left, name/title/bio/links on right. Company legal info below a subtle divider. Responsive: stacks vertically on mobile.

---

### Task 7: CTA — Gradient Glow

**Files:**
- Modify: `src/components/cta.astro`

- [ ] **Step 1: Replace CTA with dark gradient glow version**

Replace the entire contents of `src/components/cta.astro` with:

```astro
---

---

<div
  id="contact"
  class="py-20 md:py-28 text-center relative overflow-hidden scroll-mt-24">
  <!-- Gradient glow behind CTA -->
  <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[500px] h-[250px] bg-[radial-gradient(ellipse,rgba(0,212,255,0.08)_0%,rgba(124,92,252,0.04)_50%,transparent_70%)] pointer-events-none"></div>

  <div class="relative z-10">
    <h2 class="text-3xl md:text-4xl lg:text-5xl font-bold tracking-tight">
      <span class="text-[var(--color-text-primary)]">Ready to build something </span>
      <span class="text-gradient">real?</span>
    </h2>
    <p class="text-[var(--color-text-secondary)] mt-4 text-sm md:text-base max-w-md mx-auto">
      Tell us what you're working on. We'll tell you how we can help.
    </p>
    <div class="mt-8">
      <a
        href="mailto:pvillega@aracon.com?subject=Inquiry"
        class="inline-flex items-center justify-center px-8 py-3.5 bg-gradient-to-r from-[var(--color-accent-cyan)] to-[var(--color-accent-violet)] text-[var(--color-bg-deep)] rounded-lg font-semibold text-sm btn-glow">
        Get in Touch
      </a>
    </div>
  </div>
</div>
```

- [ ] **Step 2: Verify CTA renders**

Run: `npm run dev`
Expected: "Ready to build something real?" with "real?" in gradient text. Subtle elliptical glow behind the section. Gradient button with glow shadow.

---

### Task 8: Footer — Minimal Dark

**Files:**
- Modify: `src/components/footer.astro`

- [ ] **Step 1: Replace footer with minimal dark version**

Replace the entire contents of `src/components/footer.astro` with:

```astro
---

---

<footer class="bg-[var(--color-bg-footer)] py-8 mt-8">
  <div class="max-w-(--breakpoint-xl) mx-auto px-5">
    <div class="flex flex-col sm:flex-row justify-between items-center gap-4">
      <div class="text-center sm:text-left">
        <div class="flex items-center gap-2 justify-center sm:justify-start mb-2">
          <div class="w-5 h-5 bg-[var(--color-accent-cyan)] rounded flex items-center justify-center text-[8px] font-extrabold text-[var(--color-bg-deep)]">
            A
          </div>
          <span class="text-[var(--color-text-secondary)] text-xs font-semibold">
            ARACON SOFTWARE
          </span>
        </div>
        <p class="text-[var(--color-text-tertiary)] text-[10px]">
          NIF: B21636643 · Barcelona, Spain · © {new Date().getFullYear()}
        </p>
      </div>
      <div class="flex gap-4">
        <a
          href="https://www.linkedin.com/company/aracon-software/"
          target="_blank"
          rel="noopener noreferrer"
          class="text-[var(--color-text-secondary)] hover:text-[var(--color-text-primary)] transition-colors text-xs">
          Company LinkedIn ↗
        </a>
        <a
          href="https://www.linkedin.com/in/perevillega/"
          target="_blank"
          rel="noopener noreferrer"
          class="text-[var(--color-text-secondary)] hover:text-[var(--color-text-primary)] transition-colors text-xs">
          Pere LinkedIn ↗
        </a>
        <a
          href="https://perevillega.com"
          target="_blank"
          rel="noopener noreferrer"
          class="text-[var(--color-text-secondary)] hover:text-[var(--color-text-primary)] transition-colors text-xs">
          Blog ↗
        </a>
      </div>
    </div>
  </div>
</footer>
```

- [ ] **Step 2: Verify footer renders**

Run: `npm run dev`
Expected: Dark footer bar. Small cyan "A" logo + "ARACON SOFTWARE" on left with legal info. Three outbound links on right. Responsive: stacks vertically on mobile.

---

### Task 9: Link Component & 404 — Dark Theme Updates

**Files:**
- Modify: `src/components/ui/link.astro`
- Modify: `src/pages/404.astro`

- [ ] **Step 1: Update link component for dark theme**

Replace the entire contents of `src/components/ui/link.astro` with:

```astro
---
interface Props {
  href: string;
  size?: "md" | "lg";
  block?: boolean;
  style?: "outline" | "primary" | "inverted" | "muted";
  class?: string;
  [x: string]: any;
}

const {
  href,
  block,
  size = "lg",
  style = "primary",
  class: className,
  ...rest
} = Astro.props;

const sizes = {
  lg: "px-5 py-2.5",
  md: "px-4 py-2",
};

const styles = {
  outline:
    "border border-[var(--color-border-card)] text-[var(--color-text-secondary)] hover:border-[var(--color-accent-cyan)]/30 hover:text-[var(--color-text-primary)]",
  primary:
    "bg-gradient-to-r from-[var(--color-accent-cyan)] to-[var(--color-accent-violet)] text-[var(--color-bg-deep)] font-semibold",
  inverted:
    "bg-[var(--color-text-primary)] text-[var(--color-bg-deep)]",
  muted:
    "bg-[var(--color-bg-surface)] text-[var(--color-text-secondary)] hover:text-[var(--color-text-primary)]",
};
---

<a
  href={href}
  {...rest}
  class:list={[
    "rounded-md text-center transition text-sm focus-visible:ring-2 ring-offset-2 ring-[var(--color-accent-cyan)]",
    block && "w-full",
    sizes[size],
    styles[style],
    className,
  ]}
  ><slot />
</a>
```

- [ ] **Step 2: Update 404 page for dark theme**

Replace the entire contents of `src/pages/404.astro` with:

```astro
---
import Layout from "@/layouts/Layout.astro";
import Container from "@/components/container.astro";
---

<Layout title="Page Not Found">
  <Container>
    <div class="flex flex-col items-center justify-center min-h-[60vh] text-center">
      <h1 class="text-6xl font-bold mb-4 text-[var(--color-text-primary)]">404</h1>
      <h2 class="text-2xl mb-6 text-[var(--color-text-secondary)]">Page Not Found</h2>
      <p class="text-[var(--color-text-tertiary)] mb-8">
        The page you're looking for doesn't exist or has been moved.
      </p>
      <a
        href="/"
        class="px-6 py-3 bg-gradient-to-r from-[var(--color-accent-cyan)] to-[var(--color-accent-violet)] text-[var(--color-bg-deep)] rounded-lg font-semibold text-sm btn-glow transition">
        Go Home
      </a>
    </div>
  </Container>
</Layout>
```

- [ ] **Step 3: Verify both pages render correctly**

Run: `npm run dev`
Expected: 404 page at `/404` shows dark themed error page with gradient "Go Home" button. Link component styles updated for dark backgrounds.

---

### Task 10: Final Verification — Full Page Review

**Files:**
- No file changes

- [ ] **Step 1: Run production build to check for errors**

Run: `npm run build`
Expected: Build succeeds with no errors. Output shows generated pages.

- [ ] **Step 2: Preview production build**

Run: `npm run preview`
Expected: Full site renders correctly at the preview URL. Walk through all sections:
1. Navbar: Cyan "A" logo, nav links in muted gray
2. Hero: Centered headline with gradient text, single CTA, gradient orbs drifting
3. Services: 5 glassmorphism cards in 3+2 grid, hover border glow
4. About: Company-first with small photo, founder bio, outbound links
5. CTA: "Ready to build something real?" with gradient glow
6. Footer: Minimal, company info + 3 outbound links
7. Grain texture visible across entire page (subtle noise)
8. Responsive: Check mobile and tablet breakpoints

- [ ] **Step 3: Check 404 page**

Navigate to any non-existent URL (e.g., `/nonexistent`)
Expected: Dark-themed 404 page with gradient "Go Home" button.
