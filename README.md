# Effects of Recreational Marijuana Legalization on Alcohol Consumption in California

This repository contains the data and R code for my Econ 109E research paper, which asks:

> **Did legalizing recreational marijuana change alcohol consumption in California?**

California's Proposition 64 (passed November 2016) legalized recreational marijuana, and retail sales began on **January 1, 2018**. Using a **difference-in-differences (DiD)** design, I compare per-capita alcohol consumption in California (treatment group) with the average of 18 states that did not legalize recreational marijuana at any point from 2001–2022 (control group), before and after 2018.

## Key findings

| Result | Value |
|---|---|
| DiD estimate (California × Post) | **+0.306** gallons of ethanol per capita (SE 0.035, clustered by state) |
| R² (with state and year fixed effects) | 0.935 |
| CA vs. controls, pre-2018 | ~3% higher |
| CA vs. controls, post-2018 | ~16% higher |

* **No anticipation:** the event study shows pre-2018 effects close to zero, then a clear jump starting in 2018.
* **Parallel trends:** California and the control average move together from 2001–2017.
* **Robustness:** the estimate stays between about 0.29 and 0.32 when any single control state is dropped. Excluding the COVID years (2020–2022) lowers it to about 0.24, which is still clearly positive.

Together these point to legalization being associated with *higher* alcohol consumption in California, meaning the two acted as complements rather than substitutes. Because California is the only treated state, the results generalize best to places that resemble California.

## Repository contents

| File | Description |
|---|---|
| `Marijuana_and_Alchohol_V2.Rmd` | R Markdown file that produces every table and figure in the paper |
| `Alcohol Consumption Data.xlsx` | Annual per-capita alcohol consumption data, 2001–2022 |
| `Econ_109E__Research_Paper.pdf` | The full research paper |

## Data

* **Source:** National Institute on Alcohol Abuse and Alcoholism (NIAAA), [*Surveillance Report #120*](https://www.niaaa.nih.gov/sites/default/files/surveillance-report120.pdf) (April 2023), based on the Alcohol Epidemiologic Data System (AEDS).
* **Measure:** apparent per-capita alcohol consumption, in **gallons of ethanol per person aged 14+**. This number comes from alcohol sales data, converted to ethanol with fixed coefficients (beer 0.045, wine 0.129, spirits 0.411).
* **Period:** 2001–2022, annual.
* **Treatment state:** California (CA).
* **Control states (18):** AL, AR, GA, FL, ID, IN, IA, KS, KY, LA, MS, NE, NC, SC, TN, TX, WI, WY.

The report is only available as a PDF, so the values were entered into Excel by hand. Each state's figures are in that state's section of the report. The workbook has two sheets:

| Sheet | Columns | Used for |
|---|---|---|
| `Graph 1` | `Year`, `CA`, `ControlledStateAverage` | Trend and counterfactual plots; CA rows of the panel |
| `Graph 2` | `Year`, `CA`, one column per control state, `Average` (sum of the 18 controls ÷ 18) | Control-state rows of the panel; robustness check |

## Methodology

The DiD regression with two-way fixed effects is:

$$Y_{st} = \alpha + \beta\,(CA_s \times Post_t) + \gamma_s + \delta_t + \varepsilon_{st}$$

* $Y_{st}$: alcohol consumption in state *s*, year *t*
* $CA_s$: 1 for California, 0 for the control states
* $Post_t$: 1 for 2018 and later
* $\gamma_s$, $\delta_t$: state and year fixed effects
* $\beta$: the treatment effect (ATT)

In R (`fixest`), this is `feols(y ~ did | state + year, cluster = "state")`.

## What the code does

The `.Rmd` is organized into sections that follow the paper in order:

| Section | Output | Paper location |
|---|---|---|
| 0. Setup | Loads the packages and sets the data file name | — |
| 1. Data preparation | Builds `df_trend` (wide: CA vs. control average by year) and `df_panel` (long: one row per state-year, with `treated`, `post`, `did`, `treat_year`) | — |
| 2. Summary statistics | **Table 1**: mean, SD and N of consumption for California vs. the control states | §3.2 |
| 3. Event study | **Figure**: Sun & Abraham (`sunab`) event study with 2017 as the reference year, used to test no anticipation | §4.1 |
| 4a. Trend comparison | **Figure**: California vs. control-state average, 2001–2022 | §4.2 |
| 4b. Counterfactual | **Figure**: control average shifted by the mean pre-2018 gap, showing where CA would be without legalization | §4.2 |
| 5. DiD estimate | **Table 2**: main DiD coefficient with state-clustered SEs | §4.3 |
| 6. Robustness | **Figure**: DiD estimate and 95% CI for the baseline, excluding 2020-2022, and dropping each control state one at a time | §4.4 |

## How to run

1. Put `Marijuana_and_Alchohol_V2.Rmd` and `Alcohol Consumption Data.xlsx` in the **same folder**. The code looks for the data file by that exact name. If your copy is named differently (for example `Alcohol_Consumption_Data.xlsx`), rename the file or update `data_file` in the Setup chunk.
2. Open the `.Rmd` in RStudio.
3. Install any missing packages by running once in the console:
   ```r
   install.packages(c("readxl", "ggplot2", "tidyr", "dplyr", "fixest",
                      "modelsummary", "gt", "broom", "purrr"))
   ```
4. Click **Run All**, or **Knit** to produce a PDF. Knitting to PDF requires a LaTeX installation, such as `tinytex::install_tinytex()`.

## Limitations

* California is the only treated state, which limits external validity.
* There is no federal data on illegal marijuana use. The analysis assumes illegal access has no effect on alcohol consumption.
* Other policy changes around 2018 (such as tax changes) could confound the estimate.
