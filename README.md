# World Layoffs Analysis: SQL Data Cleaning & EDA
## Project Overview
This project involves a full data migration and analysis pipeline using a raw dataset of global company layoffs from 2020 to 2023. The workflow is divided into two major SQL scripts: 
1. **Data Cleaning**: Transforming messy "raw" data into a structured, usable format.
2. **Exploratory Data Analysis (EDA)**: Extracting actionable insights and identifying economic trends.

## 💾 Data Source
The dataset used is the [Kaggle World Layoffs Dataset](https://www.kaggle.com/datasets/swaptr/layoffs-2022), containing real-world data on layoffs, industries, and funding stages.

## 🛠️ Tech Stack
* **Database:** MySQL
* **Key Techniques:** CTEs (Common Table Expressions), Window Functions (`DENSE_RANK`, `ROW_NUMBER`), Self-Joins, Data Standardization, and Date Formatting.

---

## Phase 1: Data Cleaning 
In this phase, I focused on **Data Quality** to ensure the analysis is based on accurate information.

### Key Steps:
1. **Staging Environment:** Created a `layoffs_staging` table to protect the raw data from accidental changes.
2. **Deduplication:** Used `ROW_NUMBER()` partitioned by all columns to identify and delete duplicate entries.
3. **Standardization:** 
    * Unified industry names (e.g., consolidated variations of `Crypto` and `Crypto Currency`).
    * Fixed geographical errors (e.g., removing trailing periods from `United States.`).
4. **Handling NULLs & Blanks:** 
    * Populated missing `industry` values using a **Self-Join** (matching companies that had the industry listed in other rows).
    * Removed records where both `total_laid_off` and `percentage_laid_off` were NULL.
5. **Format Conversion:** Converted the `date` column from `TEXT` to a proper `DATE` type using `STR_TO_DATE`.

---

## Phase 2: Exploratory Data Analysis (EDA)
With clean data, I explored the dataset to find significant patterns:

### Insights & Analysis:
* **The Surge:** Identified a massive spike in layoffs starting in late 2022, peaking in early 2023.
* **Top Impacted Industries:** The **Consumer** and **Retail** sectors saw the highest total number of employees laid off.
* **Geographic Focus:** The **United States** accounted for the vast majority of layoffs in this period.
* **Time-Series Analysis:** 
    * Calculated a **Monthly Rolling Total** to visualize the layoff momentum over time.
    * Ranked the **Top 3 Companies** with the most layoffs for each year using `DENSE_RANK()`.
* **Company Stages:** Large **Post-IPO** companies (e.g., Amazon, Google, Meta) were responsible for the largest single layoff events.

---

## How to Run
1. Import the raw CSV into your MySQL database as `[layoffs](data/layoffs.csv)`.
2. Run `[1_Data_Cleaning.sql](notebooks/1_Data_Cleaning.sql)` to prepare the cleaned table `layoffs_staging2`.
3. Run `[2_Exploratory_Data_Analysis.sql](notebooks/2_Exploratory_Data_Analysis.sql)` to view the analytical reports.

## Key Findings
* **Consumer, Retail, and Transportation** industries were the hardest hit globally.
* Layoffs significantly accelerated in **2022 and 2023** compared to the start of the pandemic (2020).
* Many startups (100% layoff percentage) went out of business, especially those in the **Food** and **Travel** sectors.
