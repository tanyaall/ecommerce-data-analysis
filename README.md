# Marketplace Analytics

Full analysis of an e-commerce marketplace database - from SQL querying through data cleaning to statistical testing, finishing with an interactive dashboard.

## SQL analysis

Used SQLAlchemy to query a SQLite database and answer business questions: revenue and average order value by category, top products within each category, monthly and daily revenue trends, and customer retention patterns.

Electronics and Home & Kitchen turned out to be the leading categories by revenue (~66M and ~64M), driven mostly by order volume rather than average check. Overall revenue grew almost 50x over the two-year period, with seasonal spikes around November-December. The median gap between a customer's first and second purchase was about 25 days, and second-purchase retention within 90 days rose from roughly 25-30% at the start of the period to 60-70% by the end.

## Data cleaning

Found and documented 18 data quality issues across 6 tables: inconsistent date formats, mismatched city name spellings, impossible birth years, duplicate customer records under different IDs, malformed prices, and both structural and unexplained missing values.

## Statistical analysis

1. Compared mean vs. median order value - the distribution is right-skewed (skew ~1.87), so median (~22,400) is the more representative summary than mean (~31,300), which is pulled up by a small number of large orders
2. Modeled the number of items per order as a shifted Poisson distribution (every order has at least 1 item by definition, plus additional items follow Poisson(λ≈0.79))
3. Tested delivery time distribution - strongly non-normal (skew ~15), confirmed with a Q-Q plot; also found 31 orders with clearly anomalous delivery times that needed to be excluded
4. Ran a chi-square test on device type vs. conversion rate - desktop converts almost twice as well as mobile (5.94% vs 3.17%), a statistically significant difference (p ≈ 0) with non-overlapping 95% confidence intervals
5. Tested whether promo codes are associated with higher order value - found a statistically significant difference (p = 0.0072), but in the opposite direction expected: orders with a promo code were actually slightly cheaper on average, and the effect size was negligible (Cohen's d ≈ 0.07)
6. Estimated return probability (6.46% overall) and tested whether cash-on-delivery orders return more often - the difference wasn't statistically significant (p = 0.21)
7. Applied Bayes' theorem to a fraud-detection flag: the model catches 87% of actual chargebacks, but because chargebacks are rare (1.29% of orders) and the flag fires on 9% of orders, the actual probability that a flagged order is fraudulent is only about 12%

## Dashboard

Built an interactive dashboard in Plotly with category revenue trends over time and delivery time by city, confirming that Almaty and Astana deliver fastest (median 2.6-2.7 days) while Aktobe and Shymkent lag behind (4-4.4 days).

## Tools

Python, SQL (SQLAlchemy), pandas, SciPy, statsmodels, Plotly

## Files

- `analysis.ipynb` - full analysis: SQL queries, data cleaning, statistical tests, visualizations
- `dashboard.html` - interactive dashboard (download and open in browser to view)
