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

# 🟢 Basic-Level Questions

### Q1. What is the average rating of apps in the dataset?

**Answer:** The average rating is **4.17** out of 5.

**Insight:** Users are generally satisfied, so 4.17 is a realistic baseline for new apps. Any new release should aim to launch at or above 4.2 to stand out.

---

### Q2. How many unique categories of apps are there?

**Answer:**  **33** distinct categories (after removing the corrupted `February 11, 2018` row).

**Insight:** The market is spread across many categories, and categories with few apps but good ratings (see Q12) are potential low-competition niches.

---

### Q3. What is the distribution of app sizes?

**Answer:** Apps were grouped into **39 size bins of ≈2.56 MB** each. The distribution is **right-skewed**: most apps are small (roughly under 20–30 MB) and the number of apps falls steadily as size increases, leaving a thin tail of very large apps.

**Insight:** Category matters. In the deep-dive table, Tools average only **8.30 MB**, while Arcade (**39.34 MB**) and Action (**49.18 MB**) are several times heavier. Size budgets should be set per category.

---

### Q4. How many free vs paid apps are there?

**Answer:**
| Type | Apps | Share |
|---|---|---|
| Free | **7,588** | **92.66%** |
| Paid | **602** | **7.34%** |

**Insight:** The market is overwhelmingly free. Paid apps are a small niche, so a paid model needs clear differentiation and the business should look at in-app purchases and ads for revenue.

---

### Q5. What is the most common content rating?

**Answer:** **Everyone** is the most common rating, with **6,612 of 8,190 apps (~80.7%)**.

<img width="622" height="451" alt="Screenshot 2026-09-26 192027" src="https://github.com/user-attachments/assets/bcbca512-51e2-4b69-9715-d6c29ce858b4" />


**Insight:** Designing for "Everyone" gives access to the broadest audience and the fewest compliance restrictions.

---

### Q6. What are the top 5 most installed apps?

**Answer:** The table lists Facebook, WhatsApp Messenger, Instagram, Messenger – Text and Video Chat for Free, and Subway Surfers. Each is displayed as having 1B installs.

---

### Q7. How many apps have a rating of 4.0 and above?

**Answer:** 
<img width="252" height="157" alt="image" src="https://github.com/user-attachments/assets/f22f9870-9adc-4023-87e2-f2b2ac67c9ec" />

---

### Q8. What is the average number of reviews for free vs paid apps?

**Answer:** The table reports 234.4K average reviews for free apps and 6.68.8K average reviews for paid apps.

---

### Q9. What is the average app size for each category?

**Answer:**
| Genre | Avg Size (MB) |
|---|---|
| Tools | 8.30 |
| Communication | 11.95 |
| Productivity | 12.06 |
| Photography | 15.78 |
| Video Players & Editors | 15.93 |
| Entertainment | 16.92 |
| Social | 17.31 |
| Casual | 32.84 |
| Arcade | 39.34 |
| Action | 49.18 |

**Insight:** Utility apps (Tools, Communication, Productivity) are light at about 8–12 MB, while games run 33–49 MB. Games need extra optimisation such as asset compression and on-demand downloads.

---

### Q10. How many apps were last updated in 2018?

**Answer:** 6,272 apps updated in 2018. This is a count of app records with a 2018 last-updated date, not the number of update events during the yr.


**Insight:** Two-thirds of the market was updated in the latest year of the data, so regular maintenance is the norm. An app that goes stale will look neglected next to its competitors.

---

# 🟡 Medium-Level Questions

### Q11. What is the correlation between installs and rating?

**Answer:** There is a **weak positive relationship**. Average rating rises from **4.10** (lowest bucket) to **4.39** at 100M+ installs, then dips slightly at the 500M bin (4.38) and 1bn+ bin (4.22). See Advanced Q23 for the full bins.

**Insight:** Popular apps tend to be rated slightly higher, but installs alone don't guarantee quality.

---

### Q12. Which app categories have the highest average rating?

**Answer:**
<img width="600" height="400" alt="Screenshot 2026-09-26 225357" src="https://github.com/user-attachments/assets/5e965b9a-316d-4ca4-85bb-2f3d3bacdc65" />


**Insight:** Niche, purpose-driven categories (Events, Art & Design, Education) satisfy users most. Several of them also have few reviews (see Q16), which suggests a low-competition, high-satisfaction opportunity.

---

### Q13. How does price affect average rating?

**Answer:** There is **no clear linear relationship**. Paid apps average **4.26**, and individual price points vary a lot:

| Price ($) | Avg Rating |
|---|---|
| 5.49 | 4.50 |
| 7.49 | 4.45 |
| 5.99 | 4.29 |
| 9.99 | 4.19 |
| 8.99 | 3.73 |
| 8.49 | 3.70 |
| 6.49 | 3.40 |
| 79.99 | 4.60 (very few apps) |

**Insight:** A higher price does not guarantee happier users. Apps that disappoint at $6–9 get punished with low ratings, so value-for-money matters more than price level. The $5–7.5 range looks like the sweet spot.


---

### Q14. How are ratings distributed across content ratings?

**Answer:**
<img width="600" height="400" alt="Screenshot 2026-09-26 230139" src="https://github.com/user-attachments/assets/92e0f499-7b18-4bb8-9bf6-f7a0bc6a6373" />


**Insight:** Differences are small (4.10–4.30). Content rating has little effect on satisfaction. Treat the 18+ and Unrated figures cautiously because of tiny sample sizes.

---

### Q15. Which genres have the most apps with over 1 million installs?

**Answer:**
<img width="600" height="400" alt="Screenshot 2026-09-26 230611" src="https://github.com/user-attachments/assets/f8cedc6f-2c4a-49ba-8005-7d7c57d84ccb" />


**Insight:** Tools dominate the 1M+ club. Utility and entertainment genres are the most proven routes to scale.

---

### Q16. How frequently do apps get updated? (average time between updates)

**Answer:** The dataset holds only **one `Last Updated` date per app**, so the gap between consecutive updates cannot be computed. 
---

### Q17. What is the impact of app size on installs?

**Answer:** The scatter plot (SizeMB vs count of installs) shows that **most apps cluster at small sizes** (about 5–25 MB) and the number of apps falls steadily past ~30 MB, leaving a sparse tail beyond 60–80 MB. Large apps are rare.

<img width="600" height="400" alt="Screenshot 2026-09-27 000801" src="https://github.com/user-attachments/assets/1493b4d3-16bd-4ac2-adcf-863553050ced" />


**Insight:** Teams usually keep apps lean. To measure size's effect on installs directly, add **Average of Installs by Size Bin** (my first attempt used *Sum of SizeMB*, which gave a single dot).

---

### Q18. Which apps have the most reviews, and what are their ratings?

**Answer (Top 10):**
<img width="600" height="400" alt="Screenshot 2026-09-26 235345" src="https://github.com/user-attachments/assets/43ed8e8e-fdc2-4385-978b-c9b8cbdfb980" />


**Insight:** The most-reviewed apps are **social/messaging apps and games**. Social apps score lower (4.0–4.4) than games and cleaner/security utilities (4.5–4.7). Scale does not guarantee satisfaction.

---

### Q19. How does the content-rating distribution differ between free and paid apps?

**Answer:**
<img width="617" height="332" alt="image" src="https://github.com/user-attachments/assets/7aa8b5ca-b842-4b9c-8d16-31a3aa9011fd" />


**Insight:** Both types are dominated by "Everyone" (80% of free, 85% of paid). Paid apps have a slightly higher share of Everyone and a lower share of Teen and Mature content.

---

### Q20. What are the top 5 categories by installs?

**Answer:**
<img width="600" height="400" alt="Screenshot 2026-09-27 002755" src="https://github.com/user-attachments/assets/d538ff5c-3672-4813-83b9-cb875d7251ad" />


**Insight:** Games lead demand, then communication and utilities. The breakdown tree (Page 2) shows Game installs split mostly across **Arcade (4bn), Action (4bn), Casual (2bn) and Racing (1bn)**.

---

# 🔴 Advanced-Level Questions

### Q21. Top 10 highest-rated apps: how do reviews and installs compare?

**Answer:** 
<img width="430" height="306" alt="image" src="https://github.com/user-attachments/assets/9deb590d-45e2-4e2a-adf5-7c63e6fa84f8" />


---

### Q22. Trend of app updates over time: any patterns?

**Answer:** The annual counts rise sharply, from 1 in 2010 to approximately 6.3K in 2018; the most pronounced increases occur in 2017 and 2018.


**Insight:** The curve is **exponential**, with a huge jump in 2018. This reflects when the data was collected (apps naturally show a recent last-updated date) and an actively maintained app market. Because only the latest update is stored, true seasonality cannot be proven. Drill down to Quarter/Month for the pattern within 2018.

---

### Q23. How does average rating change with installs? (binned analysis)

**Answer:**
<img width="362" height="285" alt="Screenshot 2026-09-27 010236" src="https://github.com/user-attachments/assets/77a19b8c-ac80-4f7e-ad75-7528d96af954" />


**Insight:** Ratings **climb steadily with popularity up to 100M installs** (4.10 → 4.39). Great apps earn more installs and installs bring more feedback to improve the app. At the 1bn+ level the average falls back to 4.22, since mass-market apps are harder to please.

---

### Q24. Sentiment analysis on app reviews
**Answer:** **Not possible with this file.** `googleplaystore.csv` contains only review *counts*, not review *text*. The same Kaggle dataset includes `googleplaystore_user_reviews.csv`, which could be used for a future sentiment analysis in Python or Power BI.

---

### Q25. Relationship between genre and rating: are some genres consistently higher or lower?

**Answer (Genres, average / median):**

<img width="546" height="391" alt="image" src="https://github.com/user-attachments/assets/baf2a458-247c-434a-b114-506eb9b98aaa" />
<img width="515" height="300" alt="Screenshot 2026-09-27 012029" src="https://github.com/user-attachments/assets/cbdd92f1-811e-435f-979a-fab9a4f87737" />


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
