# 📱 App Insights Unlocked: Google Play Store Analytics (Power BI)

> An end-to-end data analytics case study on 8,000+ Google Play Store apps, built with **Power Query + DAX + Power BI**. It answers 25 business questions across basic, medium and advanced levels and ends with actionable recommendations for an app-development company.

## Problem Statement: 
Your task is to analyze this dataset to uncover patterns and trends that can guide the 
company's decision-making process. The analysis should focus on understanding what factors 
contribute to high app ratings, identifying the most popular app categories, and exploring the 
impact of app size and price on user reviews and installs. 

---

##  Table of Contents
1. [Business Problem](#-business-problem)
2. [Dataset](#-dataset)
3. [Tools & Skills](#-tools--skills)
4. [Data Cleaning & Preprocessing](#-data-cleaning--preprocessing)
5. [Data Model & DAX Measures](#-data-model--dax-measures)
6. [Dashboard Overview](#-dashboard-overview)
7. [Key Insights](#-key-insights).
8. [Key Recommendations](#-key-recommendations)
9. [Challenges Faced](#-challenges-faced).

---

## 🎯 Business Problem
A mobile-app company has acquired Google Play Store data. Management wants to know:
- What factors drive an app's success (ratings, installs, reviews)?
- How can the company use those factors to improve its own apps?
- What do the data say about user preferences and behaviour?

**Stakeholders:** App Developers, Product Managers, Marketing Team, Senior Management (internal); App Users, Advertisers, Partners (external).

---

## Dataset
- **Source:** [Kaggle: Google Play Store Apps](https://www.kaggle.com/datasets/lava18/google-play-store-apps?select=googleplaystore.csv)
- **File:** `googleplaystore.csv`
- **After cleaning:** **8,190 rows** (8,174 distinct app names)

| Column | Description |
|---|---|
| App | Name of the app |
| Category | Category of the app |
| Rating | Average user rating (0–5) |
| Reviews | Number of user reviews |
| Size | Size of the app |
| Installs | Number of installs (e.g. 1,000+) |
| Type | Free or Paid |
| Price | Price of the app |
| Content Rating | Age group the app is appropriate for |
| Genres | Genre(s) of the app |
| Last Updated | Date of last update |
| Current Ver | Current version |
| Android Ver | Minimum Android version required |

---

## 🛠 Tools & Skills
- **Power BI Desktop:** report design, drill-through pages, tooltips, slicers
- **Power Query (M):** cleaning and transformation
- **DAX:** KPI and analytical measures
- **Concepts:** data cleaning, binning/grouping, KPI design, storytelling with data

---

## Data Cleaning & Preprocessing

| Step | What I did | Why |
|---|---|---|
| **Promote headers & change types** | Set Reviews to whole number, Rating to decimal, Last Updated to Date | Correct data types for aggregation |
| **Duplicate check** | Duplicated the `App` column (`App - Check`) and used *Keep Duplicates* to inspect repeated rows | Find repeated app entries before removing them |
| **Remove corrupted row** | Found a row where Category held a date (`February 11, 2018`) because its columns were shifted, and removed it | Prevents a fake category from appearing |
| **Standardise Size** | Split `Size` into `SizeNumber` + `SizeUnit`, and created `SizeMB` (`k` values ÷ 1024, `M` values kept as is) | Size was text such as `19M` and `201k` |
| **Handle "Varies with device"** | Filtered with `SizeUnit <> " "` | These rows have no usable size |
| **Installs & Price → numeric** | Removed `+`, `,` and `$` and converted to numbers | Required for sums and averages |
| **Split Genres** | Split `Education;Action & Adventure` into `Genres` + `Sub-Genres` | Needed for genre and sub-genre analysis |
| **Date hierarchy** | Used Last Updated's Year → Quarter → Month → Day hierarchy | Trend analysis |
| **Size Bins** | Created a Power BI group on `SizeMB` (39 bins, ≈2.56 MB each) | Size-distribution analysis |
| **Missing values** | Handled missing Rating and Type values | Prevents skewed averages |

📸 **Screenshot: Power Query applied steps (duplicate check)**

<img width="600" height="400" alt="Screenshot 2026-09-26 155440" src="https://github.com/user-attachments/assets/9b283a10-9aa9-4781-896c-c7de262db8b0" />


📸 **Screenshot: Size column before cleaning (`2.5M`, `201k`…)**

<img width="515" height="807" alt="Screenshot 2026-09-26 160322" src="https://github.com/user-attachments/assets/5c29cd12-a540-4c4e-bb94-fe79dd201e9c" />


📸 **Screenshot: Genres values (e.g. `Education;Brain Games`)**

<img width="445" height="602" alt="Screenshot 2026-09-26 161242" src="https://github.com/user-attachments/assets/e66867f2-5ad5-466d-9fbd-051940106bd2" />


📸 **Screenshot: Corrupted category row (`February 11, 2018`)**

<img width="340" height="167" alt="Screenshot 2026-09-26 162346" src="https://github.com/user-attachments/assets/8adf0a33-22df-4915-bd92-62b373d71552" />


📸 **Screenshot: Size converted to MB (M values)**

<img width="600" height="700" alt="Screenshot 2026-09-26 233431" src="https://github.com/user-attachments/assets/0686b627-a84f-4afe-ad01-356d3c474b81" />


📸 **Screenshot: Size converted to MB (k values)**

<img width="600" height="700" alt="Screenshot 2026-09-26 233601" src="https://github.com/user-attachments/assets/f0e2bc1b-ea20-43de-ab05-6cf5913d1c8d" />


📸 **Screenshot: Size Bins group (39 bins)**

<img width="700" height="400" alt="Screenshot 2026-09-26 235825" src="https://github.com/user-attachments/assets/d050ecaf-f1cc-4d9d-b1ff-a62e2dcc1e35" />

---

## Data Model & DAX Measures
A dedicated **Measure Table** holds every KPI:
`Total Apps · Free Apps · Paid Apps · Total Installs · Total Reviews · Average Rating · Average Reviews · Avg Reviews Free · Avg Reviews Paid · Apps Rated 4+ · Apps Updated (year) · Unique Categories`

```DAX
-- Reference versions: match them to the exact measures in your .pbix
Average Rating     = AVERAGE ( googleplaystore[Rating] )
Total Installs     = SUM ( googleplaystore[Installs] )
Total Reviews      = SUM ( googleplaystore[Reviews] )
Total Apps         = DISTINCTCOUNT ( googleplaystore[App] )
Apps Rated 4+      = CALCULATE ( DISTINCTCOUNT ( googleplaystore[App] ), googleplaystore[Rating] >= 4 )
Avg Reviews Free   = CALCULATE ( AVERAGE ( googleplaystore[Reviews] ), googleplaystore[Type] = "Free" )
Avg Reviews Paid   = CALCULATE ( AVERAGE ( googleplaystore[Reviews] ), googleplaystore[Type] = "Paid" )
Unique Categories  = DISTINCTCOUNT ( googleplaystore[Category] )
```

---

## 📊 Dashboard Overview

**Page 1: Application Insights (executive summary)**

| KPI | Value |
|---|---|
| Total Installs | **75.12 bn** |
| Total Reviews | **2.09 bn** |
| Average Rating | **4.17** |
| Total Apps | **8,174** |
| Free / Paid split | **92.66% / 7.34%** |

Slicers: Category and Genre. Visuals: Top 5 installs by category, Top 10 installed genres, Free vs Paid donut, Update trend.

<img width="600" height="400" alt="Screenshot 2026-09-27 224608" src="https://github.com/user-attachments/assets/00a30a4e-cc3a-481f-b115-670949148e2e" />



**Page 2: Deep dive.** Breakdown tree (Category → Genres), Reviews/Installs/Size table, Least-reviewed categories, Average installs by content rating.

<img width="600" height="400" alt="Screenshot 2026-09-27 225115" src="https://github.com/user-attachments/assets/53f2dce3-00c0-4e45-97da-55d0a2e299ab" />



---
## Key Insights

### 1. Games Lead in Total Installs

The analysis identifies Game as the leading category by total installs, followed by Communication and Tools.

Insight: These categories represent substantial user demand in the dataset.

Business implication: An app-development company should investigate successful applications in these categories before deciding where to invest development and marketing resources.

### 2. Free Applications Dominate the Dataset

Approximately 92.66% of applications in the original dashboard report are free, compared with 7.34% paid applications.

Insight: Free applications account for the overwhelming majority of listed apps.

Business implication: A free-to-use offering supported by advertising or optional in-app purchases may be worth testing, depending on the target audience and business model.

### 3. High Ratings Do Not Automatically Mean High Popularity

Some applications receive very high ratings despite having relatively few reviews or installs.

Insight: Rating measures user feedback, while installs and reviews provide different perspectives on popularity and engagement.

Business implication: App performance should be evaluated using several metrics rather than ratings alone.

### 4. Ratings Vary Across Categories and Genres

The analysis shows differences in average and median ratings across application categories and genres.

Insight: User satisfaction is not uniform across all types of applications.

Business implication: Developers should investigate the features, usability, and audience expectations associated with highly rated applications in their target category.

### 5. Application Price Does Not Guarantee Higher Ratings

The price-versus-rating analysis does not establish a consistent relationship in which more expensive applications always receive better ratings.

Insight: Pricing alone is not a reliable indicator of user satisfaction.

Business implication: Paid applications should justify their price through useful features, reliability, support, and a positive user experience.

### 6. App Size Requires Context

The size analysis compares application sizes with installs, but the relationship is not necessarily consistent across all applications.

Insight: A larger application does not automatically attract more installs or receive better ratings.

Business implication: Developers should balance functionality and media quality against download size and device-storage requirements.

### 7. Application Updates Increased Over the Recorded Period

The update-trend visualization shows a substantial increase in recorded update activity toward the end of the dataset's time range.

Insight: The distribution of Last Updated dates indicates that update activity was concentrated in later recorded years.

Business implication: Developers should maintain applications through bug fixes, compatibility updates, security improvements, and useful feature releases.

Limitation: The dataset records the latest update date for each application, not a complete history of all releases. It therefore cannot establish the actual frequency of updates for individual applications.

---

## 🚀 Key Recommendations

1. **Target proven demand:** Game, Communication and Tools drive the most installs (14bn, 11bn, 8bn).
2. **Look for niche opportunities:** Events, Art & Design, Education and Parenting score highest on rating with low review volume, a sign of low competition.
3. **Optimise app size by category:** keep utilities under ~12 MB, and use asset compression for games (33–49 MB average).
4. **Price for perceived value:** price level alone doesn't drive ratings. Poor value at $6–9 produces rating dips, so keep premium apps well supported.
5. **Plan for a free-first model:** 92.7% of apps are free. Use ads or in-app purchases, and make sure paid apps ask for reviews.
6. **Design for "Everyone":** 81% of apps use this rating, giving the widest reach.
7. **Update regularly:** two-thirds of apps were refreshed in 2018, so staleness is visible.
8. **Benchmark with volume:** compare against apps with 1,000+ reviews, not raw top-rated lists.

---

## Challenges Faced

### Data Cleaning

The dataset contained text-formatted numeric fields, missing values, inconsistent size units, and a corrupted category record.

Solution: Used Power Query to standardize fields and prepare them for analysis.

### Application Size Conversion

The Size column included values in both KB and MB, along with Varies with device.

Solution: Separated the numeric values from their units and standardized usable sizes into MB.

### Duplicate Handling

Repeated app names made it necessary to distinguish between repeated names and exact duplicate records.

Solution: Inspected repeated entries and considered the effect of duplicate records on distinct counts and aggregate measures.

### Aggregation Accuracy

Summing repeated values can inflate totals when records are duplicated or when a measure is intended to represent one value per app.

Solution: Used appropriate aggregation methods, including SUM, AVERAGE, and DISTINCTCOUNT, depending on the metric being analyzed.

### Visual Design

Presenting multiple business questions without overcrowding the dashboard required careful selection of charts and KPIs.

Solution: Separated executive-level KPIs from detailed analytical views and used interactive slicers to support exploration.

### Analytical Limitations

The dataset does not contain the text of individual user reviews or a full history of application updates.

Solution: Limited conclusions to the available fields and avoided claiming that sentiment or historical update frequency could be measured directly.

## Conclusion

The App Insights Unlocked project demonstrates how Power BI can transform raw Google Play Store data into an interactive analytical report.

By combining data preprocessing, KPI development, visual analysis, and business recommendations, the project explores application popularity, user ratings, pricing, size, and update patterns.

The findings can help an app-development company identify promising categories, evaluate competing applications, improve user experience, and make more informed product decisions.

The project also highlights an important analytical principle: ratings, installs, and reviews measure different aspects of application performance and should be interpreted together.

## Project Resources
GitHub Repository: App Insights — Power BI
Dataset: Google Play Store Apps on Kaggle
Primary Tool: Microsoft Power BI Desktop
