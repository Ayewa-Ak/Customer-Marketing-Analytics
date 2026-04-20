# What Makes an Influencer Post Work?
**Modelling Engagement, Conversion & Audience Response in Social Commerce**

BBS Customer & Marketing Analytics — Team Assignment 1, 2026

---

## Overview 

This project analyses a dataset of **600 sponsored social media posts** from the #RealResults campaign, run by **Pulse Digital** — a digital marketing agency based in Amsterdam. The campaign placed influencer content across Instagram and TikTok on behalf of brands in three product categories: Beauty & Skincare, Fashion & Apparel, and Tech & Gadgets.

The goal was to identify what actually drives post performance — not just engagement in the abstract, but three distinct commercial outcomes: how much the audience interacts, whether they click through to the brand, and how much community discussion a post generates. Using regression modelling in Python, we built a separate model for each outcome and tested where caption choices, platform, post format, and influencer characteristics make a measurable difference.

---

## Research Questions

| # | Question | Model Type |
|---|---|---|
| Q1 | What drives **Engagement Rate**? | OLS Linear Regression |
| Q2 | What predicts whether a post gets a **Click-Through**? | Logistic Regression |
| Q3 | What determines **Comment Count**? | Negative Binomial GLM |
| Q4 | Do **interaction effects** change the story? | OLS with interaction terms |

---

## Key Findings

### Q1 — Engagement Rate (OLS, R² = 0.687)

The model explains nearly 69% of variation in engagement rate — a strong fit for real-world marketing data.

**What drove engagement up:**
- **TikTok vs Instagram (+2.39 pp)** — the single biggest predictor. TikTok posts significantly outperformed Instagram on engagement.
- **Short video / Reel format (+1.95 pp)** — video content consistently outperformed static photos.
- **Call-to-action in caption (+1.05 pp)** — posts with a CTA drove meaningfully higher interaction.
- **Lifestyle/values messaging (+0.73 pp)** — content that tapped into identity or values resonated more.
- **Promotional deals (+0.66 pp)** — discount or offer mentions boosted engagement.
- **Positive caption sentiment (+0.14 pp per unit)** — a more upbeat tone helped, though the effect was modest.

**What drove engagement down:**
- **Longer captions (–0.59 pp per SD)** — concise captions consistently outperformed verbose ones.
- **Macro influencers (–0.99 pp)** — micro influencers (≤50k followers) generated higher engagement rates relative to audience size.
- **Tech & Gadgets category (–1.39 pp)** — significantly underperformed vs the Beauty & Skincare baseline.
- **Fashion & Apparel (–0.65 pp)** — also underperformed Beauty, though by less.
- **Male influencers (–0.53 pp)** — female influencers drove higher engagement on average.

---

### Q2 — Click-Through (Logistic Regression, Pseudo R² = 0.075)

Click-through is harder to predict than engagement — many factors that drive likes and shares don't necessarily convert to clicks, which is itself a meaningful finding.

**What significantly increased click-through probability:**
- **Positive CTA framing (OR = 2.51)** — an inviting, upbeat call-to-action more than doubled the odds of a click-through. The single most reliable conversion driver in the model.
- **Macro influencer tier (OR = 1.89)** — unlike engagement, where micro influencers excelled, macro influencers drove more click-throughs — likely due to greater brand authority and audience trust.

**Notable but marginal signals:**
- Negative promo deal framing (OR = 5.19) had a large odds ratio but was not statistically significant at p < 0.05, likely due to its rarity in the dataset.
- Positive promo deals (OR = 1.42) approached significance (p = 0.069).
- Higher engagement rate was positively associated with click-through, but the relationship was not significant once other variables were controlled for.

---

### Q3 — Comment Count (Negative Binomial GLM, Pseudo R² = 0.375)

We used a **Negative Binomial** model rather than Poisson because comment counts are right-skewed — most posts receive a modest number, but some go viral. The Negative Binomial accounts for this overdispersion.

**What drove more comments:**
- **TikTok vs Instagram (+0.77 log-count)** — TikTok dramatically outperformed Instagram for community discussion, consistent with Q1.
- **Macro influencer tier (+0.70)** — larger audiences generated more raw comment volume, even if engagement *rate* was lower.
- **Short video format (+0.56)** — video content sparked more conversation than static posts.
- **Lifestyle/values messaging (+0.38)** — content with a values angle generated more discussion.
- **CTA in caption (+0.32)** — calls-to-action not only drove engagement and clicks, but also prompted more commenting.
- **Longer captions (+0.16 per SD)** — unlike in Q1, longer captions were associated with *more* comments, suggesting they may invite responses even if they dampen the engagement rate.

**Not significant:** product features, promotional deals, influencer gender or age, follower count.

---

### Q4 — Interaction Effects (Applied to Engagement Rate Model)

We tested two theoretically motivated interactions:

- **CTA × Platform** — does a call-to-action work differently on TikTok vs Instagram?
- **Video × Sentiment** — does caption tone matter more for video posts than static ones?

**Result:** Neither interaction was statistically significant (p > 0.89 for both), and the model's adjusted R² (0.665) was slightly lower than the main-effects-only model (0.679). This suggests the main effects of CTA, platform, and post format are **independent and additive** — there is no evidence that combining them produces a bonus effect beyond what each contributes on its own.

---

## Data

The dataset covers all 600 posts from the #RealResults campaign. Variables include:

- **Caption features** — sentiment score, word count, topic flags for product features, promo deals, lifestyle messaging, and CTAs, with positive/negative framing splits for each
- **Post & platform characteristics** — Instagram vs TikTok, static photo vs short video/Reel
- **Influencer attributes** — age, gender, follower count, micro vs macro tier
- **Product category** — Beauty & Skincare, Fashion & Apparel, Tech & Gadgets
- **Outcomes** — engagement rate (%), click-through (binary), comment count (integer)

Caption text is not included — all text features were pre-processed into numeric variables by Pulse Digital's NLP team using sentiment modelling and zero-shot classification.

---

## Data Preparation

- `Product_Category` was dummy-coded into two binary variables (`ProdCat_1` for Fashion & Apparel, `ProdCat_2` for Tech & Gadgets), with **Beauty & Skincare as the reference group**.
- `ZCaption_Length` (standardised z-score) was used in place of raw word count for easier coefficient interpretation across models.
- For Q2, positive/negative framing splits were used (e.g. `Topic_CTA_Positive`, `Topic_Promo_Deal_Negative`) to capture not just whether a topic appeared, but how it was framed.
- For Q4, two interaction terms were created: `CTA_x_Platform` and `Video_x_Sentiment`.

---

## Tech Stack

- **Python** — pandas, statsmodels, matplotlib, seaborn
- **Models** — OLS (statsmodels), Logit, Negative Binomial GLM
- **Environment** — Google Colab

---

## Repository Structure

```
├── Team_Assignment_1.ipynb        # Full analysis notebook
├── README.md
└── data/
    └── Team_Assignment_1_Data_2026.xlsx
```
