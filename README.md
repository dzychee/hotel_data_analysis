# hotel_data_analysis

# Cleaning & Analysing Hotel Booking Data

An end-to-end data cleaning, validation, and exploratory data analysis (EDA) project on hotel booking records from two hotels in Portugal (**City Hotel** and **Resort Hotel**, July 2015 – August 2017).

This project transforms a deliberately corrupted dataset into a clean, fully traceable analytical table without imputing missing values, answers 10 descriptive business questions, and uncovers 13 subgroup-verified insights using 95% Wilson confidence intervals.

---

## Project Overview

* **Course:** Data Analysis with Python — National Economics University (NEU)
* **Data Source:** Teaching extract of the *Hotel Booking Demand Datasets* (Antonio, de Almeida, & Nunes, 2019, CC BY 4.0)
* **Raw Dataset:** `10,325` rows × `24` columns (`row_id` + 23 business columns), seeded with format errors, missing tokens, label variants, contradictions, and duplicate records.
* **Cleaned Dataset:** `10,000` distinct bookings × `30` columns (including 7 derived features), `0` invalid cells, and `51/51` automated validation checks passed.

---

## Tech Stack

* **Language:** Python 3
* **Libraries:** `pandas`, `matplotlib`, `re`, `pathlib`
* **Environment:** Jupyter Notebook / Google Colab
* **Reporting:** LaTeX (PDF Lab Report)

---

## Data Cleaning & Traceability Pipeline

All columns were read as raw strings to prevent silent type coercion. Every transformation was logged to ensure 100% reproducibility:

1. **Key Normalization & Deduplication:** Standardized `booking_id` (`strip().upper()`) before deduplicating, merging `325` duplicate rows into `10,000` unique bookings while recording every raw `row_id` in `row_lineage.csv` (`10,000 kept`, `325 merged`).
2. **Misplaced Values & Missing Tokens:** Moved `26` dates mistakenly entered in `reservation_status` to `reservation_status_date`; standardized all missing tokens (`NULL`, `N/A`, `?`, `unknown`, `-`, etc.) to `NA` without imputation.
3. **Label & Conflict Resolution:** Mapped categorical variants to standard labels and resolved `41` contradictions between `is_canceled` and `reservation_status` by prioritizing `reservation_status`.
4. **Explicit Date & Numeric Parsing:** Parsed `arrival_date` across 4 explicit formats (`1,954` non-ISO dates standardized), stripped currency symbols (`€`, `EUR`) and `"nights"` suffixes, converted decimal commas (`95,50` → `95.5`), and set impossible/out-of-range values to `NA`.
5. **ISO-3 Country Standardization:** Mapped full country names and `"UK"` to standard ISO-3 codes (`GBR`, `PRT`, `FRA`, `ESP`, `DEU`).

---

## Key Insights & Findings

### 1. Cancellations
* **Hotel Mix Effect:** Overall, City Hotel cancels significantly more than Resort Hotel (**41.79%** vs. **28.50%**). However, within *No Deposit* bookings and the same market segment, cancellation rates between the two hotels are very close (overlapping 95% Wilson intervals).
* **Lead Time Effect:** Cancellation rates rise sharply with lead time across every subgroup—from **10.06%** (0–7 days ahead) to **58.06%** (181+ days ahead).
* **Non-Refund Anomaly:** **99.05%** (`1,247/1,259`) of *Non Refund* bookings were cancelled. They heavily concentrate in City Hotel, Portuguese guests, and the *Groups* segment, and **88.13%** were cancelled in batches (sharing the same hotel, arrival date, and cancellation date).
* **Channel & Guest Behavior:** Within *No Deposit*, **Online TA** has the highest cancellation rate (**36.88%** vs. **14.96%** for Direct). Bookings with **0 special requests** cancel more than twice as often as those with 2+ requests (**48.05%** vs. **19.98%**).
* **Repeated Guests:** Repeated guests cancel less when booking close to arrival (0–30 days: **5.99%** vs. **18.10%**), but cancel *more* when booking over 30 days ahead (**50.77%** vs. **34.45%**).

### 2. Seasonality, Pricing & Stay Patterns (2016)
* **Peak Demand Depends on Metric:** City Hotel peaked in **October** by booking count (`360`), but in **August** by realized room nights (`579`). Resort Hotel peaked in **May** by bookings (`162`), but in **June** by room nights (`545`).
* **Price Volatility (ADR):** Resort Hotel's median nightly rate fluctuates **4.8×** across the year (from **41.0 EUR** in January to **196.4 EUR** in August), whereas City Hotel remains relatively stable (**81.0 – 123.3 EUR**).
* **Length of Stay:** **70.29%** of City Hotel stays are short (1–3 nights). In contrast, **17.28%** of Resort Hotel stays last exactly **7 nights**, peaking at **33.71%** in the *Offline TA/TO* segment.

### 3. Operations & Data Integrity
* **Room Changes:** Resort Hotel assigns a different room type more often than City Hotel (**24.80%** vs. **14.42%**), with almost the entire gap driven by **Room Type A** (**35.96%** vs. **15.85%**), especially during off-peak months.

---

## Repository Structure

```text
├── hotel_analysis.ipynb      # Complete Python notebook (Audit → Clean → Validate → EDA)
├── Bao_cao_Khach_san.pdf     # Full LaTeX Lab Report (23 pages)
└── README.md
```

---

## How to Run
* Open `hotel_analysis.ipynb` in Jupyter Notebook or Google Colab and run all cells.
