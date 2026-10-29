# Adding a portfolio project

Every new piece of work needs a **catalog file** in content. A **custom case study page** also needs a component folder and one line in the registry.

You do **not** edit `src/pages/projects/[slug].astro` for a new slug.

## Quick pick

| Goal | Do this |
|------|---------|
| “Coming soon” card + placeholder page | Path **A** only |
| Short story in Markdown | Path **A** with MDX body + `status: published` |
| Full designed case study (like Clarity) | Path **A** + **B** |

---

## Path A — Catalog (always)

1. Create `src/content/projects/{slug}.mdx`. The file name is the URL: `/projects/{slug}/`.

2. Fill frontmatter (see `src/content.config.ts` for allowed fields):

```yaml
---
title: Project name
summary: One line for cards and SEO.
year: 2026
role: Product design
tags:
  - Tag one
order: 2
status: stub
seo:
  title: Optional page title
  description: Optional meta description
---
```

3. Optional: add a **cover** image in frontmatter when you have art for the work grid.

4. Optional: write the **body** below the frontmatter for a simple MDX story (used when there is no custom component in the registry).

The home **Selected work** list reads this collection automatically (`order`, then `year`).

---

## Path B — Custom case study page

Use when the story needs bespoke sections, not just paragraphs.

1. Copy the shape of **Clarity**:
   - Folder: `src/components/projects/clarity/`
   - Composer: `ClarityCaseStudy.astro` (assembles sections)
   - Reuse blocks from `src/components/case/` (`CaseChapter`, `CaseBand`, `CasePlatePair`, `MetaStrip`, …)

2. Create `src/components/projects/{slug}/` with your composer (e.g. `FareClarityCaseStudy.astro`).

3. Register in **`registry.ts`** in this folder:

```ts
import YourCaseStudy from '../components/projects/{slug}/YourCaseStudy.astro';

export const projectViews = {
  clarity: ClarityCaseStudy,
  '{slug}': YourCaseStudy,
} as const;
```

4. Your composer must accept the same props as Clarity:

- `year` (number)
- `role` (string)
- `next` (`{ href, title } | null` — “next case study” link)

5. Set `status: published` in the MDX frontmatter when the page is ready.

**Clarity catalog-only MDX:** `src/content/projects/clarity.mdx` has frontmatter only; the page UI lives entirely in components.

---

## Optional site touches

- **Home marquee:** add the project title to the `reel` array in `src/pages/index.astro`.

---

## Checklist before you ship

- [ ] `npm run build` passes
- [ ] `/projects/{slug}/` looks right (stub, MDX, or custom)
- [ ] Card on home **Work** section shows correct title, summary, and order
- [ ] If custom: registry entry slug matches MDX file name (without `.mdx`)

---

## Where things live

| What | Where |
|------|--------|
| Card data + route slug | `src/content/projects/{slug}.mdx` |
| Slug → custom page | `src/projects/registry.ts` |
| Default page template | `src/components/ProjectStub.astro` |
| Shared case-study UI | `src/components/case/` |
| One project’s UI | `src/components/projects/{slug}/` |
