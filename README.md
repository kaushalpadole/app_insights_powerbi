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
7. [Basic-Level Questions (Q1–Q10)](#-basic-level-questions)
8. [Medium-Level Questions (Q11–Q20)](#-medium-level-questions)
9. [Advanced-Level Questions (Q21–Q25)](#-advanced-level-questions)
10. [Key Recommendations](#-key-recommendations)

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




**Insight:** Entertainment and creative genres (Music, Strategy, Puzzle, Comics) are consistently rated highest, and mean and median agree. Communication, Books & Reference and Lifestyle sit lowest. Comics' very high score likely rests on a small number of apps, so treat it with caution.

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
