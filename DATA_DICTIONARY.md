# Data Dictionary
## Wind Turbine Physical Evolution: Bigger, Taller, More Powerful?

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
| **`p_year`** ⭐ | int | **Year the project became operational — primary time axis** |
| `p_tnum` | int | Number of turbines in the project |
| `p_cap` | float | Total project capacity (MW) |
| **`t_manu`** ⭐ | str | **Turbine manufacturer — used for manufacturer evolution analysis** |
| `t_model` | str | Turbine model |
| **`t_cap`** ⭐ | float | **Individual turbine capacity (kW) — primary outcome variable** |
| **`t_hh`** ⭐ | float | **Hub height (meters) — physical dimension #1** |
| **`t_rd`** ⭐ | float | **Rotor diameter (meters) — physical dimension #2** |
| **`t_rsa`** ⭐ | float | **Rotor swept area (m²) — physical dimension #3, correlated with capacity at r=0.88** |
| `t_ttlh` | float | Total height to blade tip (meters) |
| `retrofit` | int | Retrofit flag — 1 = retrofitted, 0 = not (not a focus of this analysis) |
| `retrofit_year` | float | Year of retrofit, where applicable |
| `t_conf_atr` | int | Confidence in turbine attribute data (1=low, 3=high) — used for quality filtering |
| `t_conf_loc` | int | Confidence in turbine location data (1=low, 3=high) |
| `t_img_date` | str | Date of imagery used for verification |
| `t_img_srce` | str | Imagery source |
| `xlong` | float | Longitude — used for geographic fleet-age mapping |
| `ylat` | float | Latitude — used for geographic fleet-age mapping |

---

## `wind-operators.csv` — Operator Generation Data
**Source:** [EIA-923](https://www.eia.gov/electricity/data/eia923/)
**Rows:** ~14,400 | **Unit of observation:** Plant-year

Used to contextualize whether states with higher-spec (newer, larger) turbine fleets translate their physical advantages into measurably greater net generation output.

| Column | Type | Description |
|---|---|---|
| `Plant Id` | int | EIA plant ID (links to wind-turbines.csv via `eia_id`) |
| `Plant Name` | str | Name of the generating plant |
| `Operator Name` | str | Utility or operator name |
| `Plant State` | str | State where plant is located |
| `NERC Region` | str | Grid reliability region |
| `Sector Name` | str | Utility sector classification |
| `Netgen [Month]` | float | Net monthly generation (MWh) — 12 columns (Jan–Dec) |
| **`Net Generation (Megawatthours)`** ⭐ | float | **Total annual net generation — used to validate capacity vs. output relationship** |
| `YEAR` | int | Reporting year (2015–2024) |

---

## `average_electricity_rates.csv`
**Rows:** 50 (states) | Average electricity rates in cents per kWh

Used to assess whether states with older, lower-capacity fleets face higher electricity costs — and whether modernizing those fleets has economic implications for consumers.

| Column | Type | Description |
|---|---|---|
| `State` | str | U.S. state name |
| `Residential` | float | Avg residential rate (¢/kWh) |
| `Commercial` | float | Avg commercial rate (¢/kWh) |
| `Average` | float | Avg across both sectors (¢/kWh) |

---

## `average_electricity_bills.csv`
**Rows:** 49 (states) | Monthly average electricity bills in USD

Supplementary context — higher bills in states with aging fleets would strengthen the case for fleet modernization.

| Column | Type | Description |
|---|---|---|
| `State` | str | U.S. state name |
| `Residential` | float | Avg monthly residential bill ($) |
| `Commercial` | float | Avg monthly commercial bill ($) |
| `Average` | float | Avg across both sectors ($) |

---

## `windiest-states-in-the-us_-2025.csv`
**Rows:** 50 (states) | Wind resource data by state

Used as a control variable — states with stronger wind resources may naturally support larger turbines, so wind speed is included to isolate the effect of technology era from geography.

| Column | Type | Description |
|---|---|---|
| `state` | str | U.S. state name |
| `WindiestStatesAverageWindSpeedMPH` | float | Average wind speed (MPH) |
| `MeanWindSpeed328ft` | float | Mean wind speed at 100m elevation (mph) — closest to modern hub heights |
| `MeanWindPower328ft` | float | Mean wind power density at 100m (W/m²) |
| `MeanWindSpeed33ft` | float | Mean wind speed at 10m elevation (mph) |

---

## Key Columns for Analysis

⭐ = directly tied to the problem statement

| Column | Dataset | Role |
|---|---|---|
| `p_year` ⭐ | wind-turbines | Primary time axis — used to bin turbines by decade |
| `t_cap` ⭐ | wind-turbines | Primary outcome variable — capacity (kW) tracked across decades |
| `t_hh` ⭐ | wind-turbines | Physical dimension — hub height growth over time |
| `t_rd` ⭐ | wind-turbines | Physical dimension — rotor diameter growth over time |
| `t_rsa` ⭐ | wind-turbines | Physical dimension — rotor swept area (r=0.88 with capacity) |
| `t_manu` ⭐ | wind-turbines | Manufacturer — breakdown of who drove vs. lagged innovation |
| `t_state` | wind-turbines | Geographic grouping — fleet-age and capacity gap by state |
| `Net Generation (Megawatthours)` | wind-operators | Validates whether higher-spec fleets produce more output |
| `MeanWindSpeed328ft` / `MeanWindPower328ft` | windiest-states | Control variable — isolates technology era from wind resource |
| `Residential` (rates) | electricity-rates | Context — economic cost of operating an aging fleet |

---

## Engineered Features
*Planned for `02_Data_Cleaning.ipynb`*

| Feature | Source Columns | Description |
|---|---|---|
| `decade` | `p_year` | Installation decade, binned as 1980 / 1990 / 2000 / 2010 / 2020 |
| `capacity_tier` | `t_cap` | Size category: sub-MW (<1000 kW), mid (1000–1999 kW), large (2000–2999 kW), utility (3000+ kW) |
| `specific_power` | `t_cap` / `t_rsa` | Capacity divided by rotor swept area (W/m²) — proxy for design efficiency; a declining trend indicates rotors grew faster than capacity, reflecting optimization for lower wind speed sites |
| `fleet_age_index` | `p_year` (state avg) | State-level average installation year — used to rank states by fleet modernity |