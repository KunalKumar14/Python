# Amazon Product Listings: Business and HR Data Analysis

**exploratory data project | Presented by Kunal Kumar**

This project uses exploratory data analysis (EDA) to describe Amazon product listings, displayed prices and discounts, ratings, and review counts. It turns those observations into questions a business or HR team could test with company operating data.

> **Scope:** The supplied CSV is a product-listing and customer-feedback dataset. It is not a sales or employee dataset. Listing counts and review counts must not be treated as units sold, revenue, workload, or workforce performance.

## Project files

- **Presentation:** [`presentation/Amazon_Business_HR_Analysis`](presentation/Amazon_Business_HR_Analysis)
- **Analysis notebook:** [`notebooks/AMAZON DATA PROJECT.ipynb`](notebooks/AMAZON%20DATA%20PROJECT.ipynb)
- **Dataset instructions:** [`data/README.md`](data/README.md)

## Business question

How can catalog mix, listed pricing, and customer feedback help teams decide what to investigate next, and what additional business and workforce evidence is required before acting?

## Dataset and unit of analysis

The file has **1,465 listing rows and 16 columns**. It includes product identifiers and descriptions, hierarchical categories, actual and discounted listed prices, discount percentages, ratings, review counts, review text/IDs, and product links. There are **1,351 distinct product IDs**: 114 repeated ID occurrences beyond the first, but **zero exact duplicate rows** across all columns.

The analysis treats each row as a listing record. Repeated product IDs may represent multiple listing records; their meaning should be confirmed before joining to operational tables or interpreting rows as independent products.

The raw CSV is kept locally at `data/raw/amazon.csv` for notebook use but is excluded from Git by default. It contains user identifiers and review text, and redistribution terms have not been verified. Read [`data/README.md`](data/README.md) before publishing any data.

### Column reference

| Column | Description | Analysis use |
|---|---|---|
| `product_id` | Product/listing identifier | Identity checks and joins; values repeat across rows. |
| `product_name` | Product title | Product context and optional listing review. |
| `category` | Pipe-separated category path | Roll up to the top-level category for mix analysis. |
| `discounted_price` | Displayed discounted price (rupees) | Listed-price summaries after numeric conversion. |
| `actual_price` | Displayed reference price (rupees) | Listed-price summaries and comparison with discounted price. |
| `discount_percentage` | Displayed discount (percent text) | Discount distribution and category averages. |
| `rating` | Product rating | Rating distribution; one malformed value is handled per notebook. |
| `rating_count` | Listing-level review count | Feedback-volume summaries; two missing values are median-filled. |
| `about_product` | Product description | Context only; not used to claim product quality. |
| `user_id` | Reviewer identifier | Not needed for the aggregate analysis; do not publish raw identifiers. |
| `user_name` | Reviewer name/label | Not needed for the aggregate analysis; treat as sensitive. |
| `review_id` | Review identifier | Not needed for the aggregate analysis. |
| `review_title` | Review title text | Not needed for aggregate summaries; review text needs privacy review. |
| `review_content` | Review body text | Not needed for aggregate summaries; do not publish without permission. |
| `img_link` | Product image URL | Not needed for aggregate summaries. |
| `product_link` | Product page URL | Not needed for aggregate summaries. |

The raw CSV stores several numeric-looking fields as text (for example, rupee prices, percentage strings, comma-separated review counts, and the malformed rating). The notebook converts the relevant fields before calculating summaries.

## Exploratory analysis and preparation

The notebook explores category counts, listed prices and discounts, rating and review-count distributions, category averages, and pairwise correlations.

Preparation steps documented in the supplied notebook:

1. Remove the rupee symbol and thousands separators from price fields; convert prices to numeric values.
2. Convert discount percentages to numeric values for calculations.
3. Replace the single malformed rating value (`|`) with `3.9`, following the notebook's stated rule.
4. Convert review counts to numbers and fill the two missing values with the column median (**5,179**).
5. Group the pipe-separated category paths by their first, top-level category for category summaries.

The reported summary statistics are descriptive and row-level. Imputation and the single rating correction should be revisited if a more authoritative source is available.

## Findings supported by this file

| Area | Observation | Useful business follow-up |
|---|---|---|
| Catalog mix | Electronics has 526 rows, Computers & Accessories 453, and Home & Kitchen 448. Together they make up 1,427 of 1,465 rows (**97.4%**). Office Products has 31 rows; the remaining five categories total 7 rows. | These categories dominate this sample's catalog coverage. Compare with sales, margin, inventory, and service data before setting commercial priorities. |
| Listed discounts | The mean listed discount is **47.7%**, the median is **50%**, and about **47.4%** of rows show a discount above 50%. Category mean discounts are 54.0% for Computers & Accessories, 50.8% for Electronics, 40.1% for Home & Kitchen, and 12.4% for Office Products. | Treat this as a description of displayed prices. Test promotion performance against orders, conversion, margin, and returns before changing discount policy. |
| Ratings | The mean product rating is about **4.10/5**; **75.8%** of rows have a rating of 4.0 or higher. | Ratings are mostly positive in this sample. Review low-rated listings with complaint, quality, and return information to understand causes. |
| Review counts | The median is **5,179** and the mean is about **18,278** per row after median imputation. The much higher mean indicates a strongly uneven distribution. | Review counts can help describe feedback volume or visibility, but they are not sales or unique customer counts. |
| Relationships | Pearson correlation is about **0.12** between actual listed price and rating, and **-0.15** between discount percentage and rating. These are weak associations. | Price and discount alone are weak rating indicators in this sample. Correlation does not show that price or discount caused a rating change. |

Values are rounded for readability. Category labels in the report are spaced for presentation; the CSV uses joined labels such as `Computers&Accessories` and `Home&Kitchen`.

## What the data does not establish

The CSV has no order or transaction records, units sold, dates, revenue, gross margin/profit, conversion, inventory, availability, returns, customer-service cases, employee records, team hours, roles, skills, staffing costs, or workforce outcomes. Therefore this project cannot establish:

- Sales performance, demand, seasonality, market share, or business growth.
- Whether a discount increased sales, conversion, or profit.
- Whether ratings or review volume caused customer behavior or product success.
- Staffing needs, productivity, skill gaps, employee performance, or HR outcomes.
- A causal effect from any relationship or category difference in the file.

A category's share of listing rows is not its share of sales or operational workload. The dataset is a sample; its coverage period and representativeness should be verified before generalizing to Amazon's full catalog or current business conditions.

## Business and HR use

### Business teams

- Use category counts to choose where to perform a deeper review of listings.
- Compare displayed discount practices across categories, then validate with dated SKU-level orders, net revenue, gross margin, conversion, inventory, and returns.
- Review low-rated or low-feedback products alongside complaint reasons and quality data.
- Pilot one change with a defined baseline and measurement period; monitor commercial results and customer outcomes together.

### HR and people operations

- Treat category concentration as a prompt to ask about ownership and capability coverage, not as evidence of a staffing gap.
- Compare team capacity with measured work such as orders, support cases, labor hours, service levels, and staffing cost.
- Review skills for pricing, merchandising, product operations, quality, and customer care only after the related workload and outcomes are measured.
- Do not make individual employee or staffing decisions from this dataset.

## Recommended next data to join

For a stronger business analysis, join listings to dated SKU-level orders and include units, net sales, gross margin, inventory, stockouts, and returns. Add customer-service contact reasons, response times, and resolution outcomes. For a governed HR view, use aggregated team-level roles, skills coverage, labor hours, service levels, and staffing cost; limit access to personal employee data.

For a pilot, agree on a baseline and comparison period first. Select business measures such as conversion, gross margin, return rate, or case resolution, with service and workload guardrails.

## Repository structure

```text
Amazon_Business_HR_Analysis_GitHub/
├── README.md
├── .gitignore
├── requirements.txt
├── data/
│   ├── README.md
│   └── raw/
│       └── amazon.csv          # local only; ignored by Git
├── notebooks/
│   └── AMAZON DATA PROJECT.ipynb
└── presentation/
    └── Amazon_Business_HR_Analysis_Brown_Beige_Reordered.pptx
```

## Run the notebook

1. Install Python 3.10 or newer.
2. Create and activate a virtual environment.
3. Install the packages listed in `requirements.txt`.
4. Obtain an authorized copy of the dataset and save it as `data/raw/amazon.csv`.
5. Open `notebooks/AMAZON DATA PROJECT.ipynb` from the repository root and run the cells in order.

```bash
python -m venv .venv
# Windows PowerShell
.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
jupyter notebook "notebooks/AMAZON DATA PROJECT.ipynb"
```

The GitHub-ready notebook copy uses a relative dataset path and has saved cell outputs cleared. The original notebook in the project folder is not changed.

## Publish to GitHub

Create an empty repository on GitHub, then run these commands from the extracted project folder. Replace the placeholder remote URL with your own repository URL.

```bash
git init
git add README.md .gitignore requirements.txt data/README.md data/raw/.gitkeep notebooks presentation
git status --short
git commit -m "Add Amazon business and HR EDA project"
git branch -M main
git remote add origin https://github.com/<your-username>/amazon-business-hr-analysis.git
git push -u origin main
```

Before pushing, confirm that `data/raw/amazon.csv` is not staged. The `.gitignore` excludes raw CSVs by default. Do not force-add the raw dataset or any user/reviewer identifiers, credentials, or private company data unless you have confirmed the license, privacy requirements, and permission to redistribute them.

## Tools

Python, Jupyter Notebook, pandas, NumPy, Matplotlib, and Seaborn.
