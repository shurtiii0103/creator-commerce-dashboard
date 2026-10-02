# DAX Measure Catalogue

All 56 measures live in the `_measures` table and calculate over the single flat table `tiktok_shop_viral_video_sales_attribution_cac` (one row per video session). The only relationships are Power BI's auto date/time tables.

Generated from `Creator Commerce Dashboard.SemanticModel/definition/tables/_measures.tmdl`.

## Key patterns

- **Session grain**: every row is one video session, so volume measures are plain `SUM`s and `Video Sessions` is `COUNTROWS`.
- **Rates from 0–100 columns**: source rate columns (view rate, completion, engagement, commission) are stored as 0–100, so the measures divide by 100 and use a `%` format.
- **Modelled CAC**: the data has only 2 purchases, so `Avg CAC (modelled)` averages the dataset's own `cac` column instead of Spend ÷ Orders.
- **Month-over-month KPI comparison**: the data covers a single year (2025), so KPI cards compare the latest month in the selection with the month before. `REMOVEFILTERS` on the auto date table lets the comparison see the previous month even when the Month slicer excludes it.
- **KPI card labels**: every KPI has a `Change Label` (▲/▼ %) and a hidden `Change Color` (green = good direction, red = bad, grey = neutral KPI). `KPI Context` builds the caption (e.g. "Full year 2025 · Dec vs Nov:").
- **Benchmarks & colour rules**: benchmark measures recalculate the same metric across `ALLSELECTED` categories (dashed reference lines); colour measures return hex codes for conditional formatting (red > 2% above the benchmark, dark > 2% below, grey within ±2%).

## Summary

| Folder | Measure | Format | Description |
|---|---|---|---|
| 0. KPI Labels | KPI Period Label *(hidden)* | `—` | Months in the current selection, e.g. 'Full year 2025', 'Oct–Dec 2025'. |
| 0. KPI Labels | KPI Context *(hidden)* | `—` | KPI caption: period + which months are compared, e.g. 'Full year 2025 · Dec vs Nov:'. |
| 0. KPI Labels | Orders Context *(hidden)* | `—` | Orders card caption with conversion rate prefix. |
| 1. Overview | Video Sessions | `#,0` | Row count; one row = one video session record. |
| 1. Overview | Campaign Spend | `\$#,0` | Total campaign spend, USD. |
| 1. Overview | Video Views | `#,0` | Total video views. |
| 1. Overview | Orders | `#,0` | Sessions that ended in a purchase (purchase_flag). |
| 1. Overview | Conversion Rate | `0.000%` | Orders ÷ video sessions. |
| 1. Overview | Avg CAC (modelled) | `\$#,0` | Average of the dataset-supplied CAC per row (not Spend ÷ Orders). |
| 1. Overview\KPI Labels | Campaign Spend Change Label | `—` | Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January. |
| 1. Overview\KPI Labels | Video Views Change Label | `—` | Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January. |
| 1. Overview\KPI Labels | Orders Change Label | `—` | Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January. |
| 1. Overview\KPI Labels | Avg CAC (modelled) Change Label | `—` | Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January. |
| 1. Overview\Formatting | Avg CAC Benchmark | `\$#,0` | Avg CAC across all traffic sources / regions in the current selection (reference line). |
| 1. Overview\Formatting | CAC vs Avg Colour *(hidden)* | `—` | Bar colour: red if >2% above the benchmark, dark if >2% below, grey within ±2%. |
| 1. Overview\Formatting | Campaign Spend Change Color *(hidden)* | `—` | Delta colour: spend is neutral, so always grey. |
| 1. Overview\Formatting | Video Views Change Color *(hidden)* | `—` | Delta colour by good direction (up): green = good, red = bad. |
| 1. Overview\Formatting | Orders Change Color *(hidden)* | `—` | Delta colour by good direction (up): green = good, red = bad. |
| 1. Overview\Formatting | Avg CAC (modelled) Change Color *(hidden)* | `—` | Delta colour by good direction (down): green = good, red = bad. |
| 1. Overview\Funnel | Product Clicks | `#,0` | Funnel step 2: sessions with a product click. |
| 1. Overview\Funnel | Product Page Views | `#,0` | Funnel step 3: sessions with a product page view. |
| 1. Overview\Funnel | Add to Cart | `#,0` | Funnel step 4: sessions with an add to cart. |
| 1. Overview\Funnel | Checkout Started | `#,0` | Funnel step 5: sessions that started checkout. |
| 2. Content & Hooks | Avg Hook Strength | `0.0" / 100"` | Average hook strength score (0–100). |
| 2. Content & Hooks | 3-Second View Rate | `0.0%` | Average 3-second view rate (source column is 0–100). |
| 2. Content & Hooks | Full Completion Rate | `0.0%` | Average full-video completion rate (source column is 0–100). |
| 2. Content & Hooks | Viral Rate | `0.0%` | % of sessions flagged viral (any tier above Normal). |
| 2. Content & Hooks | Engagement Rate | `0.0%` | Row-average of the per-video engagement rate (source column is 0–100). |
| 2. Content & Hooks | Viral Videos | `#,0` | Sessions whose viral tier is above Normal. |
| 2. Content & Hooks\KPI Labels | Avg Hook Strength Change Label | `—` | Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January. |
| 2. Content & Hooks\KPI Labels | 3-Second View Rate Change Label | `—` | Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January. |
| 2. Content & Hooks\KPI Labels | Full Completion Rate Change Label | `—` | Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January. |
| 2. Content & Hooks\KPI Labels | Viral Rate Change Label | `—` | Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January. |
| 2. Content & Hooks\Formatting | Engagement Rate Benchmark | `0.0%` | Engagement rate across all viral tiers in the current selection (reference line). |
| 2. Content & Hooks\Formatting | Engagement Tier Colour *(hidden)* | `—` | Grey for the Normal tier, dark for viral tiers. |
| 2. Content & Hooks\Formatting | Viral Rate Benchmark | `0.0%` | Viral rate across all video formats in the current selection (reference line). |
| 2. Content & Hooks\Formatting | Viral Rate Colour *(hidden)* | `—` | Dark if at/above the average viral rate, grey if below. |
| 2. Content & Hooks\Formatting | Avg Hook Strength Change Color *(hidden)* | `—` | Delta colour by good direction (up): green = good, red = bad. |
| 2. Content & Hooks\Formatting | 3-Second View Rate Change Color *(hidden)* | `—` | Delta colour by good direction (up): green = good, red = bad. |
| 2. Content & Hooks\Formatting | Full Completion Rate Change Color *(hidden)* | `—` | Delta colour by good direction (up): green = good, red = bad. |
| 2. Content & Hooks\Formatting | Viral Rate Change Color *(hidden)* | `—` | Delta colour by good direction (up): green = good, red = bad. |
| 3. Creators & Cost | Cost per 1K Views | `\$#,0.00` | Campaign spend per 1,000 video views. |
| 3. Creators & Cost | Return Rate | `0.0%` | % of sessions flagged as returned. |
| 3. Creators & Cost | Avg Creator Commission | `0.0%` | Average creator commission rate (source column is 0–100). |
| 3. Creators & Cost | Share of Spend | `0.0%` | Tier's % of total campaign spend in the current selection. |
| 3. Creators & Cost | Share of Views | `0.0%` | Tier's % of total video views in the current selection. |
| 3. Creators & Cost\KPI Labels | Engagement Rate Change Label | `—` | Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January. |
| 3. Creators & Cost\KPI Labels | Cost per 1K Views Change Label | `—` | Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January. |
| 3. Creators & Cost\KPI Labels | Return Rate Change Label | `—` | Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January. |
| 3. Creators & Cost\KPI Labels | Avg Creator Commission Change Label | `—` | Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January. |
| 3. Creators & Cost\Formatting | Return Rate Benchmark | `0.0%` | Return rate across all product categories in the current selection (reference line). |
| 3. Creators & Cost\Formatting | Return Rate Colour *(hidden)* | `—` | Bar colour: red if >2% above the average return rate, dark if >2% below, grey within ±2%. |
| 3. Creators & Cost\Formatting | Engagement Rate Change Color *(hidden)* | `—` | Delta colour by good direction (up): green = good, red = bad. |
| 3. Creators & Cost\Formatting | Cost per 1K Views Change Color *(hidden)* | `—` | Delta colour by good direction (down): green = good, red = bad. |
| 3. Creators & Cost\Formatting | Return Rate Change Color *(hidden)* | `—` | Delta colour by good direction (down): green = good, red = bad. |
| 3. Creators & Cost\Formatting | Avg Creator Commission Change Color *(hidden)* | `—` | Delta colour: commission is neutral, so always grey. |

## Definitions

### 0. KPI Labels

#### KPI Period Label

Months in the current selection, e.g. 'Full year 2025', 'Oct–Dec 2025'.

```dax
KPI Period Label =
    VAR _first = MIN ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _n = DISTINCTCOUNT ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    RETURN
        SWITCH (
            TRUE (),
            ISBLANK ( _last ), "No data for this selection",
            _n = 12, "Full year 2025",
            _last - _first + 1 = _n,
                IF ( _n = 1,
                    FORMAT ( DATE ( 2025, _last, 1 ), "mmm" ) & " 2025",
                    FORMAT ( DATE ( 2025, _first, 1 ), "mmm" ) & "–" & FORMAT ( DATE ( 2025, _last, 1 ), "mmm" ) & " 2025" ),
            _n & " months, 2025"
        )
```

#### KPI Context

KPI caption: period + which months are compared, e.g. 'Full year 2025 · Dec vs Nov:'.

```dax
KPI Context =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    RETURN
        [KPI Period Label] & " · "
            & IF ( _last = 1, "Jan: no prior month",
                FORMAT ( DATE ( 2025, _last, 1 ), "mmm" ) & " vs " & FORMAT ( DATE ( 2025, _last - 1, 1 ), "mmm" ) & ":" )
```

#### Orders Context

Orders card caption with conversion rate prefix.

```dax
Orders Context =
    "Conv. " & FORMAT ( [Conversion Rate], "0.000%" ) & " · " & [KPI Context]
```

### 1. Overview

#### Video Sessions

Row count; one row = one video session record.

```dax
Video Sessions =
    COUNTROWS ( 'tiktok_shop_viral_video_sales_attribution_cac' )
```

#### Campaign Spend

Total campaign spend, USD.

```dax
Campaign Spend =
    SUM ( 'tiktok_shop_viral_video_sales_attribution_cac'[campaign_spend] )
```

#### Video Views

Total video views.

```dax
Video Views =
    SUM ( 'tiktok_shop_viral_video_sales_attribution_cac'[video_views] )
```

#### Orders

Sessions that ended in a purchase (purchase_flag).

```dax
Orders =
    SUM ( 'tiktok_shop_viral_video_sales_attribution_cac'[purchase_flag] )
```

#### Conversion Rate

Orders ÷ video sessions.

```dax
Conversion Rate =
    DIVIDE ( [Orders], [Video Sessions] )
```

#### Avg CAC (modelled)

Average of the dataset-supplied CAC per row (not Spend ÷ Orders).

```dax
Avg CAC (modelled) =
    AVERAGE ( 'tiktok_shop_viral_video_sales_attribution_cac'[cac] )
```

### 1. Overview\KPI Labels

#### Campaign Spend Change Label

Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January.

```dax
Campaign Spend Change Label =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Campaign Spend], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Campaign Spend], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( _last <= 1, BLANK (), IF ( ISBLANK ( _d ), "n/a", IF ( _d >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _d ), "0.0%" ) ) )
```

#### Video Views Change Label

Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January.

```dax
Video Views Change Label =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Video Views], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Video Views], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( _last <= 1, BLANK (), IF ( ISBLANK ( _d ), "n/a", IF ( _d >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _d ), "0.0%" ) ) )
```

#### Orders Change Label

Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January.

```dax
Orders Change Label =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Orders], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Orders], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( _last <= 1, BLANK (), IF ( ISBLANK ( _d ), "n/a", IF ( _d >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _d ), "0.0%" ) ) )
```

#### Avg CAC (modelled) Change Label

Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January.

```dax
Avg CAC (modelled) Change Label =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Avg CAC (modelled)], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Avg CAC (modelled)], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( _last <= 1, BLANK (), IF ( ISBLANK ( _d ), "n/a", IF ( _d >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _d ), "0.0%" ) ) )
```

### 1. Overview\Formatting

#### Avg CAC Benchmark

Avg CAC across all traffic sources / regions in the current selection (reference line).

```dax
Avg CAC Benchmark =
    CALCULATE ( [Avg CAC (modelled)], ALLSELECTED ( 'tiktok_shop_viral_video_sales_attribution_cac'[traffic_source] ), ALLSELECTED ( 'tiktok_shop_viral_video_sales_attribution_cac'[region] ) )
```

#### CAC vs Avg Colour

Bar colour: red if >2% above the benchmark, dark if >2% below, grey within ±2%.

```dax
CAC vs Avg Colour =
    VAR _r = DIVIDE ( [Avg CAC (modelled)], [Avg CAC Benchmark] ) RETURN SWITCH ( TRUE (), _r > 1.02, "#FE2C55", _r < 0.98, "#161823", "#C4C4C4" )
```

#### Campaign Spend Change Color

Delta colour: spend is neutral, so always grey.

```dax
Campaign Spend Change Color =
    "#757780"
```

#### Video Views Change Color

Delta colour by good direction (up): green = good, red = bad.

```dax
Video Views Change Color =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Video Views], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Video Views], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( ISBLANK ( _d ), "#757780", IF ( _d >= 0, "#008C87", "#FE2C55" ) )
```

#### Orders Change Color

Delta colour by good direction (up): green = good, red = bad.

```dax
Orders Change Color =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Orders], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Orders], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( ISBLANK ( _d ), "#757780", IF ( _d >= 0, "#008C87", "#FE2C55" ) )
```

#### Avg CAC (modelled) Change Color

Delta colour by good direction (down): green = good, red = bad.

```dax
Avg CAC (modelled) Change Color =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Avg CAC (modelled)], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Avg CAC (modelled)], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( ISBLANK ( _d ), "#757780", IF ( _d >= 0, "#FE2C55", "#008C87" ) )
```

### 1. Overview\Funnel

#### Product Clicks

Funnel step 2: sessions with a product click.

```dax
Product Clicks =
    SUM ( 'tiktok_shop_viral_video_sales_attribution_cac'[product_click_flag] )
```

#### Product Page Views

Funnel step 3: sessions with a product page view.

```dax
Product Page Views =
    SUM ( 'tiktok_shop_viral_video_sales_attribution_cac'[product_page_view_flag] )
```

#### Add to Cart

Funnel step 4: sessions with an add to cart.

```dax
Add to Cart =
    SUM ( 'tiktok_shop_viral_video_sales_attribution_cac'[add_to_cart_flag] )
```

#### Checkout Started

Funnel step 5: sessions that started checkout.

```dax
Checkout Started =
    SUM ( 'tiktok_shop_viral_video_sales_attribution_cac'[checkout_started_flag] )
```

### 2. Content & Hooks

#### Avg Hook Strength

Average hook strength score (0–100).

```dax
Avg Hook Strength =
    AVERAGE ( 'tiktok_shop_viral_video_sales_attribution_cac'[hook_strength_score] )
```

#### 3-Second View Rate

Average 3-second view rate (source column is 0–100).

```dax
3-Second View Rate =
    AVERAGE ( 'tiktok_shop_viral_video_sales_attribution_cac'[three_second_view_rate] ) / 100
```

#### Full Completion Rate

Average full-video completion rate (source column is 0–100).

```dax
Full Completion Rate =
    AVERAGE ( 'tiktok_shop_viral_video_sales_attribution_cac'[full_video_completion_rate] ) / 100
```

#### Viral Rate

% of sessions flagged viral (any tier above Normal).

```dax
Viral Rate =
    DIVIDE ( SUM ( 'tiktok_shop_viral_video_sales_attribution_cac'[viral_flag] ), [Video Sessions] )
```

#### Engagement Rate

Row-average of the per-video engagement rate (source column is 0–100).

```dax
Engagement Rate =
    AVERAGE ( 'tiktok_shop_viral_video_sales_attribution_cac'[engagement_rate] ) / 100
```

#### Viral Videos

Sessions whose viral tier is above Normal.

```dax
Viral Videos =
    CALCULATE ( [Video Sessions], 'tiktok_shop_viral_video_sales_attribution_cac'[viral_tier] <> "Normal" )
```

### 2. Content & Hooks\KPI Labels

#### Avg Hook Strength Change Label

Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January.

```dax
Avg Hook Strength Change Label =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Avg Hook Strength], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Avg Hook Strength], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( _last <= 1, BLANK (), IF ( ISBLANK ( _d ), "n/a", IF ( _d >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _d ), "0.0%" ) ) )
```

#### 3-Second View Rate Change Label

Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January.

```dax
3-Second View Rate Change Label =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [3-Second View Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [3-Second View Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( _last <= 1, BLANK (), IF ( ISBLANK ( _d ), "n/a", IF ( _d >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _d ), "0.0%" ) ) )
```

#### Full Completion Rate Change Label

Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January.

```dax
Full Completion Rate Change Label =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Full Completion Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Full Completion Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( _last <= 1, BLANK (), IF ( ISBLANK ( _d ), "n/a", IF ( _d >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _d ), "0.0%" ) ) )
```

#### Viral Rate Change Label

Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January.

```dax
Viral Rate Change Label =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Viral Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Viral Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( _last <= 1, BLANK (), IF ( ISBLANK ( _d ), "n/a", IF ( _d >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _d ), "0.0%" ) ) )
```

### 2. Content & Hooks\Formatting

#### Engagement Rate Benchmark

Engagement rate across all viral tiers in the current selection (reference line).

```dax
Engagement Rate Benchmark =
    CALCULATE ( [Engagement Rate], ALLSELECTED ( 'tiktok_shop_viral_video_sales_attribution_cac'[Viral Tier] ) )
```

#### Engagement Tier Colour

Grey for the Normal tier, dark for viral tiers.

```dax
Engagement Tier Colour =
    IF ( SELECTEDVALUE ( 'tiktok_shop_viral_video_sales_attribution_cac'[Viral Tier] ) = "Normal", "#C4C4C4", "#161823" )
```

#### Viral Rate Benchmark

Viral rate across all video formats in the current selection (reference line).

```dax
Viral Rate Benchmark =
    CALCULATE ( [Viral Rate], ALLSELECTED ( 'tiktok_shop_viral_video_sales_attribution_cac'[video_format] ) )
```

#### Viral Rate Colour

Dark if at/above the average viral rate, grey if below.

```dax
Viral Rate Colour =
    IF ( [Viral Rate] >= [Viral Rate Benchmark], "#161823", "#C4C4C4" )
```

#### Avg Hook Strength Change Color

Delta colour by good direction (up): green = good, red = bad.

```dax
Avg Hook Strength Change Color =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Avg Hook Strength], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Avg Hook Strength], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( ISBLANK ( _d ), "#757780", IF ( _d >= 0, "#008C87", "#FE2C55" ) )
```

#### 3-Second View Rate Change Color

Delta colour by good direction (up): green = good, red = bad.

```dax
3-Second View Rate Change Color =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [3-Second View Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [3-Second View Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( ISBLANK ( _d ), "#757780", IF ( _d >= 0, "#008C87", "#FE2C55" ) )
```

#### Full Completion Rate Change Color

Delta colour by good direction (up): green = good, red = bad.

```dax
Full Completion Rate Change Color =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Full Completion Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Full Completion Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( ISBLANK ( _d ), "#757780", IF ( _d >= 0, "#008C87", "#FE2C55" ) )
```

#### Viral Rate Change Color

Delta colour by good direction (up): green = good, red = bad.

```dax
Viral Rate Change Color =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Viral Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Viral Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( ISBLANK ( _d ), "#757780", IF ( _d >= 0, "#008C87", "#FE2C55" ) )
```

### 3. Creators & Cost

#### Cost per 1K Views

Campaign spend per 1,000 video views.

```dax
Cost per 1K Views =
    DIVIDE ( [Campaign Spend], [Video Views] ) * 1000
```

#### Return Rate

% of sessions flagged as returned.

```dax
Return Rate =
    DIVIDE ( SUM ( 'tiktok_shop_viral_video_sales_attribution_cac'[return_flag] ), [Video Sessions] )
```

#### Avg Creator Commission

Average creator commission rate (source column is 0–100).

```dax
Avg Creator Commission =
    AVERAGE ( 'tiktok_shop_viral_video_sales_attribution_cac'[creator_commission_rate] ) / 100
```

#### Share of Spend

Tier's % of total campaign spend in the current selection.

```dax
Share of Spend =
    DIVIDE ( [Campaign Spend], CALCULATE ( [Campaign Spend], ALLSELECTED ( 'tiktok_shop_viral_video_sales_attribution_cac'[Creator Tier] ) ) )
```

#### Share of Views

Tier's % of total video views in the current selection.

```dax
Share of Views =
    DIVIDE ( [Video Views], CALCULATE ( [Video Views], ALLSELECTED ( 'tiktok_shop_viral_video_sales_attribution_cac'[Creator Tier] ) ) )
```

### 3. Creators & Cost\KPI Labels

#### Engagement Rate Change Label

Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January.

```dax
Engagement Rate Change Label =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Engagement Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Engagement Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( _last <= 1, BLANK (), IF ( ISBLANK ( _d ), "n/a", IF ( _d >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _d ), "0.0%" ) ) )
```

#### Cost per 1K Views Change Label

Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January.

```dax
Cost per 1K Views Change Label =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Cost per 1K Views], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Cost per 1K Views], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( _last <= 1, BLANK (), IF ( ISBLANK ( _d ), "n/a", IF ( _d >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _d ), "0.0%" ) ) )
```

#### Return Rate Change Label

Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January.

```dax
Return Rate Change Label =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Return Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Return Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( _last <= 1, BLANK (), IF ( ISBLANK ( _d ), "n/a", IF ( _d >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _d ), "0.0%" ) ) )
```

#### Avg Creator Commission Change Label

Latest month in selection vs the month before, e.g. '▲ 4.2%'. Blank for January.

```dax
Avg Creator Commission Change Label =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Avg Creator Commission], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Avg Creator Commission], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( _last <= 1, BLANK (), IF ( ISBLANK ( _d ), "n/a", IF ( _d >= 0, "▲ ", "▼ " ) & FORMAT ( ABS ( _d ), "0.0%" ) ) )
```

### 3. Creators & Cost\Formatting

#### Return Rate Benchmark

Return rate across all product categories in the current selection (reference line).

```dax
Return Rate Benchmark =
    CALCULATE ( [Return Rate], ALLSELECTED ( 'tiktok_shop_viral_video_sales_attribution_cac'[product_category] ) )
```

#### Return Rate Colour

Bar colour: red if >2% above the average return rate, dark if >2% below, grey within ±2%.

```dax
Return Rate Colour =
    VAR _r = DIVIDE ( [Return Rate], [Return Rate Benchmark] ) RETURN SWITCH ( TRUE (), _r > 1.02, "#FE2C55", _r < 0.98, "#161823", "#C4C4C4" )
```

#### Engagement Rate Change Color

Delta colour by good direction (up): green = good, red = bad.

```dax
Engagement Rate Change Color =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Engagement Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Engagement Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( ISBLANK ( _d ), "#757780", IF ( _d >= 0, "#008C87", "#FE2C55" ) )
```

#### Cost per 1K Views Change Color

Delta colour by good direction (down): green = good, red = bad.

```dax
Cost per 1K Views Change Color =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Cost per 1K Views], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Cost per 1K Views], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( ISBLANK ( _d ), "#757780", IF ( _d >= 0, "#FE2C55", "#008C87" ) )
```

#### Return Rate Change Color

Delta colour by good direction (down): green = good, red = bad.

```dax
Return Rate Change Color =
    VAR _last = MAX ( 'tiktok_shop_viral_video_sales_attribution_cac'[month] )
    VAR _cur = CALCULATE ( [Return Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last )
    VAR _prev = CALCULATE ( [Return Rate], REMOVEFILTERS ( 'LocalDateTable_60f65ef6-83ce-48c1-bc68-761d858ba15d' ), 'tiktok_shop_viral_video_sales_attribution_cac'[month] = _last - 1 )
    VAR _d = DIVIDE ( _cur - _prev, ABS ( _prev ) )
    RETURN IF ( ISBLANK ( _d ), "#757780", IF ( _d >= 0, "#FE2C55", "#008C87" ) )
```

#### Avg Creator Commission Change Color

Delta colour: commission is neutral, so always grey.

```dax
Avg Creator Commission Change Color =
    "#757780"
```
