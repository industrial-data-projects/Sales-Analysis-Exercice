# 📊 Sales Analysis — Electronics Store (2019)

![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-data%20wrangling-150458?logo=pandas&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-interactive%20charts-3F4F75?logo=plotly&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)

End-to-end exploratory data analysis of **12 months of sales transactions (~186k rows)** from a US electronics retailer.
The project merges raw monthly exports, audits and cleans the data with a logged pipeline, engineers time and location features, and answers five business questions with interactive **Plotly** visualisations.

---

## 🎯 Business questions

| # | Question |
|---|---|
| 1 | What was the best month for sales, and how much was earned? |
| 2 | Which city generated the most revenue? |
| 3 | When should ads be displayed to maximise the likelihood of a purchase? |
| 4 | Which products are most often sold together? |
| 5 | Which product sold the most, and why? |

## 💡 Key findings

| Topic | Finding | Recommendation |
|---|---|---|
| Seasonality | **December** is the best month (holiday season); revenue tracks order count | Ramp up stock and marketing from October |
| Geography | **San Francisco** leads (~$8.3M), then Los Angeles and New York City | Prioritise these markets |
| Timing | Order peaks at **11:00–12:00** and **18:00–20:00**, every day of the week | Schedule ads around these windows |
| Basket | **Phone + matching charging cable** (iPhone + Lightning, Google Phone + USB-C) | Bundles and "frequently bought together" offers |
| Volume | **AAA / AA batteries** sell the most units; MacBook Pro and iPhone bring the most revenue | Use consumables as traffic drivers, upsell high-value items |

> Exact figures are computed in the notebook after cleaning (duplicates and out-of-period orders removed).

---

## 📁 Input data

Twelve CSV files, one per month of 2019 (`Sales_January_2019.csv` … `Sales_December_2019.csv`).
Each row is **one product line within an order**.

| Column | Type | Description | Example |
|---|---|---|---|
| `Order ID` | string | Order identifier, shared by all lines of an order | `176558` |
| `Product` | category | Product name (19 products) | `USB-C Charging Cable` |
| `Quantity Ordered` | int | Units of this product in the order | `2` |
| `Price Each` | float | Unit price in USD | `11.95` |
| `Order Date` | datetime | `MM/DD/YY HH:MM` | `04/19/19 08:46` |
| `Purchase Address` | string | `street, city, STATE zip` | `917 1st St, Dallas, TX 75001` |

**Getting the data:** the dataset comes from Keith Galli's [Pandas-Data-Science-Tasks](https://github.com/KeithGalli/Pandas-Data-Science-Tasks) repository (`SalesAnalysis/Sales_Data`). Copy the 12 CSV files into `data/Sales_Data/`.

---

## 🧹 Data preprocessing & cleaning

The raw data is audited first, then cleaned by a single reusable function that logs every step.

| Step | Action | Reason |
|---|---|---|
| 1 | Drop fully blank rows | No information |
| 2 | Drop repeated header rows embedded in the files | Break numeric/date conversion |
| 3 | Trim whitespace in text columns | Consistent grouping |
| 4 | Convert types (`int`, `float`, `datetime`) safely | Enable calculations |
| 5 | Drop rows that fail conversion | Corrupted records |
| 6 | Drop exact duplicate rows | Avoid double-counted revenue |
| 7 | Keep positive quantities / prices only | Sanity check |
| 8 | Keep 2019 orders only | December file contains 1 Jan 2020 orders |

The pipeline ends with **validation assertions** (no missing values, no duplicates, consistent price per product) and a funnel chart showing rows remaining after each step.

**Engineered features:** `Sales`, `Month`, `Month Name`, `Day of Week`, `Hour`, `City`, `State`, `Zip`, and `City (State)` (to keep Portland OR and Portland ME separate).

---

## 📈 Visualisations

All charts are built with Plotly: data-quality overview, cleaning funnel, KPI cards, monthly revenue vs. orders (dual axis), revenue by city (bar + US map), orders per hour and a day × hour heatmap, top product pairs and a co-occurrence matrix, units vs. price (dual axis), price–volume scatter and a revenue treemap.

> ⚠️ GitHub does not render interactive Plotly charts. To view them, open the notebook locally, use [nbviewer](https://nbviewer.org/), or set `RENDERER = "png"` in the setup cell (requires `kaleido`) and re-run before committing so static images are embedded.

---

## 🗂️ Project structure

```
sales-analysis/
├── data/
│   └── Sales_Data/            # 12 monthly CSV files (input)
├── output/                    # generated: clean dataset + cleaning log (git-ignored)
├── Sales_Analysis.ipynb       # main notebook
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

## ⚙️ Getting started

```bash
git clone https://github.com/<your-username>/sales-analysis.git
cd sales-analysis

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# place the 12 CSV files in data/Sales_Data/, then:
jupyter notebook Sales_Analysis.ipynb
```

Run all cells from top to bottom. Outputs are written to `output/`:
- `sales_2019_clean.csv` — cleaned and enriched dataset
- `cleaning_log.csv` — rows removed at each cleaning step

## 🔭 Possible improvements

- Add cost data to analyse **profit** instead of revenue
- Market-basket analysis with support / confidence / lift (e.g. `mlxtend`)
- Monthly sales forecasting
- Interactive dashboard (Dash or Streamlit)

## 📄 License

Released under the [MIT License](LICENSE).
