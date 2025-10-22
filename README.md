# Boston Airbnb Price Prediction

Predict the nightly **price** of Airbnb listings in Boston with a clean, reproducible supervised ML pipeline.

## Dataset
- **Source:** Inside Airbnb — Boston city snapshot (scraped from publicly available Airbnb pages; cleaned/aggregated for research and public discussion).
- **Kaggle mirror:** *Boston Airbnb Open Data* (`airbnb/boston`).
- **Files used here:** **`listings.csv`** only; `calendar.csv` and `reviews.csv` are not used in this iteration.

**Size (this snapshot):** `listings.csv` — **3,585 rows × 95 columns**  
**Snapshot window:** `last_scraped` **2016-09-07 → 2016-09-07**

## Features (current 9) & Types
- **Categorical:** `room_type`, `neighbourhood_cleansed`
- **Continuous:** `latitude`, `longitude`, `minimum_nights`, `number_of_reviews`, `reviews_per_month`, `availability_365`, `calculated_host_listings_count`

*Rationale:* Simple, interpretable, broadly available across rows; align with pricing intuition.

## EDA highlights
- **Price distribution:** Most listings cluster around low prices (~$50–$300); only a few high-price points on the right.  
- **By room type:** Clear separation — Entire highest, Private mid, Shared lowest; many outliers are Entire homes.  
- **Missingness:** Some columns are almost entirely missing (e.g., `jurisdiction_names`, `license`) → drop near-all-missing; impute others.

Figures in `figures/`: `fig1_price_hist.png`, `fig2_roomtype_box.png`, `fig3_missing_top.png`.

## Splitting & preprocessing
- **Split:** **60 / 20 / 20** (random) → Train **2151**, Val **717**, Test **717**  
- **Preprocessing:** Numeric → **median impute** + **standardize**; Categorical → **most-frequent impute** + **One-Hot** (ignore unknown)  
- **Feature count:** Before **9** columns → After **35** features (from One-Hot expansion)
- **Validation (planned):** Compare random split with a **group split** (by `host_id` if available, else `neighbourhood_cleansed`); consider time-based split if using `calendar`.

## Leakage considerations
- Sensitivity test: train **with vs without** `availability_365`.  
- Avoid future-derived fields from `calendar`.  
- Report both **MAE** (robust to rare large errors) and **RMSE** (more sensitive to large errors).

