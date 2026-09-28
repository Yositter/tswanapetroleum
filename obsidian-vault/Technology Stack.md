# Technology Stack: Tswana Petroleum

A council recommendation for the Tswana Petroleum website. Prepared 2026-09-28.
Corporate Social Responsibility is treated as a first-class requirement, woven through
both content and engineering choices, not appended as a page.

---

## Starting point

The current site is a single self-contained `index.html`: inline design-token CSS with a
dark mode, a vanilla-JavaScript IIFE, a JavaScript-generated SVG corridor map, accessible
tabs, a mobile menu, and a quote form that validates and confirms entirely client side. It
is fast, accessible, and grounded in South African regulation (POPIA, PAIA, B-BBEE, SARS
diesel refund, SANS specifications, SADC geography).

The virtues to protect: near-zero JavaScript weight, strong accessibility, and legal
grounding. The gaps to close: the quote and onboarding-pack forms reach no backend, content
is hard-coded in markup, there is no CSR presence, and there is no build, test, or delivery
pipeline.

---

## The council verdicts

**Frontend Architect.** Adopt Astro. It is static-first, ships zero JavaScript by default,
and the existing hand-written HTML, CSS, and SVG move in almost verbatim. Promote the
hard-coded products, sectors, corridors, and pricing rows into Astro content collections so
they become editable data. Keep the current token-based CSS as global styles. Use
TypeScript. Reserve React or Svelte islands only for the interactive map, tabs, and form.

**Backend and Platform.** The site needs one thin server surface: a form endpoint for the
quote and onboarding-pack requests. Use Cloudflare Pages with Pages Functions or Workers.
No standing server, no container, no idle cost. Server-side validation mirrors the existing
client rules, persists the request, and notifies the sales desk.

**Security and Compliance.** Data residency drives the choice. Store any personal
information in the Cape Town region (`af-south-1`) or a Cloudflare region control set to
keep data in-jurisdiction, in step with POPIA. Replace any Google reCAPTCHA instinct with
Cloudflare Turnstile: privacy-preserving and consent-light. Enforce a Content Security
Policy, security headers, and Subresource Integrity. Keep the POPIA notice on the form and
wire the PAIA and privacy footer links to real documents.

**DevOps and Reliability.** GitHub Actions builds, lints, tests, and deploys to Cloudflare
Pages with a preview deployment for every pull request. Gate merges on an accessibility
check, a Lighthouse performance budget, and a page-weight budget. Add Sentry for error
monitoring and use Cloudflare analytics for delivery health.

**Data and Analytics.** Use cookieless, privacy-first analytics: Cloudflare Web Analytics
or Plausible. No consent banner, no cross-site tracking, minimal data collection. This is
both a compliance posture and a CSR posture.

**CSR and Sustainability Lead.** Responsibility is expressed twice: in what the site says
and in how the site is built. See the dedicated layer below. The engineering choices above
already lower energy per visit and widen access on constrained networks, which is the
point.

---

## Recommended stack

| Layer | Choice | Rationale |
|---|---|---|
| Framework | **Astro** + TypeScript | Static-first, zero-JS default, preserves current performance. |
| Styling | Existing token CSS as global styles; Tailwind optional | The current design system is strong. Do not discard it. |
| Interactivity | Astro islands (map, tabs, form) in vanilla or Svelte | Ship interactivity only where it is used. |
| Component sourcing | **21st.dev Magic MCP** (already configured) | Generate new UI blocks on demand as islands. |
| Content | Astro content collections (products, sectors, corridors, pricing) | Move data out of markup into typed, editable files. |
| CMS (phase 2) | **Keystatic** (git-based) | Non-technical editing with data kept in the repository. |
| Hosting and CDN | **Cloudflare Pages** + Workers | Edge presence near SADC users, renewable-matched power, thin serverless. |
| Forms and email | Pages Functions + **Amazon SES (af-south-1)** or Resend | Server-side capture and sales notification, data kept in-region. |
| Anti-spam | **Cloudflare Turnstile** | Privacy-preserving, POPIA-friendly, no Google tracking. |
| Analytics | **Cloudflare Web Analytics** or **Plausible** | Cookieless, consent-light, minimal data. |
| Testing | **Vitest** (unit), **Playwright** (form, tabs, map) | Protect the interactive parts against regression. |
| Quality gates | ESLint, Prettier, Stylelint, **Pa11y/axe**, **Lighthouse CI** | Performance and accessibility are features here. |
| CI/CD | **GitHub Actions** to Cloudflare Pages, preview per PR | Automated, reviewable, reversible. |
| Observability | **Sentry** + Cloudflare analytics | Errors and delivery health. |
| Security | CSP, security headers, SRI, Dependabot | Baseline hardening. |

Deliberately excluded for now: a heavyweight React meta-framework, a database server, an
authentication system, and cookie-based analytics. Each adds cost and data-protection
surface the current business does not need. A customer portal for delivery tickets,
reconciliation statements, and SARS logbooks is a genuine phase-three project and would
reintroduce authentication and a database when justified.

---

## The CSR layer

Corporate Social Responsibility is delivered as content, as reporting, and as engineering.

### 1. Content and programs
A dedicated CSR and Sustainability section and page covering:
- Local economic development, and enterprise and supplier development under B-BBEE. The
  company already holds a B-BBEE certificate: surface it.
- Skills development, learnerships, and driver and safety training.
- Road safety, which ties naturally to the Road Accident Fund levy already explained in the
  pricing section.
- Environmental stewardship: spill prevention, the low-sulphur 10 ppm diesel already sold,
  and cleaner combustion.
- Governance and ethics: an anti-bribery stance, a supplier code of conduct, and the SHEQ
  policy already referenced in the onboarding pack.

### 2. Reporting and transparency
- Publish a human-readable and machine-readable sustainability report. Align disclosures to
  a recognised framework: GRI, or the JSE and IFRS S1 and S2 sustainability standards for
  listing readiness.
- Add schema.org Organization and sustainability structured data for search and machine
  consumption.

### 3. Green engineering as responsibility
- Static-first Astro means minimal server compute and low energy per visit.
- Choose renewable-powered hosting and show it via the Green Web Foundation directory.
- Add a page-carbon budget to CI using Website Carbon or CO2.js, alongside the performance
  budget.
- Subset and self-host the Archivo font to cut payload, with a system-font fallback.

### 4. Digital inclusion as responsibility
- Conform to WCAG 2.2 AA. The current site is already strong here: hold the line in CI.
- Treat low-bandwidth performance as a social goal, not only a technical one. Fuel buyers
  across the SADC region often browse on constrained, metered mobile data and low-end
  devices. A light page is an accessible page.
- Prepare multilingual delivery with Astro internationalisation: English first, then
  Portuguese for Mozambique and Angola, and French for the DRC, matching the corridor map.
- Add a service worker so the static shell loads offline or on flaky connections.

### 5. Data ethics
- Cookieless analytics, Turnstile over reCAPTCHA, and minimal quote-form data collection,
  with the POPIA notice kept in place and the PAIA and privacy links made real.

---

## Phased roadmap

1. **Migrate and harden.** Move the single file into Astro with content collections, keep
   the design intact, and add lint, accessibility, performance, and carbon budgets in CI.
   Deploy to Cloudflare Pages.
2. **Make the forms real.** Add the Pages Function endpoint, SES or Resend email in-region,
   and Turnstile. Persist requests. Add Playwright coverage.
3. **Publish CSR.** Add the CSR section and page, structured data, and the first
   sustainability report. Enable cookieless analytics.
4. **Editing and languages.** Introduce Keystatic for staff editing and Astro
   internationalisation for the priority languages.
5. **Portal, when justified.** A customer portal for tickets, reconciliations, and logbooks,
   with authentication and a database, when volume warrants it.

---

## Decisions that would change this recommendation
- **Do staff need to edit content without a developer soon?** If yes, bring Keystatic into
  phase one rather than phase four.
- **Is a customer portal on the near horizon?** If yes, the framework and hosting choice
  should be revisited toward a fuller application platform.
- **Is a specific reporting framework already mandated** (GRI, JSE, IFRS S1 and S2)? The
  answer sets the shape of the CSR report.
