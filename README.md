# MyBagRADAR

  **Daily price, quality, and liquidity intelligence for the secondhand luxury-handbag market in South Korea & Brazil — built to produce an econometric, data-driven investment score.**

  🌐 Live: [MyBagRADAR.com](https://mybagradar.com) · 🌍 Trilingual: English · 한국어 · Português

  ![MyBagRADAR home page](./assets/homepage.jpg)

  ---

  ## What it is

  MyBagRADAR scrapes **authentication-certified resale platforms** in two of the world's most active secondhand luxury markets — **South Korea** and **Brazil** — every day. It tracks how much pre-owned designer handbags are being asked for, scores the quality of each listing, and records how long each bag stays on the market before it disappears (a proxy for a sale).

  Over time this builds a **longitudinal panel**: the same listings observed day after day. That panel is the foundation for answering the questions buyers and resellers actually care about:

  - **Which brands and models hold their value best?**
  - **Which bags are the most *liquid*** — i.e. sell quickly — for both the seller today and the buyer who may want to resell tomorrow?
  - **What is a fair price right now**, across platforms and across two countries?

  The end goal is an **investment score grounded in econometric models** estimated directly from the panel — not opinion, not vibes.

  ---

  ## The vision

  I'm building MyBagRADAR because **I want to help people make better decisions with their money** — and that includes myself.

  I'm a fan of the resale market. I've **bought handbags second-hand**, and I've **resold some of them too**, so I know first-hand how confusing and opaque this market can be: prices are all over the place, "is this a good investment?" usually gets answered with a gut feeling, and almost no one can tell you how *quickly* a given bag will actually sell.

  My goal is to change that. I want anyone — whether you're buying your first pre-loved bag, deciding what to resell, or treating bags as a real asset class — to be able to **navigate this market with data instead of guesswork**. If MyBagRADAR helps even one person avoid overpaying, or sell faster, or choose a bag that actually holds its value, it's doing its job.

  In the future I want MyBagRADAR to be the place people trust to answer one simple question: *"Is this bag a smart decision for my money — and how easily could I get that money back?"*

  ---

  ## Why it exists

  The luxury resale market is opaque. Prices for the same bag vary wildly across platforms, "investment" advice is mostly anecdotal, and almost no one tracks **time-to-sale** — the single best signal of real demand. MyBagRADAR was built to replace that guesswork with a reproducible, daily-updated dataset and transparent methodology.

  ---

  ## How it works

  1. **Daily scraping (real data only).** Once per country per day, the system collects active handbag listings from a curated set of authenticated platforms. No simulated or fabricated data — ever.
  2. **Strict "bags only" filtering.** Wallets, card-holders, key-rings, and other small leather goods are excluded by design, so the dataset stays clean for like-for-like comparison.
  3. **Quality scoring.** Every listing gets a 0–100 quality score from its condition, photos, and listing completeness (Premium / Good / Average / Fair).
  4. **Longitudinal panel.** One observation is written per listing per day — even when nothing changes — so price trajectories and survival times are fully reconstructable.
  5. **Sold detection & survival analysis.** When a listing disappears for several consecutive scrapes, it is marked *presumed sold*. This yields a **time-to-sale** (duration) and a sold/censored event flag — exactly the inputs a survival model (Kaplan–Meier, Cox proportional hazards) needs to measure **liquidity** per brand and model.
  6. **Cross-platform & cross-country comparison.** Identical bags are clustered across platforms so price dispersion ("the law of one price") can be measured directly.

  ---

  ## The research goal: a data-driven investment score

  MyBagRADAR currently ships an initial **quality score** and **investment score**. The next phase is a **deep econometric investigation** to estimate that investment score rather than assume it, using the panel:

  - **Hedonic price models** — what each attribute (brand, model, size, material, condition, accessories, age) contributes to price.
  - **Value-retention models** — resale price as a fraction of original retail, and what drives it.
  - **Survival / liquidity models** — time-to-sale by brand and model, identifying which bags are easiest to exit.

  Combining these into a single, defensible **investment score** is the core research contribution. The methodology is being developed toward academic publication standards (reproducible exports, documented covariates, honest treatment of censoring and selection).

  ---

  ## Coverage

  - **Markets:** South Korea 🇰🇷 and Brazil 🇧🇷
  - **Platforms:** 10 authentication-certified resale marketplaces (peer-to-peer / non-authenticated marketplaces are excluded by policy)
  - **Brands:** 25 maisons — Hermès, Chanel, Louis Vuitton, Dior, Gucci, Prada, Bottega Veneta, Celine, Loewe, The Row, Goyard, Delvaux, Saint Laurent, Fendi, Balenciaga, Burberry, Ferragamo, Valentino, Versace, Chloé, Miu Miu, Tod's, Mulberry, Jacquemus, Marc Jacobs

  ---

  ## Status

  - **Started:** April 2026
  - **Now:** actively collecting data daily; the longitudinal panel grows every day
  - **Next:** deepen the econometric investment-score research described above

  ---

  ## Architecture

  MyBagRADAR is a **pnpm monorepo** with a clean separation between deployable apps and shared libraries, built **contract-first**: a single OpenAPI specification is the source of truth, and typed clients (React Query hooks) plus runtime validators (Zod) are **code-generated** from it — so the frontend and backend can never silently drift apart.

  ```
  ┌─────────────────────────────────────────────────────────────┐
  │  Frontend (React + Vite + Tailwind)   ← trilingual EN/KO/PT  │
  │  TanStack Query · wouter · Recharts · generated typed hooks   │
  └───────────────┬──────────────────────────────────────────────┘
                  │  OpenAPI contract → Orval codegen (hooks + Zod)
  ┌───────────────▼──────────────────────────────────────────────┐
  │  API server (Express 5, ~30 route modules)                    │
  │  search · compare · invest · rankings · analytics · export    │
  │  AI search (SSE) · admin · auth (Clerk) · cron scheduler      │
  └───────┬───────────────────────────────┬──────────────────────┘
          │                               │
  ┌───────▼─────────────┐      ┌──────────▼───────────────────────┐
  │  PostgreSQL          │      │  Data pipeline                    │
  │  Drizzle ORM         │      │  per-platform scrapers →          │
  │  + pgvector (HNSW)   │      │  guardrails → attribute extractors│
  │  longitudinal panel  │      │  → quality scoring → quarantine   │
  │  + price history     │      │  sweep → daily CSV export to repo │
  └──────────────────────┘      └───────────────────────────────────┘
  ```

  **Layers**

  - **Frontend** — React + Vite + TailwindCSS, `wouter` routing, TanStack Query for data, Recharts for price/trend charts, full trilingual EN / KO / PT i18n.
  - **API** — Express 5 with ~30 route modules (search, compare, invest, rankings, analytics, exports, AI search, admin, auth, methodology). Authentication via **Clerk**.
  - **Database** — PostgreSQL + **Drizzle ORM**, with **pgvector** (HNSW cosine index) for semantic search. The schema is instrumented for research: a daily longitudinal panel, price history, survival/sold-event fields, cross-platform clusters, FX history, and macro indicators.
  - **Data pipeline** — per-platform scrapers → defense-in-depth guardrails (category / price floor / ceiling) → deterministic + AI attribute extractors → quality scoring → reversible quarantine sweep → daily CSV exports auto-published to the research-dataset repo.
  - **Scheduler** — a full cron suite (daily scrapes per country, price-history backfill, brand snapshots, cross-platform dedup, relisting detection, macro pulls, dataset publishing, outreach waves, anomaly auto-heal).

  ---

  ## AI & LLM models

  MyBagRADAR uses a **multi-model strategy** — the right model for each job — accessed through the Replit AI Integrations proxy and direct provider APIs:

  | Model | Provider | What it does here |
  |-------|----------|-------------------|
  | **Gemini 2.5 Flash** | Google | Extracts structured attributes from free-text listings; parses natural-language search queries ("describe the bag"); generates per-listing relevance rationale (streamed). |
  | **Gemini 2.5 Flash (Vision)** | Google | Identifies brand/model from an uploaded photo (image search). |
  | **GPT-4o Vision** | OpenAI | Fallback image identification when the vision pass is low-confidence. |
  | **text-embedding-3-small** | OpenAI | 1536-dim embeddings powering semantic similarity search over `pgvector` (HNSW cosine). |
  | **Claude Sonnet 4.6** | Anthropic | The Investment Advisor — reasons over real price/retention/liquidity data to produce buy/hold/skip guidance and answer follow-up questions. |
  | **Claude Haiku 4.5** | Anthropic | Fast, cheap "is this actually a handbag?" validation to keep the dataset clean. |
  | **Perplexity Sonar** | Perplexity | Real-time retail-price research **with citations** (current boutique prices for the new-vs-resale projections). |

  > **Cost-aware by design.** Deterministic regex/keyword extractors always run first; an LLM is only invoked when the rules can't resolve an attribute. Results are cached (a persistent extraction cache keyed by content hash), so re-scrapes cost effectively zero, and new listings are embedded in batches.

  Offline research tooling (one-off brand-catalog and alias research) additionally uses **Claude Opus** and **GPT-4o-mini**.

  ---

  ## External APIs & services

  - **ScraperAPI** — primary scraping transport for the resale platforms.
  - **Bright Data** — Web Unlocker for bot-protected sites (e.g. Farfetch / Akamai), a browser/CDP session for JavaScript-heavy platforms, and residential proxying.
  - **open.er-api.com** — daily KRW / BRL / USD foreign-exchange rates (persisted for reproducibility).
  - **Resend** — transactional email (contact form, partner outreach).
  - **Clerk** — user authentication and admin gating.
  - **GitHub REST API** — automated daily publishing of the research dataset to the companion repo.
  - **Naver & Google Search** — retail-price discovery for anchoring resale against original price.
  - **Farfetch** — boutique-benchmark medians and live in-stock comparisons.

  ---

  ## How it was built

  This is not a weekend project. MyBagRADAR is the product of **thousands of carefully iterated prompts** and deliberate engineering decisions, layered over months: designing per-platform scrapers that survive anti-bot defenses, encoding strict data-integrity rules (real data only, bags only, reversible quarantine — never deletion), building the multi-model AI stack above, and instrumenting the database for serious econometric research rather than just display.

  Every fix, recalibration, and methodology decision is documented and dated, because the goal is a dataset and a scoring methodology that can stand up to **academic scrutiny** — reproducible, transparent, and honest about its limitations.

  ---

  ## Tech stack (summary)

  - **Languages/runtime:** TypeScript, Node.js (pnpm workspaces)
  - **Frontend:** React · Vite · TailwindCSS · TanStack Query · wouter · Recharts
  - **API:** Express 5 · OpenAPI + Orval codegen · Zod validation
  - **Database:** PostgreSQL · Drizzle ORM · pgvector
  - **AI:** Gemini 2.5 Flash · OpenAI embeddings + GPT-4o vision · Claude Sonnet/Haiku · Perplexity Sonar
  - **Infra/services:** ScraperAPI · Bright Data · Clerk · Resend · GitHub API
  - **Data science:** longitudinal panel · survival analysis (Kaplan–Meier / Cox PH) · hedonic & value-retention models (reference scripts in Stata, R, Python)

  ---

  ## Research dataset

  The cleaned CSV exports that power the analysis (price panel, survival panel, cross-section for regression, brand trends, FX history, and more) are refreshed daily into a companion repository:

  ➡️ **[`ingridleiria/mybagradar-data`](https://github.com/ingridleiria/mybagradar-data)** *(private research dataset)*

  ---

  ## Author

  Built by **[Ingrid Leiria](https://kr.linkedin.com/in/ingrid-leiria-25b4767a)** — Chief of Staff and an advocate for AI enablement at her current company, currently pursuing a **PhD in Economics (Applied Microeconometrics) in South Korea**.

  She's also a reseller-market enthusiast exploring whether luxury handbags can be evaluated as a genuine, measurable asset class — so that people can make smarter, data-backed decisions with their money.

  🔗 Connect on [LinkedIn](https://kr.linkedin.com/in/ingrid-leiria-25b4767a)

  ---

  *MyBagRADAR is committed to real, transparent data: every figure traces back to a real listing on a real, authenticated platform.*
  