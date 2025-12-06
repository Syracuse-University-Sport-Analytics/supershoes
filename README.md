# Supershoe Effects on Elite Marathon Performance

An empirical analysis of how advanced footwear technology (AFT) has impacted elite marathon times across gender, examining World Athletics' top-100 annual performances from 2010–2024.

## Overview

This project investigates the effects of "supershoes"—running shoes featuring carbon-fiber plates and advanced foam technology—on elite marathon performance. The Nike Vaporfly 4% debuted at the 2016 Olympic Marathon, marking the beginning of what we define as the *supershoe era* (post-2016.72). Using fixed effects and mixed effects regression models, we estimate time-progression effects and examine whether these gains differ by gender or competitive rank.

## Authors

- Brandon Parikh
- Justin Ehrlich
- Dan Griffiths
- Shane Sanders

## Key Findings

- **Pre-supershoe stagnation**: From 2010–2016, elite marathon times were *increasing* by approximately 10 seconds per year
- **Supershoe-era gains**: Post-2016, times dropped significantly—approximately 35.6–35.8 seconds per year for both men and women
- **Gender effect**: Women showed slightly larger time improvements than men in the supershoe era
- **Competitive balance**: Effects are broadly democratic across the top-100 distribution, lowering expected times equally regardless of rank

## Repository Structure

```
├── analysis.Rmd          # Main analysis script (R Markdown)
├── data/
│   ├── mens_data.csv         # Men's marathon data
│   ├── womens_data.csv       # Women's marathon data
│   ├── new_top100.csv        # Men's top-100 annual performances
│   └── top100_women.csv      # Women's top-100 annual performances
├── visualizations/       # Output directory for figures
│   ├── exploratory_plots.pdf
│   ├── interaction_fe_model.pdf
│   └── interaction_mixed_model.pdf
├── tables/               # Output directory for regression tables
│   ├── interaction_fe_model.html
│   ├── interaction_fe_model_comparisons.html
│   ├── interaction_fe_quadratic_model.html
│   ├── interaction_fe_quartile_model.html
│   └── interaction_mixed_model_comparisons.html
└── README.md
```

## Data

The analysis uses World Athletics top-100 annual marathon performances for men and women spanning 2010–2024. Key variables include:

| Variable | Description |
|----------|-------------|
| `time_s` | Finishing time in seconds |
| `frac_year` | Continuous year (accounting for race date) |
| `supershoe_era` | Binary indicator (1 if post-2016.72) |
| `category` | Gender (Men/Women) |
| `athlete` | Athlete identifier (for fixed/random effects) |
| `venue` | Race venue |
| `after_2020` | Binary indicator for post-pandemic period |

**Note**: 2020 is excluded from analysis due to COVID-19 disruptions to the racing calendar.

## Methods

The analysis employs several regression specifications:

1. **Fixed Effects Model**: Controls for athlete and venue heterogeneity
2. **Interaction Model**: Allows supershoe effects to vary by gender
3. **Mixed Effects Model**: Treats athletes as random effects for more efficient estimation
4. **Quartile Models**: Tests whether supershoe effects differ across the performance distribution

Core model specification:
```
time_s ~ frac_year + supershoe_era + frac_year:supershoe_era + 
         frac_year:supershoe_era:category + category + after_2020 + 
         venue + athlete
```

## Requirements

```r
# Core packages
library(tidyverse)
library(ggplot2)
library(ggridges)
library(lubridate)

# Modeling
library(lme4)
library(stargazer)
library(ggeffects)

# Visualization
library(patchwork)
```

## Usage

1. Clone the repository
2. Ensure data files are in the `data/` directory
3. Open `analysis.Rmd` in RStudio
4. Knit the document or run chunks sequentially

```r
# To render the full analysis:
rmarkdown::render("analysis.Rmd")
```

## Visualizations

The analysis produces several key figures:

- **Hexbin plots**: Standardized time distributions over the study period by gender
- **Scatter plots with trend lines**: Time progression across pre-supershoe, supershoe, and post-2020 eras
- **Ridge plots**: Year-over-year time distributions
- **Marginal effects plots**: Predicted times from interaction models

## References

1. World Athletics regulations on shoe technology (effective January 1, 2022)
2. Nike Vaporfly and subsequent AFT developments (2016–present)


## Citation

If you use this analysis in your work, please cite:

```
Parikh, B., Ehrlich, J.,  Griffiths, D., & Sanders, S. (2025). Supershoe Effects on Elite Marathon Performance [Source code]. GitHub. https://github.com/Syracuse-University-Sport-Analytics/supershoes
```
