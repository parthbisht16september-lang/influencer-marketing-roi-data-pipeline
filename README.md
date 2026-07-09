# Creator Economy Ad Spend and Campaign ROI Optimization Pipeline

## Interactive Project Assets
* Strategic Performance Dashboard: [View Google Looker Studio Dashboard](https://datastudio.google.com/reporting/62cf3e11-abbe-4e32-ab0d-6d2d4b32c022)

* Executive Stakeholder Presentation: [View Gamma AI Slide Deck](https://gamma.app/docs/Influencer-Marketing-Campaign-Dashboard-1shn54ly8d4y0pg)

## Project Overview and Business Context
A multinational consumer brand allocates marketing budgets to digital creators across Instagram, YouTube, TikTok, and Twitter. This project establishes a data-driven framework to move past vanity metrics and systematically isolate high-converting creator niches, optimize multi-platform ad spend allocation, and maximize overall Return on Ad Spend (ROAS) across 150,000 corporate records.

## Pipeline Architecture and Technical Stack

### 1. Data Cleaning and Imputation (Python/Pandas)
* Processed 150,000 campaign logs and simulated a 5% missing-value mask across primary sales streams to test pipeline durability.
* Imputed missing metrics systematically using partitioned, category-specific medians via programmatic aggregation grouping.
* Discretized continuous audience reach indices into four statistically uniform tiers (Micro, Mid-Tier, Macro, Mega) utilizing quantile discretization.

### 2. Relational Database Migration (MySQL)
* Structured an automated relational injection channel using SQLAlchemy and PyMySQL drivers.
* Streamed the transformed session-state DataFrames natively into a local MySQL storage engine managed via phpMyAdmin.

### 3. Business Intelligence and Analytics (SQL)
* Isolated top-performing creator niches and budget anomalies by writing complex subqueries and conditional logic matrices.
* Conducted density analysis using analytical window functions to rank campaign formats within distinct creator segments.

### 4. Enterprise Visual Reporting (Looker Studio & Gamma AI)
* Built a fully cross-filtered reporting layout featuring dynamic vertical sidebar controls to allow immediate drill-downs by platform and tier.
* Generated an executive presentation deck detailing budget reallocation strategies from underperforming channels into high-efficiency segments.

## Production SQL Script Highlight
```sql
WITH ranked_campaign_types AS (
    SELECT 
        influencer_category, 
        campaign_type, 
        SUM(product_sales) AS total_sales,
        DENSE_RANK() OVER(PARTITION BY influencer_category ORDER BY SUM(product_sales) DESC) AS format_rank
    FROM campaign_data 
    GROUP BY influencer_category, campaign_type
)
SELECT 
    influencer_category, 
    campaign_type, 
    total_sales 
FROM ranked_campaign_types 
WHERE format_rank = 1;
```
