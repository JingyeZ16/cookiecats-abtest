# 🎮 **Cookie Cats A/B Testing — Impact of Gate Design on Player Retention**

<p align="center">
  <img src="https://miro.medium.com/v2/resize:fit:1400/1*VhPfZIYkcnV-vWZYaAvgjg.jpeg" width="600">
</p>

---

## 🧭 **1. Project Overview**

### 🎯 **1.1 Background & Motivation**
In free-to-play mobile games, **progression gates** (levels that temporarily block advancement) are key for pacing gameplay and monetization.  
However, poor tuning can frustrate players and lower retention.

This project analyzes an A/B test from the puzzle game **Cookie Cats**, where the first gate was moved from **level 30 → level 40**.  
The hypothesis: delaying the first gate might increase early engagement before players face friction.

- **A / Control group:** gate at level 30  
- **B / Variant group:** gate at level 40  
- **Players:** ≈ 90 K (randomly assigned)

**Metrics tracked:**
- 🎮 `sum_gamerounds` – total rounds played  
- 📅 `retention_1` – returned on Day 1  
- 📆 `retention_7` – returned on Day 7  

---

### 🧪 **1.2 Experiment Design**
Each group had roughly **45 K players** tracked for **14 days post-install**.

**Analysis pipeline:**
- Summary statistics & visualization  
- Hypothesis testing (t-test / Mann–Whitney U)  
- Two-proportion z-tests for retention  
- Bootstrapping for confidence intervals  

> **Goal:** Provide data-driven evidence on whether moving the gate improves player engagement or retention.

---

## 📊 **2. Data Overview & Cleaning**

### 📋 **2.1 Dataset Summary**

| Column | Description |
|--------|-------------|
| `userid` | Unique player ID |
| `version` | Group label (`gate_30` / `gate_40`) |
| `sum_gamerounds` | Total rounds played in 14 days |
| `retention_1` | Returned on Day 1 |
| `retention_7` | Returned on Day 7 |

The dataset was balanced between groups.  
The `sum_gamerounds` variable was **heavily right-skewed** (max ≈ 49 K).

> ⚙️ **Action:** Removed one extreme outlier (`sum_gamerounds = 49,854`) → new max = 2,961.  
> This stabilized variance and improved downstream statistical reliability.

---

## 🔍 **3. Exploratory Analysis**

### 🎮 **3.1 Game Rounds**
Both groups showed similar engagement:
- Median ≈ 16–17 rounds  
- Strong right-skew  
- No major difference in playtime  

> 💡 **Insight:** Delaying the gate did **not change total gameplay.**

---

### 📈 **3.2 Retention**

| Version | Day 1 | Day 7 |
|----------|-------|-------|
| gate_30 | 44.8 % | 19.0 % |
| gate_40 | 44.2 % | 18.2 % |

> **Observation:** Day-1 retention is almost identical, but **Day-7 retention is lower** for gate_40.

---

## 🧩 **4. Randomization Check**

### ✅ **4.1 Balance Validation**

| Metric | Test | p-value | Result |
|---------|------|----------|--------|
| `sum_gamerounds` | Welch t-test | 0.949 | ✅ Comparable |
| `retention_1` | Chi-square | 0.075 | ✅ Comparable |
| `retention_7` | Chi-square | 0.064 | ✅ Comparable |

> All p > 0.05 → Proper randomization achieved.

---

### 📊 **4.2 Distribution Comparison**
Both groups had:
- Nearly identical medians (~16–17)  
- Right-skewed tails  
- Overlapping cumulative curves  

> 💡 **Insight:** Randomization was balanced, confirming that differences later are due to the treatment itself.

---

## 📏 **5. Hypothesis Testing**

### 🎮 **5.1 Game Rounds (Numeric Metric)**
`sum_gamerounds` was **non-normal**, so a **Mann–Whitney U test** was applied.

| Test | p-value | Conclusion |
|------|----------|------------|
| Mann–Whitney U | 0.0509 | ❌ Not significant (α = 0.05) |

> 📊 **Interpretation:** Moving the gate did **not significantly affect** total gameplay.

---

### 📊 **5.2 Retention (Binary Metrics)**

| Metric | gate_30 | gate_40 | p-value | Conclusion |
|--------|----------|----------|----------|-------------|
| Day 1 | 44.8 % | 44.2 % | 0.0739 | ❌ Not significant |
| Day 7 | 19.0 % | 18.2 % | 0.0016 | ✅ Significant |

> 💡 **Interpretation:** Short-term retention stayed similar, but **long-term retention declined** in the test group.

---

## 🧮 **6. Bootstrapping Analysis**

### 🧠 **6.1 Why Bootstrapping?**
Traditional tests provide only p-values.  
Bootstrapping captures the **full uncertainty distribution** and builds confidence intervals for effect size.

---

### 🔁 **6.2 Methodology**
- 500 bootstrap resamples (with replacement)  
- Calculated mean `retention_1`, `retention_7` for A/B  
- Derived differences and 95 % confidence intervals  

---

### 📊 **6.3 Results**

| Metric | 95 % CI | Includes 0? | Conclusion |
|---------|----------|--------------|-------------|
| Retention 1-day | [ −0.002, 0.012 ] | ✅ Yes | Not significant |
| Retention 7-day | [ 0.008, 0.022 ] | ❌ No | Significant |

**Probability that gate_40 performs worse:**
- Day 1 ≈ 96.6 %  
- Day 7 ≈ 99.98 %

> 💡 **Insight:** Delaying the first gate almost certainly lowers 7-day retention.

---

## 💡 **7. Key Insights & Recommendations**

### 🧾 **7.1 Summary of Findings**
- 🎮 Gameplay unchanged → gate position didn’t affect engagement  
- ⏱ Day-1 retention unchanged → short-term unaffected  
- 📉 Day-7 retention ↓ 0.8 pp (~5 % relative) → statistically meaningful  

---

### 💼 **7.2 Business Impact**
The goal was to **delay friction** and improve monetization,  
but the change **reduced long-term retention** instead.

By testing before rollout, the team **prevented a negative product update** that could have decreased retention at scale.

> ✅ **Recommendation:** Keep the gate at **level 30**.  
> This A/B test shows how experimentation protects user experience as much as it drives optimization.

---

## 🧰 **Tech Stack**
- Python · pandas · NumPy · Seaborn · Matplotlib  
- SciPy · Statsmodels · SQLite · Jupyter Notebook  

---

## 👩🏻‍💻 **Author**
**Jingye Zhao (Lolly)**  
_Data Analyst ｜ Data Scientist ｜ Seattle, WA_
