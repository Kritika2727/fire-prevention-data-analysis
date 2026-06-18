# Project Description — Fire Prevention Data Analysis

---

## What Is This Project?

This is a **real-world data analysis project** built on fire prevention inspection records from telecom infrastructure sites across **9 CMP (Central Maintenance Point) areas** in Uttar Pradesh, India. The dataset was collected by field technicians who physically visited each site and recorded the condition of 14 specific electrical and structural safety parameters.

The project uses Python (pandas, seaborn, matplotlib, scikit-learn) to clean the raw inspection data, analyze fault patterns, and produce visualizations that help operations teams decide **where to act first and what to fix**.

---

## What Does the Raw Data Look Like?

The Excel file (`Fire_Prevention.xlsx`) contains **7,927 rows** — one row per site visit. Each row covers:

- **Site identity**: CMP area, SAP ID, site type (ENB/ESC, P1/RP1), GBM classification
- **Site safety verdict**: Fire Free Site or Fire Vulnerable Site
- **14 inspection checkpoints** — each marked OK or NOT OK by the technician on-ground
- **Faults count**: total number of NOT OK items found at that site (ranges from 0 to 14)

The 14 checkpoints cover electrical safety items like:
- MCB (circuit breaker) condition in meter boxes and DG sets
- Power cable ratings and integrity
- Electrical hygiene (loose connections, damaged insulation)
- Illegal power tapping
- Faulty battery removal
- Roxtec/exit point sealing
- Rectifier module installation
- SPD (Surge Protection Device) condition
- GBM-specific items: cable condition, deep cleaning, bird mesh, protection rings

---

## What Problems Did the Data Have?

Before analysis, the data needed significant cleaning:

- The actual column headers were in **row 2, not row 1** — the first row contained summary counts
- Many categorical columns had **inconsistent values**: `ok`, `Ok`, `OK`, `Not OK`, `NOT OK`, `Not Ok` all meaning the same thing
- Some columns contained **EMG ticket numbers** (like `EMG-4539`) entered where OK/NOT OK was expected — these were treated as OK (issue was logged in the system)
- GBM-specific columns had `NA` for non-GBM sites — these needed to be handled separately
- Several columns had **trailing spaces** in their names
- The `GBM/Non GBM` column had inconsistent naming: `GBM`, `NON GBM`, `Non GBM`, `GBT`
- CMP names had capitalization inconsistencies: `Agra` vs `AGRA`, `Bijnor` vs `BIJNOR`

---

## What Did the Analysis Find?

### Scale of the Problem
- **7,927 sites** were inspected across 9 CMP areas
- **3,675 sites (46.4%)** had at least one fault — nearly half of all inspected infrastructure
- The average faulty site had **2.15 faults** — meaning problems typically come in clusters
- The worst single site had **14 faults** (all 14 checkpoints failed)
- **3,640 sites were officially labeled "Fire Vulnerable"** — about 46% of the total

### Regional Breakdown (CMP-wise Fault Rates)

| CMP Area      | Sites Inspected | Faulty Sites | Fault Rate |
|---------------|----------------|--------------|------------|
| Aligarh       | 311            | 266          | **85.5%**  |
| Meerut        | 909            | 703          | **77.3%**  |
| BIJNOR        | 718            | 496          | 69.1%      |
| ETAWAH        | 1,377          | 824          | 59.8%      |
| Muzaffarnagar | 898            | 485          | 54.0%      |
| Hapur         | 1,127          | 351          | 31.1%      |
| Bareilly      | 1,059          | 277          | 26.2%      |
| Moradabad     | 868            | 203          | 23.4%      |
| AGRA          | 660            | 70           | **10.6%**  |

### Most Common Faults (Total NOT OK count across all sites)

| Problem Type                        | Sites Affected |
|-------------------------------------|---------------|
| MCB Replacement (faulty/underrated) | 2,330         |
| SPD Replacement (Class B & C)       | 2,206         |
| Rectifier Module Installation       | 1,655         |
| Electrical Hygiene Correction       | 1,532         |
| 4-Core Power Cable Replacement      | 1,498         |
| Illegal Power Tapping               | 1,286         |
| Roxtec/Exit Point Sealing           | 994           |
| Rectifier Bypass Correction         | 926           |
| Faulty Battery Removal              | 920           |
| Shifting BM/SMPS from GBM to ODC   | 846           |
| Bird Mesh Installation              | 738           |
| Deep Cleaning of GBM Enclosure      | 731           |
| GBM Power Cable Replacement         | 692           |
| GBM Protection Ring Installation    | 725           |

---

## What Does the Notebook Do, Step by Step?

1. **Loads** the Excel file and fixes the header row issue
2. **Drops** administrative columns not needed for analysis (S.No, SAP ID, Tech Name, visit dates)
3. **Standardizes** all OK/NOT OK variations to uniform uppercase values
4. **Handles GBM-specific columns** — marks non-GBM sites as `NO GBM` to separate them from actual faults
5. **Encodes** all categorical columns to numeric (0/1) using OrdinalEncoder and manual mapping
6. **Visualizes site distribution** per CMP area (how many sites each region has)
7. **Identifies faulty sites** per CMP (sites where Faults > 0)
8. **Computes fault vs. fine percentages** per CMP — shown as a grouped bar chart
9. **Builds a heatmap** showing which problem types are most concentrated in which regions
10. **Generates individual bar charts** for each of the 14 problem types broken down by CMP
11. **Summarizes insights and recommendations** with specific numbers per region

---

## Why Does This Matter?

Field inspection data like this is typically reviewed manually in Excel by operations managers. This analysis automates the pattern recognition — turning thousands of rows into clear visual priorities. Instead of guessing which area to send a team to next, a manager can see immediately that **Aligarh has 85% faulty sites** and that **MCB and SPD replacement** are the top two systemic issues to fix across the entire network.

The end goal is **preventing fires at telecom tower sites** by ensuring electrical safety compliance before a fault becomes a fire incident.

---

## Tech Used

- **Python 3** — core language
- **pandas** — data loading, cleaning, transformation
- **numpy** — numerical operations
- **matplotlib / seaborn** — all visualizations
- **scikit-learn** — OrdinalEncoder for categorical encoding, ML classifiers (DecisionTree, RandomForest, KNN)
- **Google Colab** — original runtime environment
