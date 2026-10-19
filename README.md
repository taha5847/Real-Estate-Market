# Danish Housing Market Dashboard (Power BI)

An interactive Power BI report analysing **100,000 Danish residential property sales (1992 – Oct 2024)**: price trends, regional and house-type differences, offer-vs-purchase price behaviour, and the influence of macro-economic indicators (interest rate, inflation, mortgage bond yield).

## Repository Contents

| File | Description |
|---|---|
| `Housing_Data.pbix` | Power BI report (data model + 3 dashboard pages) |
| `Housing_Data.csv` | Source dataset: 100,000 rows × 19 columns (~14 MB) |
| `Housing_Data_Columns_Defination.xlsx` | Data dictionary for all columns |
| `images/` | Dashboard screenshots used in this README |

## Dashboard Pages

### 1. House Market Overview
 https://github.com/taha5847/Real-Estate-Market/blob/62d56b1fa5b300a2ab22a33a2c07946689c3b5ce/overviewpage.png

High-level market health at a glance.
- **KPI cards:** Last 12-Month Sales, Units Sold in the latest year & quarter
- **Bar chart:** Median Sales Price Change (YoY) by region
- **Line chart:** YoY Sales Growth by sales type
- **Scatter chart:** Offer price vs. purchase price

### 2. Sales Performance Page
![Sales Performance](<img width="605" height="339" alt="salespage" src="https://github.com/user-attachments/assets/6ddb69b4-7fa9-40d0-8bc8-9ce312ddbb65" />
)

Where and when sales happen.
- **Bar chart:** Sales by Region
- **Donut chart:** Average Price per SQM by region
- **Column chart:** Offer to SQM Ratio by sales type
- **Table:** Purchase price and Total YTD Sales by Year → Quarter → Month → Day
- **Key Influencers visual:** What drives purchase price (e.g. age of the house)

### 3. House Type Analysis
![House Type Analysis](<img width="602" height="332" alt="housetype" src="https://github.com/user-attachments/assets/93ff857a-231d-469c-963a-43ac4d4aa62b" />
)

Drill-down by property type, with slicers for **Region, Area, City and Sales Type**.
- Average offer price vs. average purchase price by house type
- Total area (sqm) and price per sqm by house type
- Inflation, nominal interest rate and mortgage bond yield by house type

## Data Dictionary

| Column | Description |
|---|---|
| `date` | Date of the transaction |
| `quarter` | Quarter of the sale (e.g. `2024Q4`) |
| `house_id` | Unique ID for each house |
| `house_type` | Villa, Apartment, Summerhouse, Townhouse, Farm |
| `sales_type` | `regular_sale`, `family_sale`, `other_sale`, `auction` |
| `year_build` | Year the house was built |
| `purchase_price` | Final purchase price |
| `%_change_between_offer_and_purchase` | % difference between offer and purchase price |
| `no_rooms` | Number of rooms |
| `sqm` | Living area in square metres |
| `sqm_price` | Price per square metre |
| `address` | Street address |
| `zip_code` | Postal code |
| `city` | City / urban area |
| `area` | District or part of the country (8 areas) |
| `region` | Administrative region (Jutland, Zealand, Fyn & islands, Bornholm) |
| `nom_interest_rate%` | Nominal mortgage interest rate |
| `dk_ann_infl_rate%` | Annual inflation rate in Denmark |
| `yield_on_mortgage_credit_bonds%` | Yield on mortgage credit bonds |

**Derived in Power BI:** `Age of the House` and `offer Price` (calculated columns), plus DAX measures such as *Average Price SQM*, *Offer to SQM Ratio*, *Sales by Region*, *TotalYTD Sales*, *Last 12 Month Sales*, *YOY Sales Growth* and *Median Sales Price Change (YOY)*.

## Dataset Snapshot

| | |
|---|---|
| Records | 100,000 sales (100,000 unique `house_id`s, no duplicate rows) |
| Period | 5 Jan 1992 – 24 Oct 2024 |
| Regions | Jutland (50%), Zealand (40%), Fyn & islands (9%), Bornholm (1%) |
| House types | Villa 54%, Apartment 19%, Summerhouse 11%, Townhouse 10%, Farm 5% |
| Sales types | Regular 88%, Family 8%, Other 2.5%, Auction 1% |
| Median purchase price | 1.4M (mean 1.93M) |
| Median size | 123 sqm |

## Highlights from the Data

- **Long-term growth:** median purchase price rose from about 650K in 1992 to about 2.0M in 2024, and median price per sqm from about 4.8K to about 18.5K.
- **2008 shock:** prices fell after 2007, then stayed flat from 2009 to 2013 before a sustained recovery.
- **Regional gap:** Zealand's median price per sqm (about 17.1K) is almost double Jutland's (about 9.8K) and Bornholm's (about 8.1K).
- **Apartments are the priciest per sqm** (about 25.4K), far above villas (about 9.1K) and farms (about 9.4K).
- **Auctions sell cheapest** (about 7.4K per sqm vs. about 12.5K for regular sales) and show the biggest average drop from offer to final price (about -4%).
- **Most deals close at the offer price:** 64% show 0% change, 34% close below the offer.
- **Interest rates:** the nominal rate sat at 0% from 2013 to 2021, then climbed to about 3.5% by 2024, alongside slowing sales volumes (7,837 sales in 2021 vs. 4,108 so far in 2024, a partial year).

## Data Quality Notes

- **Currency:** not specified in the data dictionary. Prices appear to be Danish kroner (DKK).
- **Missing macro data:** `dk_ann_infl_rate%` and `yield_on_mortgage_credit_bonds%` are blank for 77 rows, all from October 2024.
- **Missing city:** 11 rows have no `city`.
- **`year_build` outliers:** 92 properties are recorded as built in year 1000 (a placeholder) and 778 before 1800. This skews the *Age of the House* calculation and the Key Influencers visual. Consider cleaning or filtering.
- **Partial final year:** 2024 contains data only up to 24 October, so year-over-year comparisons to full years need care.
- **Summing rates:** the House Type page sums percentage columns (inflation, interest rate, yield). Averages are more meaningful for rates.

## How to Use

1. Download or clone the repository.
2. Open `Housing_Data.pbix` in **Power BI Desktop** (Windows).
3. If prompted, point the data source to the local `Housing_Data.csv` (**Home → Transform data → Data source settings**).
4. Use the page tabs and slicers (Region, Area, City, Sales Type) to explore.

## Tools

Power BI Desktop, DAX, Power Query. Data prep and profiling were also checked with Python (pandas).

## Possible Next Steps

- Clean `year_build` outliers and recompute house age
- Add a forecast of price per sqm by region
- Use average rather than summed rates on the House Type page
- Add a map visual using `zip_code`
