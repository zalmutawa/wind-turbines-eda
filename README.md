# Wind Turbine Physical Evolution: Bigger, Taller, More Powerful?
## A Four-Decade Analysis of U.S. Wind Turbine Technology (1980–2020)

---

## Problem Statement

Wind energy technology has undergone a dramatic physical transformation over the past four decades. But how consistently did turbines grow across capacity, height, and rotor size — and did all manufacturers evolve at the same pace? This project uses the U.S. Wind Turbine Database to quantify the physical and technical evolution of onshore wind turbines installed in the United States from 1980 to 2020, and to identify which manufacturers drove — or lagged — the industry's technological advancement.

---

## Executive Summary

This project analyzes 70,808 U.S. wind turbines to document how turbine physical dimensions and power capacity changed across four decades of commercial wind development. The primary dataset, sourced from the U.S. Wind Turbine Database (USGS), contains detailed per-turbine records on capacity (kW), hub height (m), rotor diameter (m), rotor swept area (m²), installation year, manufacturer, and geographic location across 46 U.S. states.

The headline numbers are striking: average turbine capacity grew from ~72 kW in the 1980s to ~2,820 kW by the 2020s — a 39x increase. Hub heights nearly quadrupled (23m → 91m) and rotor diameters grew 8x (16m → 125m). All three physical dimensions are tightly correlated (r = 0.88 between rotor swept area and capacity), suggesting that larger physical size is the primary driver of capacity gains rather than efficiency improvements alone.

The EDA will break this trend down by manufacturer to assess whether dominant players like GE Wind, Vestas, Siemens Gamesa, and Nordex advanced at the same rate — and whether newer entrants leapfrogged incumbents. Geographic analysis will further examine whether states that installed turbines in later decades benefited disproportionately from these technological gains, and whether California's aging, lower-spec fleet represents a meaningful generation gap compared to states like Texas and Iowa.

Findings are intended to inform energy planners, policymakers, and wind farm operators evaluating the generational gap between legacy and modern fleets, and the implications for capacity repowering decisions.

---

## File Directory

```
project/
│
├── README.md                               ← You are here
│
├── data/
│   ├── original/
│   │   ├── wind-turbines.csv               ← Primary dataset (USGS Wind Turbine DB)
│   │   ├── wind-operators.csv              ← Operator-level generation data (EIA-923)
│   │   ├── average_electricity_bills.csv   ← Avg monthly electricity bills by state
│   │   ├── average_electricity_rates.csv   ← Avg electricity rates (¢/kWh) by state
│   │   ├── windiest-states-in-the-us_-2025.csv  ← Wind speed & power density by state
│   │   └── Wind_Operators_Data_Dictionary.xlsx  ← Column reference for wind-operators
│   └── cleaned/
│       └── wind_turbines_clean.csv         ← Cleaned, feature-engineered dataset
│                                              (generated in 02_Data_Cleaning)
│
├── code/
│   ├── 01_Data_Collection.ipynb            ← Data sourcing, loading, and initial inspection
│   ├── 02_Data_Cleaning.ipynb              ← Null handling, type casting, decade binning,
│   │                                          outlier flagging, and feature engineering
│   └── 03_EDA.ipynb                        ← Trend analysis, manufacturer comparisons,
│                                              geographic breakdowns, and correlation plots
│
├── presentation/
│   └── wind_turbine_evolution_analysis.pdf
│
└── scratch/                                ← Working notes (not evaluated)
```

---

## Data & Data Dictionary

**Primary:** [U.S. Wind Turbine Database (USWTDB)](https://eerscmap.usgs.gov/uswtdb/) — 70,808 turbines across 46 U.S. states, Guam, and Puerto Rico. Contains per-turbine specs including capacity, hub height, rotor diameter, swept area, manufacturer, model, installation year, and GPS coordinates.

**Secondary:** [EIA-923](https://www.eia.gov/electricity/data/eia923/) — Monthly generation and fuel consumption data by U.S. power plant (2015–2024), used to contextualize capacity differences between states with older vs. newer fleets.

**Supporting:** State-level average electricity rates (¢/kWh), electricity bills (USD/month), and wind speed/power density data — used to contextualize whether states with higher-spec turbines also benefit from stronger wind resources and lower consumer electricity costs.

For full column descriptions, data types, key analysis columns, and engineered features, see [`DATA_DICTIONARY.md`](./DATA_DICTIONARY.md).

### Key Analysis Columns

| Column | Description | Completeness |
|---|---|---|
| `t_cap` | Turbine capacity (kW) | ~92% populated |
| `t_hh` | Hub height (m) | ~91% populated |
| `t_rd` | Rotor diameter (m) | ~92% populated |
| `t_rsa` | Rotor swept area (m²) | ~92% populated |
| `p_year` | Project installation year | ~99% populated |
| `t_manu` | Turbine manufacturer | ~92% populated |
| `t_state` | U.S. state | 100% populated |

### Engineered Features (created in 02_Data_Cleaning)

- `decade` — installation decade, binned from `p_year` (1980, 1990, 2000, 2010, 2020)
- `capacity_tier` — turbine size category: sub-MW (<1000 kW), mid (1000–1999 kW), large (2000–2999 kW), utility (3000+ kW)
- `specific_power` — capacity divided by rotor swept area (W/m²), a proxy for turbine design efficiency

---

## Analytical Plan

### 1. Decade-level physical trend analysis
Track mean and median `t_cap`, `t_hh`, and `t_rd` by decade. Visualize as multi-line trend charts. Confirm whether all three dimensions grew proportionally or whether one dimension outpaced the others.

### 2. Correlation structure between physical dimensions
Compute pairwise correlations between `t_cap`, `t_hh`, `t_rd`, `t_rsa`, and `p_year`. Build a heatmap and scatter matrix to confirm that size and capacity track together, and to assess whether hub height or rotor diameter is the stronger predictor of capacity.

### 3. Manufacturer-level evolution
For each manufacturer with sufficient data across at least two decades (GE Wind, Vestas, Siemens, Siemens Gamesa, Gamesa, Nordex, Mitsubishi), plot average capacity by decade. Identify which manufacturers led in each era and which lagged or exited the market.

### 4. Geographic fleet-age analysis
Map average turbine capacity by state. Overlay with average installation year to test whether states with earlier adoption (California, the Great Plains) are sitting on lower-capacity fleets than later adopters. Quantify the capacity gap between California's avg of ~1,360 kW vs. Texas (~2,073 kW) and Iowa (~1,974 kW).

### 5. Specific power trend
Compute `specific_power` (W/m²) across decades. A declining specific power over time would indicate that rotor size grew faster than capacity — a known industry trend as developers optimized for lower wind speed sites. A flat or rising trend would indicate capacity-driven rather than rotor-driven growth.

---

## Hypotheses to Test

- **H1:** Average turbine capacity, hub height, and rotor diameter each increased monotonically across every decade from 1980 to 2020.
- **H2:** Rotor swept area is a stronger predictor of capacity than hub height alone (r > 0.85).
- **H3:** GE Wind, Vestas, and Siemens Gamesa drove above-average capacity growth, while smaller or exited manufacturers plateaued earlier.
- **H4:** States that adopted wind energy earlier (pre-2000) have a measurably lower average turbine capacity than states whose fleets are predominantly post-2010 installations.
- **H5:** Specific power (W/m²) has decreased over time, reflecting a shift toward larger rotors optimized for lower wind speed sites.

---

## Conclusions and Recommendations

*To be completed after EDA.*

---

## Areas for Further Research

- Repowering economics: given the capacity gap between legacy and modern turbines, at what electricity rate does replacing old turbines become cost-justified?
- Correlation between fleet modernity (avg `p_year` by state) and actual net generation per turbine using the EIA-923 operator dataset
- Offshore turbine comparison — are offshore designs (WS prime mover code in operators dataset) advancing faster than onshore?
- Manufacturer market exit analysis: tracking which companies disappeared between decades and whether acquisition vs. bankruptcy explains the pattern

---

## Sources

- U.S. Geological Survey. *U.S. Wind Turbine Database.* https://eerscmap.usgs.gov/uswtdb/
- U.S. Energy Information Administration. *EIA-923 Monthly Generation and Fuel Consumption Time Series.* https://www.eia.gov/electricity/data/eia923/
- WiseVoter. *Windiest States in the U.S. (2025).* https://www.wisevoter.com/state-rankings/windiest-states-in-the-us/
- U.S. EIA. *Average Retail Price of Electricity by State.* https://www.eia.gov/electricity/state/