# CareVisit Analytics

## Understanding the Factors Associated with Doctor Visits

CareVisit Analytics is an exploratory healthcare data analysis project that studies how demographic, financial, health, and healthcare-support factors are associated with the number of doctor visits reported by individuals.

The project uses the `DoctorVisits` dataset, containing 5,190 individuals from the Australian Health Survey conducted in 1977–1978. It uses descriptive statistics, group comparisons, visualizations, and correlations.

> This project is for educational and analytical purposes. It does not provide medical advice, diagnosis, or causal conclusions.

## Objectives

- Measure the distribution of reported doctor visits.
- Identify the proportion of respondents with zero visits.
- Compare visits across gender, age, income, illness, and healthcare-support groups.
- Examine whether activity-limiting chronic conditions are associated with more visits.
- Communicate the results through a reproducible Python notebook.

## Dataset

The CSV contains 5,190 records and 13 columns. `Unnamed: 0` is a row identifier and is excluded from analytical calculations.

| Column | Description |
|---|---|
| `visits` | Doctor visits during the previous two weeks |
| `gender` | Recorded gender |
| `age` | Age divided by 100; `0.19` represents 19 years |
| `income` | Annual income in tens of thousands of dollars |
| `illness` | Illnesses reported during the previous two weeks |
| `reduced` | Days when normal activity was reduced because of illness or injury |
| `health` | General health questionnaire score |
| `private` | Private health insurance indicator |
| `freepoor` | Free government healthcare due to low income |
| `freerepat` | Free healthcare due to age, disability, or veteran status |
| `nchronic` | Chronic condition that does not limit activity |
| `lchronic` | Chronic condition that limits activity |
| `Unnamed: 0` | Row identifier |

## Main findings

- The dataset has **5,190 records** and **no missing values**.
- **4,141 respondents**, or approximately **79.8%**, reported zero visits.
- The dataset contains **1,566 total reported visits**.
- The overall average was approximately **0.302 visits per person**.
- Respondents with an activity-limiting chronic condition averaged **0.603 visits**.
- Respondents without one averaged **0.262 visits**.
- The activity-limiting chronic-condition group reported approximately **2.3 times as many visits on average**.
- Reduced-activity days had the strongest positive correlation with visits among the numeric variables examined: approximately **0.419**.

These are associations in the sample, not proof of causation.

## Project structure

```text
carevisit-analytics/
├── 1776250375-P2-Healthcare Analytics for Doctor Visits.csv
├── Healthcare_Analytics_Doctor_Visits.ipynb
├── README.md
└── requirements.txt
```

## Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook or Visual Studio Code

## Installation

```bash
python -m venv venv
```

Windows PowerShell:

```powershell
venv\Scripts\Activate.ps1
```

macOS or Linux:

```bash
source venv/bin/activate
```

Install the dependencies:

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

Keep the CSV and notebook in the same folder.

## Running the project

Open the notebook and run the cells from top to bottom. Use the exact filename, including spaces:

```python
import pandas as pd

DATA_PATH = "1776250375-P2-Healthcare Analytics for Doctor Visits.csv"
df_raw = pd.read_csv(DATA_PATH)

print("Shape:", df_raw.shape)
display(df_raw.head())
```

The notebook checks data quality, removes the identifier from analysis, converts age into years, summarizes visits, compares groups, creates visualizations, and interprets the results.

## Example analysis

```python
import pandas as pd

df = pd.read_csv("1776250375-P2-Healthcare Analytics for Doctor Visits.csv")
data = df.drop(columns=["Unnamed: 0"]).copy()
data["age_years"] = (data["age"] * 100).round().astype(int)

print("Rows and columns:", df.shape)
print("Missing values:", df.isna().sum().sum())
print("Zero-visit percentage:", round(data["visits"].eq(0).mean() * 100, 2))

chronic_summary = data.groupby("lchronic")["visits"].agg(
    ["count", "mean", "median", "sum"]
)
print(chronic_summary)

numeric_columns = ["visits", "age", "income", "illness", "reduced", "health"]
correlations = data[numeric_columns].corr()["visits"].sort_values(ascending=False)
print(correlations)
```

## Suggested visualizations

- Distribution of doctor visits
- Zero-visit versus non-zero-visit respondents
- Average visits by gender
- Average visits by age group
- Average visits by activity-limiting chronic condition
- Average visits by illness count
- Correlation heatmap for numeric variables

Example:

```python
import matplotlib.pyplot as plt
import seaborn as sns

summary = data.groupby("lchronic", as_index=False)["visits"].mean()
sns.barplot(data=summary, x="lchronic", y="visits", color="#2C84AF")
plt.title("Average Doctor Visits by Activity-Limiting Chronic Condition")
plt.xlabel("Activity-limiting chronic condition")
plt.ylabel("Average visits in previous two weeks")
plt.tight_layout()
plt.show()
```

## Interpretation

Health-related variables show stronger associations with doctor visits than income or demographic variables in this dataset. People whose daily activity was limited by a chronic condition reported more visits on average. Reduced-activity days also had the strongest linear association with visits among the numeric variables examined.

The outcome is heavily concentrated at zero, so a simple average does not describe the entire distribution. A future extension could use Poisson, negative-binomial, or zero-inflated count models.

## Limitations

- The data come from the 1977–1978 Australian Health Survey and may not represent current healthcare use.
- They should not be presented as current Indian healthcare estimates.
- The analysis is observational and descriptive.
- Correlation does not establish causation.
- The outcome contains a large number of zero-visit records.
- Several variables are scaled or encoded and require careful interpretation.
- Unobserved factors may influence both health and doctor visits.
- The project should not be used for individual medical decisions.

## Future improvements

- Build an interactive Streamlit or Power BI dashboard.
- Add detailed age-group and income-group comparisons.
- Apply Poisson and negative-binomial regression.
- Compare zero-visit and non-zero-visit respondents with a classification model.
- Add confidence intervals and suitable statistical tests.
- Use a recent, geographically relevant healthcare dataset.

## Reproducibility

Keep the notebook and CSV in the same directory, install the dependencies, and run the notebook cells in order. If the CSV is renamed, update `DATA_PATH` accordingly.

## Data source

This dataset corresponds to the `DoctorVisits` dataset documented by the AER R package as Australian Health Service Utilization Data from the 1977–1978 Australian Health Survey.

Source: [AER DoctorVisits documentation](https://search.r-project.org/CRAN/refmans/AER/html/DoctorVisits.html)

## Author

**Ayush Bhatt**  
B.Tech Information Technology  
Maharaja Surajmal Institute of Technology
