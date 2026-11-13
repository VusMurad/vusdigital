# vusdigital.com — Vusala Muradli · Digital Marketing Specialist

Interactive personal portfolio built with **Next.js 16 (App Router) · TypeScript · Tailwind CSS 4 · GSAP + ScrollTrigger · Motion · Three.js / React Three Fiber · Lenis**.

> **New to code?** You only ever need to edit files in the `content/` folder and drop images/PDFs into `public/`.
> Read **[CONTENT_GUIDE.md](./CONTENT_GUIDE.md)** — it walks through every update step by step.

---

## 1. Run it on your computer

Requirements: [Node.js 20 or newer](https://nodejs.org) (LTS).

```bash
npm install        # once
npm run dev        # start → open http://localhost:3000
```

While `npm run dev` is running, **draft mode** is on: every placeholder and unpublished case study is visible (marked with lime “Draft” labels) so you can see where your content goes. None of that appears on the live site.

```bash
npm run build      # production build (what Vercel runs)
npm start          # preview the production build at http://localhost:3000
npm run lint       # type-check only
```

---

## 2. Deploy (GitHub + Vercel) and connect vusdigital.com

**A. Put the code on GitHub**
1. Create a free account at github.com → **New repository** → name it `vusdigital` (Private is fine) → Create.
2. In this folder run:
   ```bash
   git init
   git add .
   git commit -m "Initial portfolio"
   git branch -M main
   git remote add origin https://github.com/<your-username>/vusdigital.git
   git push -u origin main
   ```
   (Or use GitHub Desktop: *Add existing repository* → *Publish*.)

**B. Deploy on Vercel**
1. Sign in at vercel.com with your GitHub account → **Add New… → Project** → import `vusdigital`.
2. Framework preset: **Next.js** (auto-detected). Leave build settings as they are → **Deploy**.
3. You’ll get a URL like `vusdigital.vercel.app` — check it.

**C. Connect the domain**
1. Vercel → your project → **Settings → Domains** → add `vusdigital.com` and `www.vusdigital.com`.
2. Vercel shows DNS records. At your domain registrar, add them (usually an **A record** `@ → 76.76.21.21` and a **CNAME** `www → cname.vercel-dns.com` — always use the exact values Vercel shows you).
3. Set `vusdigital.com` as the primary domain (redirect `www` to it). HTTPS is automatic.
4. DNS can take from a few minutes to 48 hours.

**D. Every future update**: edit content → `git add . && git commit -m "Update" && git push`. Vercel redeploys automatically in ~1 minute.

### Environment variables (optional — only for the contact form)
Copy `.env.example` → `.env.local` for local testing, and add the same values in **Vercel → Settings → Environment Variables**, then **redeploy**.

| Variable | Purpose |
|---|---|
| `RESEND_API_KEY` | API key from [resend.com](https://resend.com) (free tier is enough) |
| `CONTACT_TO_EMAIL` | Inbox that receives messages |
| `CONTACT_FROM_EMAIL` | Sender on your verified domain, e.g. `Portfolio <hello@vusdigital.com>` |
| `SHOW_DRAFTS` | `true` only on preview deployments to review drafts. **Never on production.** |

The form is **only shown when all three contact variables are set**. Without them, visitors see your email / LinkedIn / CV buttons instead — there is never a fake form.

---

## 3. Project structure

```
content/                ← EVERYTHING YOU EDIT
  profile.ts            name, statement, about text, email, LinkedIn, portrait, CV path
  projects.ts           12 portfolio projects + case studies (draft/published)
  skills.ts             expertise categories, capabilities, tools
  metrics.ts            verified results (+ "How I approach marketing" steps)
  experience.ts         CV timeline
  education.ts          education, certifications, training
  site.ts               domain, SEO title/description, navigation
  types.ts              field definitions (no need to edit)

public/
  projects/<slug>/      images for each project (cover.webp, 01.webp …)
  images/               portrait and other images
  cv/                   your CV PDF
  certificates/         certificate images / PDFs

src/
  app/                  pages & routes
    page.tsx            homepage (section order)
    work/[slug]/        case-study pages
    api/contact/        contact-form endpoint (Resend)
    sitemap.ts robots.ts opengraph-image.tsx icon.svg
  components/
    hero/               hero + 3D “Signal → Growth” scene (WebGL) and SVG mobile version
    sections/           About, Expertise, Approach, Metrics, Experience, Education, Contact
    work/               Work gallery/index, generated placeholder covers
    case-study/         case-study layout
    motion/             reveal system, split-text
    providers/          motion preferences + smooth scrolling
    ui/                 nav, cursor, buttons, footer, transitions
  lib/
    content.ts          visibility rules (drafts, approved, file checks)
    motion.ts           shared easing/timing, GSAP setup
```

---

## 4. What’s implemented

**Experience & motion**
- Hero: staggered word-by-word type reveal (pure CSS, so it never waits on JavaScript), magnetic CTAs, animated scroll cue.
- Custom 3D scene “Signal → Growth”: a point-cloud landscape rising left→right, a self-drawing growth line with milestone nodes, pointer lift + parallax tilt. Lazy-loaded after the page is interactive, paused off-screen, single draw call for points.
- Phones/tablets get a lightweight animated SVG version instead of WebGL (also the fallback if WebGL is unavailable).
- Smooth scrolling (Lenis, mouse/trackpad only), scroll-triggered reveals, parallax, a pinned horizontal “How I approach marketing” sequence with a progress bar.
- Custom cursor (desktop only) that becomes a “View” / “Next” disc over projects; magnetic buttons.
- Skills: keyboard-accessible tabs + animated capability constellation with hover/focus explanations; mobile shows the explanations inline (nothing hover-only).
- Work: editorial gallery with varied sizes, hover image zoom, **Index view** with a preview that follows the cursor, service filters (appear automatically once projects have services).
- Case studies: curtain-reveal cover, structured sections, animated **Next project** block, page-wipe transitions between routes.
- Experience: scroll-drawn timeline, expandable entries, links to related projects.
- Metrics: animated counters, line charts, before/after bars — rendered **only** from approved, real data.
- Contact: copy-email interaction, Download CV, LinkedIn/socials, real form with validation, honeypot, timing check and rate limiting.

**Integrity rules (enforced in code)**
- Unpublished case studies → no page, no sitemap entry (404). Name still listed with “Case study coming soon”.
- Metrics, experience, education → hidden on the live site unless `approved: true`. Empty sections disappear from the page and menu.
- If no metrics are approved, the site shows “How I Approach Marketing” instead.
- Download CV / portrait / project images only appear if the file really exists in `public/`.

**Accessibility & performance**
- “Motion on/off” toggle in the menu (remembered), plus full `prefers-reduced-motion` support.
- Skip link, semantic landmarks, visible focus states, ARIA tabs, live regions, accessible mobile menu (Escape closes, focus managed).
- Measured on the production build (Lighthouse): **Desktop** Performance 99 · Accessibility 100 · Best Practices 100 · SEO 100. **Mobile** Performance 92–95 · Accessibility 100 · Best Practices 100 · SEO 100. CLS 0. axe-core: 0 violations.

**SEO**
- Title/description from the brief, canonical URLs (`https://vusdigital.com`), Open Graph + Twitter cards, generated social image (`/opengraph-image`), `sitemap.xml`, `robots.txt`, schema.org `Person` + `WebSite` JSON-LD (only verified fields; LinkedIn/email added automatically when you fill them in), human-readable URLs `/work/<project>`.

---

## 5. Future enhancement (optional)
If editing TypeScript files ever feels inconvenient, a free Git-based CMS such as **[Decap CMS](https://decapcms.org)** or **[TinaCMS](https://tina.io)** can be added on top of the same content files, giving you a web form to edit projects. Not required — the file-based system works without any extra service.
