# Hotel Harmony: Data Insights for Optimized Operations

## 📌 Project Overview
Faced with complex reservation patterns, operational bottlenecks, and volatile cancellation rates across property types, **Elite Hotels International** required an analytics-driven strategy to safeguard margins and optimize customer retention. 

This project establishes a robust Data Preprocessing, Exploratory Data Analysis (EDA), and Predictive Modeling pipeline. By applying statistical regression to historical booking records, the project uncovers actionable insights into guest behavior, seasonal risk horizons, and financial indicators to enhance operational efficiency.

---

## 📋 Business Problem & Objectives
*   **Core Problem:** Elite Hotels International needs to optimize booking patterns, mitigate an overall baseline cancellation rate of **27.49%**, and maximize structural property revenue.
*   **Key Objectives:**
    *   Expose macro trends across property structures (City vs. Resort Hotels).
    *   Quantify the historical financial yield and risk horizons across market segments.
    *   Build predictive workflows to isolate primary drivers of reservation attrition.
    *   Deliver statistical strategies regarding overbooking thresholds, deposit requirements, and seasonal pricing.

---

## 🛠️ Tools, Technologies & Libraries
*   **Data Processing & ETL:** Python (`Pandas`, `NumPy`) for missing value imputation, chronological mapping, and duplicate record mitigation.
*   **Exploratory Data Analysis:** `Matplotlib` and `Seaborn` for historical trend evaluation, distribution plots, and correlation matrix maps.
*   **Statistical Modeling:** `Scikit-Learn` for multi-variable Logistic Regression classifiers and Multiple Linear Pricing Regressions.
*   **Version Control:** Git & GitHub for codebase architecture.

---

## 📦 System & Hardware Baseline
The pipeline was engineered and verified on an actual local environment with the following specifications:
*   **Processor:** Intel Core i5-10400F CPU
*   **Memory:** 16GB RAM Baseline
*   **File Path Target:** `C:\Users\mukhe\OneDrive\Documents\Jobaaj_DA Projects\hotel_bookings.csv` [2]
*   **Dependencies:**
    ```bash
    pip install pandas numpy matplotlib sklearn
    ```

---

## 📊 Data Pipeline Architecture & Cleaning
The raw dataset spans **119,390 rows** and 32 structural features. The pristine ETL cleaning pipeline processed the records through these structural changes:
1.  **Duplicate Records Removed:** Identified and safely dropped **31,994** true duplicate entries, updating the analytical row count from 119,390 to a pristine **87,396** rows.
2.  **Missing Value Imputation:**
    *   `children`: Filled missing records with `0` and converted to standard `int` representation.
    *   `agent` / `company`: Null rows indicate direct personal bookings; imputed with a zero indicator (`0`).
    *   `country`: Null values standardized to placeholder category `"Unknown"`.
3.  **Data Type Optimization:** Coerced `reservation_status_date` into a clean pandas `datetime` object for moving average computations.

---

## ⚠️ Challenges Faced & Analytical Mitigation
1. **Pervasive Structural Data Redundancy:** The initial dataset contained high data redundancy with **31,994 duplicate rows**. Left unaddressed, this would have artificially inflated statistical metrics and skewed model distribution curves. *Mitigation:* Engineered a strict data cleaning function utilizing `.drop_duplicates()` to isolate a pristine dataset of **87,396 unique rows**.
2. **Incomplete Identifiers and Sparse Demographics:** Vital classification columns such as `agent`, `company`, `children`, and `country` contained localized null values. *Mitigation:* Applied domain-specific missing value imputation: missing child counts were assigned to `0`, unlisted agents/companies were flagged as independent direct bookings (`0`), and missing country codes were bucketed into a standalone `"Unknown"` category to safeguard categorical distributions.
3. **Multi-Collinearity and High-Cardinality Fields:** Encoding massive categorical strings like `market_segment` and `deposit_type` posed a risk of multi-collinearity (the dummy variable trap), which destabilizes regression coefficient estimates. *Mitigation:* Utilized Pandas `get_dummies()` with `drop_first=True` to strip the baseline indicator and optimize matrix mathematically.
4. **Disparate Numerical Dimensions:** Predictive variables held wildly vast scales (e.g., `lead_time` spanned values up to hundreds of days, whereas `booking_changes` stayed in single digits). Left unscaled, the machine learning algorithms would prioritize large-scale features. *Mitigation:* Implemented Scikit-Learn’s `StandardScaler` to calculate standard normal distributions (z-scores) across all variables, exposing true feature impact weights.

---

## 📈 Operational Insights & Empirical Metrics

### 1. Basic Baseline Discoveries
*   **Operational Horizon:** Guests book their accommodations an average of **79.89 days** in advance.
*   **Property Footprint:** City Hotels dominate volume with **61.13%** (53,428 bookings) compared to Resort Hotels at **38.87%** (33,968 bookings).
*   **Peak Volume Window:** **August** stands as the highest overall arrival month, pulling a maximum volume of **11,257** reservations.
*   **Geographic Demographic:** Portugal (**PRT**) represents the primary market source, generating a total of **27,453** bookings.
*   **Length of Stay (LOS):** Across all properties, the combined average length of stay is **3.63 nights** (2.63 week nights / 1.01 weekend nights).
*   **Distribution Channels:** Third-party Travel Agents represent massive intermediation, mediating **86.05%** (75,203 bookings) of overall bookings.

### 2. Medium-Level Financial Performance
*   **Pricing Benchmarks:** City Hotels command a higher average ADR (**$110.99**) than Resort Hotels (**$99.03**).
*   **Property Attrition Variance:** City Hotels suffer a higher baseline cancellation velocity (**30.04%**) than Resort properties (**23.48%**).
*   **Segment Financial Yield:** 
    *   *Online Travel Agents (TA):* Highest financial value with an average ADR of **$118.17**.
    *   *Corporate Segments:* Average ADR of **$68.15**.
*   **Macro Revenue Performance Matrix:** Realized room revenue (calculated exclusively from completed guest stays) confirms **August** as the maximum profit window with a staggering **$4,608,288.90** in total revenue.
*   **Request Pricing Impact:** A positive trend exists between customized special requests and financial commitment; reservations with 0 requests yield an ADR of **$99.67**, scaling steadily to **$131.09** for reservations with 4 special requests.
*   **Behavioral Footprint by Loyalty:** New guests display a prolonged length of stay (**3.70 nights**) compared to returning, repeated loyalty guests (**1.93 nights**).

---

## 🔬 Advanced Machine Learning Modeling

### 1. Advanced Logistic Regression (Cancellation Prediction)
*   **Model Performance:** Achieved an overall baseline classification accuracy of **76.68%** in predicting whether a reservation would cancel or check out.
*   **Ranked Attrition Drivers (Top Standardized Coefficients):**
    1.  `total_of_special_requests` (**-0.563**): 🟢 Significantly *decreases* the likelihood of cancellation, acting as a functional proxy for reservation commitment.
    2.  `market_segment_Online TA` (**+0.471**): 🔴 Heavily *increases* cancellation risk, highlighting high channel volatility.
    3.  `deposit_type_Non Refund` (**+0.442**): 🔴 Shows high risk, associated with long lead-time group blocks.
    4.  `lead_time` (**+0.428**): 🔴 Confirms that a wider booking window increases reservation vulnerability.
    5.  `booking_changes` (**-0.282**): 🟢 *Decreases* cancellation risk; active booking modification indicates intention to stay.

### 2. Multiple Linear Regression (ADR Pricing Impact)
*   **Model Baseline Intercept:** **$43.82** [2]
*   **Marginal Value per Guest Type:**
    *   Each additional **adult** structures a net ADR increase of **+$31.40** per night.
    *   Each additional **child** drives a premium ADR surge of **+$38.49** per night, confirming massive revenue potential in family suite allocations.
    *   Each additional **baby** adds **+$6.54** per night.

---

## 🚀 Strategic Recommendations for Improvement
1.  **Dynamic Deposit Scaling by Booking Window:** Because `lead_time` carries a heavy risk coefficient (+0.428), implement strict, non-refundable deposit thresholds for general transient bookings exceeding a 60-day horizon to reduce the 27.49% cancellation rate.
2.  **Optimize Family-Centric Pricing Packages:** Since the presence of `children` commands a higher marginal premium (+$38.49) than an additional `adult` (+$31.40), automated pricing engines should reallocate standard inventories into premium family packages during peak seasons.
3.  **Adjust Seasonal Overbooking Limits:** Leverage the empirical data showing August as both the highest revenue driver ($4.6M) and maximum cancellation risk window (32.18%). Safely expand overbooking margins up to 15-20% in August to guarantee 100% true occupancy without risking customer walk-aways.
4.  **Channel Marketing Optimization:** Shift marketing budgets away from volatile Online TA networks into Direct booking channels or protected corporate agreements to bypass high attrition rates.
