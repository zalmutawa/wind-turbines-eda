# Wind Turbine Retrofit Analysis
## Are Retrofitted Turbines Significantly More Powerful?

---

## Problem Statement

Wind energy operators and policymakers need to understand whether retrofitting aging turbines translates into meaningful gains in power capacity. This project uses U.S. wind turbine data to determine whether retrofitted turbines are significantly more powerful than their non-retrofitted counterparts, and to explore what factors (geography, wind resource, operator scale) may influence that relationship.

---

## Executive Summary

This project analyzes a dataset of 70,808 U.S. wind turbines to evaluate whether turbines that have undergone retrofits produce meaningfully greater power output than those that have not. The primary dataset, sourced from the U.S. Wind Turbine Database (USGS), contains detailed records on turbine capacity, hub height, rotor dimensions, location, and retrofit status. Supplementary datasets from the U.S. Energy Information Administration (EIA), NREL, and publicly available electricity rate data provide additional context around operators, generation output, average electricity costs, and wind resources by state.

The analysis centers on comparing turbine capacity (`t_cap`, measured in kW) between retrofitted (`retrofit = 1`) and non-retrofitted (`retrofit = 0`) turbines. Roughly 6,100 turbines in the dataset have been retrofitted, with most retrofits occurring between 2017 and 2020. The EDA will use descriptive statistics, distribution plots, and group comparisons to assess whether the observed differences in capacity are statistically and practically meaningful. Wind speed data by state is incorporated to control for the possibility that high-wind states both retrofit more and naturally support more powerful turbines.

Preliminary exploration suggests that retrofitted turbines skew toward more recent vintages and higher individual capacities, but the full EDA will determine whether this relationship holds after accounting for confounding variables such as state-level wind resources, turbine manufacturer, and operator type. Findings will inform recommendations for wind farm operators and clean energy planners evaluating whether retrofit programs are a cost-effective path to increasing generation capacity.

---

## File Directory

```
project/
│
├── README.md                          ← You are here
│
├── data/
│   ├── original/
│   │   ├── wind-turbines.csv          ← Primary dataset (USGS Wind Turbine DB)
│   │   ├── wind-operators.csv         ← Operator-level generation data (EIA-923)
│   │   ├── average_electricity_bills.csv   ← Avg monthly electricity bills by state
│   │   ├── average_electricity_rates.csv   ← Avg electricity rates (¢/kWh) by state
│   │   ├── windiest-states-in-the-us_-2025.csv ← Wind speed & power data by state
│   │   └── Wind_Operators_Data_Dictionary.xlsx ← Column reference for wind-operators
│   └── cleaned/
│       └── wind_turbines_clean.csv    ← Cleaned, merged dataset (generated in 02_Data_Cleaning)
│
├── code/
│   ├── 01_Data_Collection.ipynb       ← Data sourcing, loading, and initial inspection
│   ├── 02_Data_Cleaning.ipynb         ← Null handling, type casting, feature engineering
│   └── 03_EDA.ipynb                   ← Exploratory analysis, visualizations, statistical comparison
│
├── presentation/
│   └── wind_turbine_retrofit_analysis.pdf
│
└── scratch/                           ← Working notes (not evaluated)
```

---

## Data & Data Dictionary

**Primary:** [U.S. Wind Turbine Database (USWTDB)](https://eerscmap.usgs.gov/uswtdb/) — 70,808 turbines across 44 U.S. states, Guam, and Puerto Rico.

**Secondary:** [EIA-923](https://www.eia.gov/electricity/data/eia923/) — Monthly generation and fuel consumption data by U.S. power plants (2015 subset).

**Supporting:** State-level average electricity bills, electricity rates (¢/kWh), and wind speed/power density data.

For full column descriptions, data types, key analysis columns, and engineered features, see [`DATA_DICTIONARY.md`](./DATA_DICTIONARY.md).

---

## Conclusions and Recommendations

*To be completed after EDA.*

---

## Areas for Further Research

- Modeling capacity gains as a function of turbine age, manufacturer, and wind resource
- Economic analysis: do capacity gains from retrofits offset retrofit costs given local electricity rates?
- Comparison of retrofit frequency across NERC grid regions
- Time-series analysis of net generation before and after retrofit years using operator data

---

## Sources

- U.S. Geological Survey. *U.S. Wind Turbine Database.* https://eerscmap.usgs.gov/uswtdb/
- U.S. Energy Information Administration. *EIA-923 Monthly Generation and Fuel Consumption Time Series.* https://www.eia.gov/electricity/data/eia923/
- WiseVoter. *Windiest States in the U.S. (2025).* https://www.wisevoter.com/state-rankings/windiest-states-in-the-us/
- U.S. EIA. *Average Retail Price of Electricity by State.* https://www.eia.gov/electricity/state/
