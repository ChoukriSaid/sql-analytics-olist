# SQL Analytics Case Study — Olist E-Commerce

Business analysis of the [Olist Brazilian e-commerce dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (~100k real orders) using **advanced SQL** on **DuckDB**.

This project answers 10 real business questions — each with the query, the result, and an actionable insight. The goal is not just to write SQL, but to turn data into decisions.

---

## Stack

`SQL` · `DuckDB` · `Python` · `pandas` · `Jupyter`

## Dataset

9 relational tables: orders, order_items, payments, reviews, products, sellers, customers, geolocation, category translation.
*Data not included in the repo (Kaggle, ~120MB) — see link above.*

---

## SQL techniques demonstrated

- Multi-table `JOIN`s across the relational schema
- `GROUP BY` + `HAVING` aggregation and filtering
- `CASE WHEN` conditional logic and bucketing
- **CTEs** (`WITH`) for readable, layered logic
- **Window functions**: `LAG()`, `SUM() OVER ()`, `RANK()`

---

## Business questions & insights

### Q1 — Average delivery time by state
Which Brazilian states have the worst delivery times?
**Insight:** Northern states (RR, AP, AM) wait ~29 days on average vs 8.7 days for São Paulo — 3x longer. These states have very low order volume, pointing to thin logistics coverage. Decentralized stock would close the gap.

### Q2 — Top 10 sellers by revenue
Which sellers generate the most revenue?
**Insight:** 9 of the top 10 sellers are based in São Paulo. Two distinct strategies emerge: high-volume / low-price (SP) vs low-volume / premium-price (the Bahia seller, avg basket €543). Encouraging premium sellers outside SP would diversify revenue geographically.

### Q3 — Late delivery rate
What share of orders is delivered after the estimated date?
**Insight:** Only 8.1% of orders are late. The average delivery lands ~12 days *before* the estimate — Olist deliberately over-estimates delivery windows to manage expectations. The real issue is concentrated in the late 8%, likely correlated with the northern states from Q1.

### Q4 — Categories with the best review scores
Which categories drive the highest customer satisfaction?
**Insight:** Books and travel accessories lead (4.45 and 4.32 / 5). These are light, easy-to-ship, low-damage products — logistics quality likely explains the scores as much as the product itself.

### Q5 — Monthly revenue growth *(window function)*
What is the month-over-month revenue trend?
**Insight:** Revenue grew ~21x in 18 months. November 2017 spikes (+52%, Black Friday). Growth stabilizes in early 2018 around €900k–1M / month — a sign of platform maturity. Uses `LAG()` to compute MoM growth without a self-join.

### Q6 — Average basket by payment method
Do customers spend more depending on payment type?
**Insight:** Credit card dominates — 75% of orders and the highest average basket (€163). Vouchers are 3.8% of orders with a basket 2x smaller (€65), suggesting use for small or complementary purchases.

### Q7 — Loyal vs one-time customers *(CTE)*
Do repeat customers represent a meaningful share of revenue?
**Insight:** 97% of customers order only once — a major retention problem. Yet repeat customers spend 3x more on average (€421 for 3+ orders vs €138 for one). A loyalty program targeting even 5% of one-time buyers could unlock millions without new acquisition.

### Q8 — Sellers with high negative-review rates *(CTE)*
Which sellers concentrate the worst customer experiences?
**Insight:** The worst seller has a 65% negative-review rate over 136 reviews — a suspension candidate. An automatic alert at a 20% negative threshold would let the platform intervene before global ratings degrade.

### Q9 — Product price vs review score
Do more expensive products get better reviews?
**Insight:** Price has almost no impact on satisfaction — scores stay between 4.00 and 4.06 across all price brackets. Satisfaction is driven by logistics and service, not by the product's price.

### Q10 — Top 5 categories by revenue & share *(two window functions)*
Which categories generate the most revenue and what share of the total?
**Insight:** The top 5 categories account for ~40% of total revenue. Health & Beauty leads (9.4%) with a high average basket despite fewer orders. Uses `SUM() OVER ()` for share-of-total and `RANK()` for ranking.

---

## How to run

```bash
pip install duckdb pandas jupyter
# Download the Olist dataset from Kaggle into the project folder
jupyter notebook analysis.ipynb
```

---

## Author

Said Choukri — [LinkedIn](https://linkedin.com/in/said-choukri) · [GitHub](https://github.com/ChoukriSaid)
