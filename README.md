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

  ## Tech stack

  - **Frontend:** React + Vite + TailwindCSS (trilingual EN / KO / PT)
  - **API:** Node.js + Express
  - **Database:** PostgreSQL + Drizzle ORM (with `pgvector` for AI semantic search)
  - **AI:** natural-language "describe the bag" search and an investment advisor
  - **Data science:** longitudinal panel, survival analysis, and hedonic/value-retention models (reference scripts in Stata, R, and Python)

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
  