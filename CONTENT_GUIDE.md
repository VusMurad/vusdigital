# Content guide — how to update your portfolio

Everything you edit lives in **`content/`** (text) and **`public/`** (images & files).
After editing, run `npm run dev` to check locally, then push to GitHub — the live site updates automatically.

**Golden rules**
- Only add information you can verify. Never estimate results.
- Keep the punctuation: text goes between quotes `"like this"`, and each item ends with a comma `,`.
- If something breaks, `npm run build` will tell you the file and line number.
- Image tips: use **.webp** or **.jpg**, ~1600 px wide, under ~400 KB. File names: lowercase, no spaces (`cover.webp`, `01.webp`).

---

## 1. Update contact info, LinkedIn and CV → `content/profile.ts`

```ts
email: "hello@vusdigital.com",
linkedin: "https://www.linkedin.com/in/your-handle",
socials: [{ label: "Instagram", url: "https://instagram.com/…" }],
cvFile: "/cv/Vusala-Muradli-CV.pdf",
```
1. Put your PDF in `public/cv/` (e.g. `public/cv/Vusala-Muradli-CV.pdf`).
2. Write its path in `cvFile` (without `public`).
3. “Download CV” buttons appear in the menu, hero, experience and contact sections automatically.

**Replace the CV later:** overwrite the PDF with a file of the same name.

## 2. Change your profile picture → `content/profile.ts`
1. Save the photo as `public/images/vusala.webp` (portrait ratio, about 1200 × 1500).
2. Set `portrait: "/images/vusala.webp",`

## 3. Publish a project case study → `content/projects.ts`

Find the project (e.g. `slug: "velvetto"`) and fill it in. Complete example:

```ts
{
  slug: "velvetto",
  name: "Velvetto",
  published: true,                       // ← makes the case study public
  industry: "Fashion e-commerce",
  role: "SEO & Paid Media Specialist",
  services: ["SEO", "Paid Advertising", "E-commerce"],
  period: "2024 – 2025",
  website: "https://velvetto.example",
  summary: "Search and paid-media setup for an online fashion store.",
  cover: { src: "/projects/velvetto/cover.webp", alt: "Velvetto homepage on desktop and mobile" },
  caseStudy: {
    overview: "Short introduction to the business and what the project covered.",
    challenge: "The business or marketing problem to solve.",
    role: "What you personally were responsible for.",
    strategy: "The approach you chose and why.",
    execution: [
      "Rebuilt the category page structure around search intent",
      "Set up GA4 + Google Tag Manager conversion tracking",
    ],
    tools: ["Google Search Console", "Google Analytics 4", "Shopify"],
    results: [
      { label: "Organic sessions", value: "+48%", note: "Jan–Jun 2025 vs Jul–Dec 2024, GA4" },
    ],
    resultsSummary: "Optional one-paragraph explanation of the results.",
    gallery: [
      { src: "/projects/velvetto/01.webp", alt: "Category page redesign", caption: "New category layout" },
      { src: "/projects/velvetto/02.webp", alt: "GA4 conversion report" },
    ],
  },
},
```
Images go in `public/projects/velvetto/`.

- Allowed `services`: `"SEO"`, `"Paid Advertising"`, `"Social Media"`, `"Website Development"`, `"E-commerce"`, `"Branding / Creative"`. The Work filters appear by themselves once projects have services.
- Any empty field is simply hidden on the live site. **`results: []` is perfectly fine** — the Results block is then hidden.
- Only use screenshots you have the right to show.

## 4. Add a new project
Copy any project block in `content/projects.ts` (from `{` to `},`), paste it where you want it in the list (order = order on site), change `slug` and `name`, and create the folder `public/projects/<slug>/`.
The URL becomes `vusdigital.com/work/<slug>`.

## 5. Add verified metrics → `content/metrics.ts`
Replace a placeholder with real figures and set `approved: true`:
```ts
{
  id: "velvetto-organic",
  approved: true,
  label: "Organic sessions",
  value: 48, prefix: "+", suffix: "%",
  period: "Jan–Jun 2025 vs Jul–Dec 2024",
  note: "Google Analytics 4",
  project: "velvetto",                                   // links to the case study
  series: [ { label: "Jan", value: 1200 }, { label: "Feb", value: 1340 }, { label: "Mar", value: 1610 } ], // optional chart
  comparison: { before: 1200, after: 1780 },             // optional before/after bars
},
```
As soon as **one** metric is approved, the “Measured results” section replaces “How I approach marketing”. Delete the placeholders you don’t use.

## 6. Update experience → `content/experience.ts`
Replace the `[bracketed]` text and set `approved: true`. Use `type` honestly — `"Full-time"`, `"Freelance"`, `"Agency"`, `"Contract"`, `"Personal project"`. Link projects by slug: `projects: ["velvetto", "miniland"]`.
The Experience section and its menu link appear once at least one entry is approved.

## 7. Add a certificate → `content/education.ts`
```ts
{
  id: "google-ads-search",
  approved: true,
  category: "Professional Certifications",
  title: "Google Ads Search Certification",
  institution: "Google",
  end: "Mar 2025",
  credentialUrl: "https://skillshop.credential.net/…",   // optional
  file: "/certificates/google-ads-search.pdf",           // optional, file in public/certificates/
},
```
Categories: `"Education"`, `"Professional Certifications"`, `"Training & Workshops"`.

## 8. Edit skills and tools → `content/skills.ts`
Add tools to a category’s `tools` list (for example, your email platform under “Email & E-commerce”). Capabilities have a `name` and a one-line `detail` shown on hover.

## 9. Change texts, SEO title or menu
- Hero statement, About paragraphs, specialties → `content/profile.ts`
- SEO title/description, menu labels → `content/site.ts`

## 10. Preview drafts on a test URL (optional)
In Vercel, set `SHOW_DRAFTS=true` **only for the Preview environment**. Preview deployments then show drafts; production never does.
