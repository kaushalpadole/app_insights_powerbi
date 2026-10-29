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
