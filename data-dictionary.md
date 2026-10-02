# Data Dictionary — `tiktok_shop_viral_video_sales_attribution_cac`

**Source:** `tiktok_shop_viral_video_sales_attribution_cac.csv` (213 MB, 402 columns, comma-delimited). Not committed to the repo; see [README](README.md#getting-the-data).
**Grain:** one row per video session (a unique session, video, campaign and customer). 100,000 rows, 1 Jan – 31 Dec 2025.
**Model:** a single flat import table. The only relationships are Power BI's auto date/time tables on `date` and `timestamp`. There are no dimension tables. Measures live in a separate `_measures` table (see [measures.md](measures.md)).

> The data is flagged as **synthetic** (`data_type = "synthetic"`). No real creators, customers or brands are included.

## Columns used in the report

Only about 25 of the 402 columns drive the dashboard. Every other column is loaded but unused.

### Date

| Column | Type | Description | Values / range |
|---|---|---|---|
| date | Date | Session date. Its auto date hierarchy (Year › Quarter › Month) feeds the slicers and the trend axis. | 1 Jan – 31 Dec 2025 |
| month | Int64 | Month number. Used by the KPI month-over-month comparison. | 1 – 12 |

### Dimensions (slicers and chart axes)

| Column | Type | Description | Values |
|---|---|---|---|
| region | Text | Customer market region (not the creator's home region) | Asia-Pacific, Europe, Latin America, Middle East, North America |
| creator_tier | Text | Creator size by follower count (use `Creator Tier` in visuals for the correct sort) | Nano, Micro, Mid-Tier, Macro, Mega |
| campaign_type | Text | Campaign objective type | Awareness, Conversions, Creator Sales, Engagement, Retargeting, Sales, Traffic |
| traffic_source | Text | Where the session came from | Creator, Organic, Paid, Referral, Viral |
| video_format | Text | Video content format | 14 formats (Before & After, Comedy, Tutorial, Unboxing …) |
| product_category | Text | Product category promoted | 19 categories (Beauty, Electronics, Fashion, Toys …) |
| viral_tier | Text | Dataset-supplied virality label (use `Viral Tier` in visuals for the correct sort) | Normal, Emerging, Trending, Viral, Mega Viral |
| data_type | Text | Data provenance flag | synthetic |

### Metrics

| Column | Type | Description | Range |
|---|---|---|---|
| campaign_spend | Decimal | Campaign spend attributed to the session (USD) | 37.65 – 103,892.99 |
| video_views | Int64 | Views of the video | 10 – 695.8M |
| purchase_flag | Int64 | 1 if the session ended in a purchase (only 2 in the whole dataset) | 0 / 1 |
| cac | Decimal | Dataset-modelled customer acquisition cost (USD) | 31.42 – 236,788.44 |
| hook_strength_score | Decimal | Strength of the video's opening hook | 0 – 100 |
| three_second_view_rate | Decimal | % of viewers still watching at 3 seconds | 12 – 99 (stored as 0–100) |
| full_video_completion_rate | Decimal | % of viewers who watched to the end | 2 – 57.2 (stored as 0–100) |
| engagement_rate | Decimal | Engagements ÷ views for the video | 0 – 65.3 (stored as 0–100) |
| viral_flag | Int64 | 1 for any viral tier above Normal | 0 / 1 |
| return_flag | Int64 | 1 if the order was returned | 0 / 1 |
| creator_commission_rate | Decimal | Creator commission | 3 – 30 (stored as 0–100) |
| product_click_flag, product_page_view_flag, add_to_cart_flag, checkout_started_flag | Int64 | Session reached that step of the path to purchase | 0 / 1 |
| data_quality_flag | Int64 | 0 marks rows flagged for quality issues (1,955 rows; included in the report) | 0 / 1 |

## Calculated columns (DAX)

| Column | Expression | Purpose |
|---|---|---|
| Hook Band Order *(hidden)* | `MIN ( INT ( [hook_strength_score] / 20 ), 4 ) + 1` | Sort key for `Hook Strength Band` |
| Hook Strength Band | `SWITCH ( MIN ( INT ( [hook_strength_score] / 20 ), 4 ), 0, "0–20", 1, "20–40", … "80–100" )` | 20-point bands for the retention chart |
| Creator Tier Order *(hidden)* | `SWITCH ( [creator_tier], "Nano", 1, "Micro", 2, "Mid-Tier", 3, "Macro", 4, "Mega", 5, 9 )` | Sort key |
| Creator Tier | `[creator_tier]` | Display copy sorted by `Creator Tier Order` |
| Viral Tier Order *(hidden)* | `SWITCH ( [viral_tier], "Normal", 1, "Emerging", 2, "Trending", 3, "Viral", 4, "Mega Viral", 5, 9 )` | Sort key |
| Viral Tier | `[viral_tier]` | Display copy sorted by `Viral Tier Order` |

> **Why the copy columns?** Sorting `creator_tier` by a calculated column derived from `creator_tier` itself creates a circular dependency. The display copies (`Creator Tier`, `Viral Tier`) carry the sort instead, and the original columns are left unchanged.

## Known data quirks

- **Duplicate columns:** several metrics appear more than once (e.g. `cac` = `customer_acquisition_cost` = `campaign_cac`; `video_views` = `video_views_1`). The report uses the first of each.
- **Near-zero sales:** with only 2 purchases, the revenue, profit, ROAS and ROI columns are effectively zero and are not used.
- **Near-uniform splits:** being synthetic, many categories (campaign type, region, traffic source) are almost evenly distributed. The strongest signals are hook strength, creator tier and product-category returns.
