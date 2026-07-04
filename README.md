# Twitter Analytics Dashboard

A Power BI dashboard analyzing tweet-level performance data, built as an internship extension of a training project. All six internship tasks were implemented as additional report pages on top of the original analytics dashboard, using the same dataset throughout.

## Dataset

`Tweet.csv` — a tweet-level Twitter analytics export, 1,181 rows spanning June 1 – October 19, 2020. Each row is a single tweet with metrics including impressions, engagements, engagement rate, retweets, replies, likes, media views/engagements, and click-through counts (URL clicks, profile clicks, hashtag clicks). The dataset has no author/user identity field — every row is from a single account's own tweet history, not a multi-user dataset.

## Files

- `Twitter Dashboard.pbix` — the full Power BI report (7 pages)
- `Tweet.csv` — source dataset

## Report Pages

1. **Twitter Analytics Overview** — original training dashboard (KPI cards, gauges, trend charts)
2. **Tweet Interaction by Category** — clustered bar chart of URL/profile/hashtag clicks grouped by tweet category (Media / Link / Hashtag), filtered to tweets with word count > 20, even day-of-month, and at least one interaction type, visible only 3–5 PM IST
3. **Engagement Rate Comparison** — average engagement rate for tweets with vs. without app opens, filtered by business-hours weekday timing, even impressions, odd date, character count > 30, and exclusion of tweets containing "d"; visible 12–6 PM or 7–11 AM IST
4. **Media Interaction by Weekday** — dual-axis (line + column) chart of media views and media engagements by day of week for Q3 2020, filtered by even impressions, odd date, character count > 30, and exclusion of tweets containing "h"; visible 3–5 PM or 7–11 AM IST
5. **Engagement Comparison** — simple SUM comparison of replies, retweets, and likes for tweets posted June–August 2020
6. **Monthly Engagement Trend** — average engagement rate by month, split by tweets with vs. without media content
7. **Top 10 Tweets** — top 10 tweets by combined retweets + likes, excluding weekends, filtered by even impressions, odd date, and word count < 30; visible 3–5 PM IST

## Key Findings & Assumptions

- **Tweet category** (Media / Link / Hashtag) is inferred from interaction metrics, since the dataset has no explicit content-type field: `media views > 0` → Media, else `url clicks > 0` → Link, else Hashtag. This is a proxy and slightly undercounts true categories (a tweet could have a link that received zero clicks).
- **"Last quarter"** (Task 3) is interpreted as Q3 2020 (Jul–Sep), the last *complete* quarter in the dataset, rather than Q4 (Oct), which is only a partial month with far less data.
- **Word count thresholds** were adjusted where the literal task spec returned zero rows on this dataset (e.g., >40 words never occurs; longest tweet is 36 words) — thresholds were lowered and documented rather than left producing an empty chart.
- **Task 2 (Engagement Rate Comparison) is correctly blank at all times.** Independently verified: zero tweets in the dataset satisfy all five combined filter conditions simultaneously. This reflects a genuine data limitation (app opens occur in only 2 of 1,166 tweets), not a build error.
- **Task 6's "user profile" requirement** could not be literally fulfilled — the dataset has no author/user identity column. Tweet text is used as the row identifier instead.
- **Timestamps convert to IST** based on the report's data model (`ConvertedDateTime.2`), confirmed by independently cross-checking filtered row counts against the raw dataset in Python.
- **Visibility-window filters** (e.g., "only visible 3–5 PM IST") are implemented via DAX measures using `UTCNOW()` converted to IST. These re-evaluate on report refresh; in Power BI Desktop this means pressing refresh, and once published to the Service, "Page refresh" can be enabled to update automatically on a schedule.

## Tools

Power BI Desktop (Power Query / M for transformations, DAX for measures and time-based visibility logic)