# Creator Commerce Dashboard · Built with Agentic AI Development

A four-page **Power BI** dashboard on TikTok Shop creator commerce (viral video sales attribution and CAC), taken from a raw 402-column CSV to a documented, version-controlled project using **Claude as an AI development agent**. I set the direction, constraints and reviews. Claude profiled the data, prototyped, built, debugged and documented it using real tools (HTML, Power BI Modeling MCP, PBIR report authoring CLI).

![Development cycle](images/development-cycle.png)

---

## 📊 The dashboard

<!-- PLACEHOLDER: upload Power BI screenshots to /images with these exact names -->
| Overview | Content & Hooks |
|---|---|
| ![Overview](images/dashboard-overview.png) | ![Content & Hooks](images/dashboard-content-hooks.png) |

| Creators & Cost | Notes |
|---|---|
| ![Creators & Cost](images/dashboard-creators-cost.png) | ![Notes](images/dashboard-notes.png) |

**Page 1: Overview.** KPIs: Campaign Spend · Video Views · Orders · Avg CAC (modelled), each compared with the previous month.
Visuals: campaign spend and video sessions by month (quarter → month drill) · video views by market region · spend by campaign type · avg CAC by traffic source vs average.

**Page 2: Content & Hooks.** KPIs: Avg Hook Strength · 3-Second View Rate · Full Completion Rate · Viral Rate.
Visuals: retention by hook-strength band · engagement rate by viral tier · viral rate by video format · viral videos by month and tier.

**Page 3: Creators & Cost.** KPIs: Avg Engagement Rate · Cost per 1K Views · Return Rate · Avg Creator Commission.
Visuals: engagement and viral rate by creator tier · share of spend vs share of views by tier · avg CAC by market region · return rate by product category.

**Page 4: Notes.** Measure definitions, how to read the KPI colours, data definitions and caveats.

**Filters (synced across pages):** Year · Quarter · Month · Market Region · Creator Tier · Campaign Type · Reset.

### 💡 What the data says
- **Big creators buy reach, small creators buy engagement.** Mega creators take ~5% of spend but deliver ~62% of views. Nano creators take ~25% of spend for ~0.1% of views, yet engage best (~23.5% engagement and ~19.6% viral rate, vs ~3.8% / ~3.7% for Mega).
- **Hooks keep people watching, but don't make videos go viral.** From the weakest hooks (0–20) to the strongest (80–100), the 3-second view rate rises from ~50% to ~91% and full completion from ~15% to ~28%. Viral rate stays flat at ~12.5–13%.
- **Spend ramps into Q4.** Sessions grow from ~5.7K in January to ~17K in December.
- **Almost no sales.** There are only 2 purchases in 100,000 sessions, so CAC uses the dataset's *modelled* CAC column instead of Spend ÷ Orders.

---

## 🤖 How I built it: agentic development with Claude

I treated Claude as a developer on my team. I wrote the brief, set the constraints and approved each stage. Claude did the hands-on work using tools connected to my environment.

### 1 · Data
The source is a synthetic TikTok Shop dataset: **100,000 rows × 402 columns**, one row per video session (Jan–Dec 2025), covering creators, products, campaigns, video performance, hooks, retention, the path to purchase, attribution, CAC and costs. Claude profiled it to agree the grain and pick the ~25 columns that matter. It also flagged the caveats up front: duplicate columns, near-uniform synthetic splits, and only **2 purchases** in the whole file.

### 2 · Prototype: interactive HTML wireframe
Claude built a **clickable HTML prototype** on the real data: working slicers, cross-filtering charts, tooltips, a month/quarter toggle and page navigation. It used Power BI's 1920×1080 canvas, TikTok brand colours, and only visuals that exist natively in Power BI, so it could be rebuilt 1:1. The prototype also documented every planned DAX measure on a Notes page.

<!-- PLACEHOLDER: HTML wireframe screenshots -->
| Prototype · Overview | Prototype · Content & Hooks |
|---|---|
| ![HTML wireframe page 1](images/wireframe-html-page1.png) | ![HTML wireframe page 2](images/wireframe-html-page2.png) |

▶️ Try it: download [`creator_commerce_wireframe.html`](creator_commerce_wireframe.html) and open it in any browser.

### 3 · Build: Power BI (PBIP)
My constraints: *flat table only, all measures in a `_measures` table, no model changes without asking, and it must look identical to the wireframe.* Claude used the **Power BI Modeling MCP** (connected live to Power BI Desktop) and the **PBIR report-authoring CLI** to produce:
- **The model:** a single import table plus a `_measures` table.
- **Asked first, then built:** before touching the model, Claude stopped and asked about the gaps the wireframe exposed. Hook-strength bands and a logical sort order for creator/viral tiers both need calculated columns. I approved **6 calculated columns**, using display copies for the sort so the HR project's circular-dependency bug couldn't recur ([data-dictionary.md](data-dictionary.md)).
- **56 DAX measures** in display folders: base KPIs, month-over-month KPI captions and ▲/▼ labels, benchmarks, and hex-colour measures for conditional formatting ([measures.md](measures.md)).
- **Numbers checked live:** every measure was tested with DAX queries against the open Desktop model before any visual was built.
- **4 pages, written as PBIR files by a generator script** that encodes the wireframe's positions, colours and fonts. They are validated with **0 errors**, and every field reference is checked against the model.

### 4 · Review and iterate
I reviewed the build in Power BI Desktop and sent screenshots. Claude applied the changes:
- **"Keep the colours consistent, no blue, use TikTok colours."** Claude added a custom `CreatorCommerce` theme (ink `#161823`, cyan `#20D5EC`, red `#FE2C55`, aqua `#25F4EE`, teal `#008C87`), so no default Power BI blue appears anywhere.
- **"The funnel chart doesn't make sense."** With only 2 purchases, a path-to-purchase funnel was mostly empty bars. It was replaced with **Video Views by Market Region**, which fits the Overview's spend-and-reach story.
- **"Make the Notes page prettier."** It was redesigned as styled cards: one measure card per page, a colour legend for the KPI deltas, and Definitions and Data caveats panels.

### 5 · Debug
- **Funnel visual needs a category.** Power BI's funnel visual can't take separate measures with no category field. This was found through CLI metadata before rendering, and it fed into dropping the funnel.
- **Single year, no previous-year comparison.** The data only covers 2025, so the KPI cards compare the latest month in the selection with the month before. `REMOVEFILTERS` on the auto date table keeps the comparison working even when the Month slicer excludes the prior month.
- **Validator catches.** Invalid legend enum values and a theme-name mismatch were caught by `powerbi-report-author validate` and fixed before Desktop ever saw them.

### 6 · Document
Claude wrote this documentation and generated the measure catalogue directly from the model's TMDL. It also set up `.gitignore` so the 213 MB source CSV stays out of GitHub.

---

## 💡 What this demonstrates

- **Agentic AI workflow:** an AI agent working through real tools (MCP servers, CLIs, file system, a live Power BI Desktop session), not just chat answers.
- **Human-in-the-loop control:** my brief, constraints and approval at every stage. Model changes were proposed and approved, never assumed.
- **Design-first BI:** profile → prototype → build → review, so feedback lands before development cost.
- **Power BI as code:** PBIP / TMDL / PBIR files, generated by script and reviewable in Git.
- **DAX:** month-over-month KPI labels and good-direction colours, `ALLSELECTED` benchmarks, colour-returning measures, share-of-total.
- **Honest analytics:** surfacing that the data has almost no sales, and designing around it instead of hiding it.

---

## 📁 Repository

| File / folder | What it is |
|---|---|
| `Creator Commerce Dashboard.pbip` | Open this in Power BI Desktop |
| `Creator Commerce Dashboard.Report/` | Report definition (PBIR: pages, visuals, custom theme) |
| `Creator Commerce Dashboard.SemanticModel/` | Model definition (TMDL: table, calculated columns, measures) |
| `creator_commerce_wireframe.html` | Interactive prototype |
| [`measures.md`](measures.md) | DAX measure catalogue |
| [`data-dictionary.md`](data-dictionary.md) | Columns used, calculated columns, data quirks |
| `images/` | Screenshots and diagrams |

> The source CSV (`tiktok_shop_viral_video_sales_attribution_cac.csv`, 213 MB) is **not in the repo**. It's over GitHub's 100 MB file limit.

## ▶️ How to open it

1. Install **Power BI Desktop** and enable the PBIP / TMDL / PBIR preview features (*Options → Preview features*).
2. Clone or download this repo, then put `tiktok_shop_viral_video_sales_attribution_cac.csv` in the repo folder.
3. Open `Creator Commerce Dashboard.pbip`.
4. Go to **Transform data**, select the table, and update the file path in the **Source** step to where the CSV is saved on your machine.
5. **Refresh.**

## 🛠 Tools

Power BI Desktop (PBIP · TMDL · PBIR) · DAX · Power Query · HTML/CSS/JS · Node.js · **Claude** (Power BI Modeling MCP, `powerbi-report-author` CLI) · Git/GitHub

> All data is synthetic. No real creators, customers or brands. Built as a self-learning portfolio project.
