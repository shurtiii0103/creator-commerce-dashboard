# Creator Commerce · Viral Video Sales Attribution & CAC

A Power BI dashboard analysing **100,000 TikTok Shop video sessions (Jan–Dec 2025)**. It tracks campaign spend, reach, what makes a video hold attention and go viral, and how creator tiers compare on cost and returns.

Built as a **PBIP project** (PBIR report + TMDL model), so every page, visual and DAX measure is plain text and diff-able in git.

> All data is synthetic. No real creators, customers or brands.

![Overview page](docs/screenshots/overview.png)

## Report pages

| Page | Question it answers | Visuals |
|---|---|---|
| **Overview** | Where is the money going, and how far does it reach? | 4 KPI cards (Campaign Spend, Video Views, Orders, Avg CAC) · Spend & video sessions by month (drill to quarter) · Video views by market region · Spend by campaign type · Avg CAC by traffic source vs average |
| **Content & Hooks** | What makes a video hold attention and go viral? | 4 KPI cards (Hook Strength, 3-Second View Rate, Completion Rate, Viral Rate) · Retention by hook-strength band · Engagement rate by viral tier · Viral rate by video format · Viral videos by month and tier |
| **Creators & Cost** | Which creator tiers are worth the money? | 4 KPI cards (Engagement Rate, Cost per 1K Views, Return Rate, Creator Commission) · Engagement & viral rate by creator tier · Share of spend vs share of views by tier · Avg CAC by market region · Return rate by product category |
| **Notes** | How is everything calculated? | Measure definitions, how to read the KPI colours, data definitions and caveats |

All pages share one header and navigation bar, plus a sidebar of synced slicers (**Year, Quarter, Month, Market Region, Creator Tier, Campaign Type**) with a **Reset filters** button. Every chart cross-filters the rest of its page.

## Key findings

- **Reach is concentrated in big creators:** Mega creators get ~5% of spend but deliver ~62% of video views. Nano creators get ~25% of spend for ~0.1% of views.
- **Small creators engage best:** Nano creators reach ~23.5% engagement and ~19.6% viral rate, against ~3.8% / ~3.7% for Mega creators.
- **Hooks matter:** going from the weakest hooks (0–20) to the strongest (80–100), 3-second view rate rises from ~50% to ~91% and full completion from ~15% to ~28%. Viral rate stays flat at ~12.5–13%, so a strong hook keeps viewers watching but doesn't make a video go viral.
- **Spend ramps into Q4:** monthly sessions grow from ~5.7K in January to ~17K in December, and December spend is up 17% on November.
- **Almost no sales:** only 2 purchases in 100,000 sessions (0.002% conversion). CAC is therefore shown as the dataset's **modelled** CAC (avg ~$27.9K), not Spend ÷ Orders.

## How the KPI cards work

Each card shows the value for the current selection, then compares the latest month in the selection with the month before, e.g. `Full year 2025 · Dec vs Nov: ▲ 21.0%`. The delta is coloured by **good direction**:

| Colour | Meaning |
|---|---|
| 🟩 Teal `#008C87` | Moved the right way (views up, CAC down …) |
| 🟥 Red `#FE2C55` | Moved the wrong way |
| ⬜ Grey `#757780` | Neutral KPI (spend, commission) |

Bar charts use the same idea against a dashed benchmark line: **red** is more than 2% worse than average, **grey** is within ±2%, and **dark** is better than average.

## Data model

- **One flat table:** `tiktok_shop_viral_video_sales_attribution_cac`, at one row per video session with 402 columns. There's no star schema and no custom date table; it relies on Power BI's auto date/time hierarchy on `date`.
- **`_measures` table:** holds all 56 DAX measures, organised into display folders per page (base KPIs, `KPI Labels`, `Formatting`).
- **6 calculated columns:** hook-strength bands, plus sort-order copies for Creator Tier and Viral Tier (avoiding a sort-by circular dependency).
- **Colour measures:** these return hex codes that drive conditional formatting, so the colour rules live in the model rather than in each visual.

See **[measures.md](measures.md)** for every measure with its DAX, and **[data-dictionary.md](data-dictionary.md)** for the columns used.

## Design

- **Theme:** a custom TikTok-inspired theme (`CreatorCommerce`) applied report-wide, so no default Power BI blue appears.

  | Role | Colour |
  |---|---|
  | Ink / primary series | `#161823` |
  | Accent (lines, benchmarks) | `#20D5EC` |
  | Highlight / bad | `#FE2C55` |
  | Aqua | `#25F4EE` |
  | Good | `#008C87` |
  | Neutral | `#C4C4C4` |
  | Page | `#F1F1F2` |

- **Font:** Segoe UI.
- **Canvas:** 1920 × 1080, fit to page.
- **Wireframe:** the report was built 1:1 from an interactive HTML mockup, [`creator_commerce_wireframe.html`](creator_commerce_wireframe.html). Open it in a browser to click through the intended layout.

## Repository structure

```text
.
├── Creator Commerce Dashboard.pbip              # open this in Power BI Desktop
├── Creator Commerce Dashboard.Report/           # PBIR report: pages, visuals, theme
│   ├── definition/pages/<page>/visuals/<visual>/visual.json
│   └── StaticResources/RegisteredResources/CreatorCommerce-*.json   # theme
├── Creator Commerce Dashboard.SemanticModel/    # TMDL model
│   └── definition/tables/_measures.tmdl         # all DAX measures
├── creator_commerce_wireframe.html              # interactive HTML mockup
├── measures.md                                  # DAX measure catalogue
├── data-dictionary.md                           # column definitions
└── README.md
```

## Getting the data

The source CSV (`tiktok_shop_viral_video_sales_attribution_cac.csv`, **213 MB**) is too large for GitHub (100 MB file limit), so it is **not in the repo** and `.gitignore` excludes `*.csv`.

1. Download the dataset and save it in the repo root as `tiktok_shop_viral_video_sales_attribution_cac.csv`.
2. Open `Creator Commerce Dashboard.pbip` in Power BI Desktop.
3. The query points to the original author's local path. Update it under **Transform data → select the table → Source step**, choosing your copy of the CSV.
4. Click **Refresh**.

## Requirements

- **Power BI Desktop:** a recent build. The PBIP/PBIR format must be enabled under *File → Options → Preview features → Power BI Project (.pbip) save option* and *Store reports using enhanced metadata format (PBIR)*.
- **Disk space:** about 300 MB free for the data and model cache.

## Tools used

- **Power BI Desktop:** PBIP, PBIR and TMDL.
- **DAX:** all measures, including month-over-month logic, benchmarks and colour rules.
- **HTML / JS mockup:** used to design and agree the layout before building.
- **Power BI Modeling MCP and the `powerbi-report-author` CLI:** used for measure authoring, PBIR generation and validation.
