# Data Dictionary
## Wind Turbine Retrofit Analysis

---

## `wind-turbines.csv` — Primary Dataset
**Source:** [U.S. Wind Turbine Database (USWTDB)](https://eerscmap.usgs.gov/uswtdb/)
**Rows:** 70,808 | **Unit of observation:** Individual turbine

| Column | Type | Description |
|---|---|---|
| `case_id` | int | Unique turbine identifier |
| `faa_ors` / `faa_asn` | str | FAA obstacle repository / aeronautical study number |
| `usgs_pr_id` | int | USGS project ID |
| `eia_id` | int | EIA plant ID (links to wind-operators.csv) |
| `t_state` | str | State abbreviation where turbine is located |
| `t_county` | str | County name |
| `t_fips` | str | FIPS county code |
| `p_name` | str | Wind plant/project name |
| `p_year` | int | Year the project became operational |
| `p_tnum` | int | Number of turbines in the project |
| `p_cap` | float | Total project capacity (MW) |
| `t_manu` | str | Turbine manufacturer |
| `t_model` | str | Turbine model |
| **`t_cap`** ⭐ | float | **Individual turbine capacity (kW) — primary outcome variable** |
| `t_hh` | float | Hub height (meters) |
| `t_rd` | float | Rotor diameter (meters) |
| `t_rsa` | float | Rotor swept area (m²) |
| `t_ttlh` | float | Total height to blade tip (meters) |
| **`retrofit`** ⭐ | int | **Retrofit flag — 1 = retrofitted, 0 = not (primary grouping variable)** |
| **`retrofit_year`** ⭐ | float | **Year of retrofit (where applicable)** |
| `t_conf_atr` | int | Confidence in turbine attributes (1–3 scale) |
| `t_conf_loc` | int | Confidence in turbine location (1–3 scale) |
| `t_img_date` | str | Date of imagery used for verification |
| `t_img_srce` | str | Imagery source |
| `xlong` | float | Longitude |
| `ylat` | float | Latitude |

---

## `wind-operators.csv` — Operator Generation Data
**Source:** [EIA-923](https://www.eia.gov/electricity/data/eia923/)
**Rows:** ~14,400 | **Unit of observation:** Plant-year

| Column | Type | Description |
|---|---|---|
| `Plant Id` | int | EIA plant ID (links to wind-turbines.csv via `eia_id`) |
| `Plant Name` | str | Name of the generating plant |
| `Operator Name` | str | Utility or operator name |
| `Plant State` | str | State where plant is located |
| `NERC Region` | str | Grid reliability region |
| `Sector Name` | str | Utility sector classification |
| `Netgen [Month]` | float | Net monthly generation (MWh) — 12 columns (Jan–Dec) |
| `Net Generation (Megawatthours)` | float | Total annual net generation |
| `YEAR` | int | Reporting year |

---

## `average_electricity_bills.csv`
**Rows:** 49 (states) | Monthly average electricity bills in USD

| Column | Type | Description |
|---|---|---|
| `State` | str | U.S. state name |
| `Residential` | float | Avg monthly residential bill ($) |
| `Commercial` | float | Avg monthly commercial bill ($) |
| `Average` | float | Avg across both sectors ($) |

---

## `average_electricity_rates.csv`
**Rows:** 50 (states) | Average electricity rates in cents per kWh

| Column | Type | Description |
|---|---|---|
| `State` | str | U.S. state name |
| `Residential` | float | Avg residential rate (¢/kWh) |
| `Commercial` | float | Avg commercial rate (¢/kWh) |
| `Average` | float | Avg across both sectors (¢/kWh) |

---

## `windiest-states-in-the-us_-2025.csv`
**Rows:** 49 (states) | Wind resource data by state

| Column | Type | Description |
|---|---|---|
| `state` | str | U.S. state name |
| `WindiestStatesAverageWindSpeedMPH` | float | Average wind speed (MPH) |
| `MeanWindSpeed328ft` | float | Mean wind speed at 100m elevation (mph) |
| `MeanWindPower328ft` | float | Mean wind power density at 100m (W/m²) |
| `MeanWindSpeed33ft` | float | Mean wind speed at 10m elevation (mph) |

---

## Key Columns for Analysis

⭐ = directly tied to the problem statement

| Column | Dataset | Role |
|---|---|---|
| `retrofit` ⭐ | wind-turbines | Primary grouping variable (retrofitted vs. not) |
| `retrofit_year` ⭐ | wind-turbines | Temporal analysis of retrofit trends |
| `t_cap` ⭐ | wind-turbines | Primary outcome variable — turbine capacity (kW) |
| `t_rd` / `t_rsa` | wind-turbines | Physical dimensions — proxy for generation potential |
| `t_hh` | wind-turbines | Hub height — related to wind capture efficiency |
| `p_year` | wind-turbines | Project age — contextualizes retrofit timing |
| `t_state` | wind-turbines | Geographic grouping for state-level comparisons |
| `t_manu` | wind-turbines | Manufacturer — checks if retrofits cluster by brand |
| `Net Generation (Megawatthours)` | wind-operators | Operator-level output for cross-dataset validation |
| `MeanWindSpeed328ft` / `MeanWindPower328ft` | windiest-states | Control variable — state wind resource quality |
| `Average` (rates) | electricity_rates | Contextual — economic value of capacity gains by state |

---

## Engineered Features
*Planned for `02_Data_Cleaning.ipynb`*

| Feature | Source Columns | Description |
|---|---|---|
| `turbine_age_at_retrofit` | `retrofit_year - p_year` | How old a turbine was when retrofitted |
| `is_retrofitted` | `retrofit` | Boolean version of retrofit flag for cleaner analysis |
| `wind_resource_tier` | `MeanWindSpeed328ft` | Binned wind speed category (Low / Medium / High) |
