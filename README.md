# AI Career Navigator

**A client-side career intelligence console.** Tell it where you are — by resume, by paragraph, or by conversation — and it charts three scored routes forward: a safe path, a growth path, and a full career-change path, complete with skill-gap analysis, a learning roadmap, and a realistic 3 / 6 / 12-month plan.

Everything runs **entirely in the browser**. No accounts, no servers, no data leaves the device.

---

## What it does

### 1 · Multi-input profile system
Three ways in, freely combinable — the input adapts to you, not the other way round:

- **Resume upload** — PDF, DOCX, DOC or TXT, parsed locally with [pdf.js](https://github.com/mozilla/pdf.js) and [mammoth](https://github.com/mwilliamson/mammoth.js). Extracts roles, dates → experience span, education level & field, industry, tools, certifications, projects, and measurable achievements. Detects and redacts emails/phone numbers.
- **Free-form text** — paste anything in your own words (`"BSc in Biotechnology, two years in QC, I want a less physical role and I'm into data"`). A natural-language extractor maps it onto the career vocabulary: education, experience, industry, interests, direction, salary targets, deal-breakers — with **confidence tracking** (● confirmed vs ◐ inferred, tagged by source).
- **Conversational assessment** — adaptive one-group-at-a-time questions that skip anything already answered by your resume or text, and only ask what materially improves the recommendation.

All inputs merge into one **unified Career Profile**. Corrections work in plain words (*"my experience is actually 3 years"*, *"I prefer remote"*, *"remove that"*) or via click-to-edit on the live profile panel. When sources conflict, the navigator flags it and lets you decide — your direct statement always outranks the document.

### 2 · Matching engine
A deterministic, transparent scoring engine (no LLM required):

- **17 occupation charts** with demand, salary bands, entry difficulty, remote potential, education gates, skill requirements and progression ladders
- **42-skill vocabulary** with transferability metadata
- **9 weighted factors** — skills overlap, experience, interest, entry effort, education, transferables, demand, work style, reward — published weights, every score arguable
- Routes are categorised into **Path A · Safe**, **Path B · Growth**, **Path C · Change** based on real proximity, not labels

### 3 · The report
- Animated route chart with fit dials, ramp estimates scaled to your weekly learning hours, and salary bands
- **Gap board**: already have / partially developed / missing / optional × MUST–SHOULD–NICE
- Resume-based evidence buckets: demonstrated / likely transferable / needs verification / missing
- **Career signal** — what your resume currently reads like, compared against your heading, with repositioning guidance
- 6-stage learning roadmap, gap-driven course & certification picks (deduped, free-first, only when they close a real gap)
- 3 / 6 / 12-month plan, job strategy with copyable LinkedIn headline and CV keywords, readiness meter
- **Reality check** — risks and mitigations, no sugar-coating, no false promises
- **Ongoing coach console** — answers questions from your own data

---

## Tech stack

| Layer | Tool |
|---|---|
| UI | React 18 + TypeScript |
| Build | Vite |
| Styling | Tailwind CSS v4 |
| Document parsing | `pdfjs-dist` (PDF), `mammoth` (DOCX), best-effort binary extraction (legacy DOC) |
| Fonts | Space Grotesk (display) · IBM Plex Sans / Mono (body & data) |

Lazy-loaded parser chunks keep the initial bundle lean — PDF/DOCX parsing only loads if you upload.

## Getting started

```bash
npm install
npm run dev      # local development
npm run build    # production build (dist/)
```

## Project structure

```
src/
├── engine/
│   ├── types.ts      # shared domain types
│   ├── careers.ts    # skill catalog, labels, 17 career charts, course library
│   ├── engine.ts     # scoring, gap analysis, roadmap, coach logic
│   ├── extract.ts    # free-text & correction NLP, shared vocabularies
│   ├── resume.ts     # file readers, resume parsing, evidence buckets
│   └── flow.ts       # adaptive waypoint flow, profile summary, patches
├── components/
│   ├── Assessment.tsx  # multi-input conversation console
│   ├── Report.tsx      # route chart & full report dashboard
│   ├── ProfilePanel.tsx# live profile with confidence marks + inline edits
│   ├── Plotting.tsx    # route-plotting sequence
│   ├── icons.tsx       # custom SVG icon set
│   └── ui.tsx          # reveal, dial, bars, primitives
└── App.tsx           # phase machine + instrument shell
```

## Privacy

Resumes are parsed **in-browser only** — files are never uploaded anywhere. Contact details are detected and redacted from display. Nothing is stored after the session.

## An honest note

Fit scores are transparent **heuristic estimates** from a rules engine running in your browser — deliberately not guarantees of employment or salary. The design philosophy follows one principle throughout: *never assume a person starts from zero*. Existing education, experience and transferable skills are mapped first; only the smallest realistic gap is charted. Verify local demand with live job postings before committing to any route.
