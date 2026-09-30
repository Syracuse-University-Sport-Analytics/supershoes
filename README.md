# Supershoes, Marathon Performance, and Stride Biomechanics

This project examines how advanced footwear technology (AFT), particularly carbon-plated "supershoes," has changed elite marathon performance. We combine time-series modeling of World Athletics' top-100 annual marathon performances with computer-vision analysis of elite runners' stride mechanics to study both the performance effects of supershoes and the biomechanical changes that may help explain them.

The project was developed for the 2027 MIT Sloan Sports Analytics Conference under the title *Beyond Sub-2: Supershoes in the Wake of a Marathoning Paradigm Shift. Stride Biomechanics, Performance Trends, Gender/Competitive Balance Assessed via Deep Learning & Time-Series Modelling.*

## Overview

Advanced footwear technology has become a major part of elite distance running over the past decade. The Nike Vaporfly 4% debuted at the 2016 Olympic Marathon, and later generations of supershoes added combinations of carbon-fiber plates, lightweight foams, and increasingly refined shoe geometries. In this project, late 2016 (`2016.72`) marks the beginning of the supershoe era.

The central question is whether the recent jump in elite marathon performance is simply part of the sport's long-run progression or represents a genuine break associated with footwear technology. We approach that question from two directions. First, we estimate changes in marathon-time trends across the pre-supershoe and supershoe eras. Second, we use OpenPose to study changes in stride mechanics from race footage, with particular attention to ground-contact time and off-ground acceleration.

The updated analysis covers men's and women's elite marathon performances from 2010 through 2025 and also asks whether the estimated benefits differ by gender or by position within the annual top-100 performance distribution.

## Authors

- Brandon Parikh
- Justin Ehrlich
- Dylan Griffiths
- Shane Sanders

## Main Findings

- **The pre-supershoe period was stagnant to slightly regressive.** From 2010 through 2016, the average sampled runner added about 10.1 seconds per year to marathon time.
- **Performance improved sharply after the introduction of supershoes.** From 2017 through 2025, estimated marathon times fell by about 35.57 seconds per year for men and 35.76 seconds per year for women.
- **Women show a slightly larger improvement.** The estimated supershoe-era slope is modestly steeper for women, and the gender difference is statistically significant in the time-series models.
- **The gains are broadly distributed across elite runners.** Within each gender, the estimated effect is approximately constant across the annual top-100 rankings rather than being concentrated among only the very fastest athletes.
- **Stride mechanics also change in the supershoe era.** OpenPose estimates from marathon footage of Edna Kiplagat and Eliud Kipchoge show meaningful pre/post differences in stride cycles, including the expected tradeoff between longer ground-contact time and greater off-ground acceleration.
- **The micro- and macro-level results point in the same direction.** Kiplagat shows a larger proportional increase in bounce progression and a smaller proportional increase in ground-contact time than Kipchoge, which is consistent with the somewhat larger time-series gains estimated for women.

## Repository Structure

```text
├── analysis.Rmd              # Main marathon time-series analysis
├── inspo_code.Rmd            # World-record speed-distance visualizations
├── data/
│   ├── mens_data.csv         # Men's marathon data
│   ├── womens_data.csv       # Women's marathon data
│   ├── new_top100.csv        # Men's top-100 annual performances
│   └── top100_women.csv      # Women's top-100 annual performances
├── visualizations/           # Output directory for figures
│   ├── exploratory_plots.pdf
│   ├── interaction_fe_model.pdf
│   ├── interaction_mixed_model.pdf
│   ├── mens_world_records_current.png
│   ├── mens_world_records_old.png
│   ├── womens_world_records_current.png
│   └── womens_world_records_old.png
├── tables/                   # Output directory for regression tables
│   ├── interaction_fe_model.html
│   ├── interaction_fe_model_comparisons.html
│   ├── interaction_fe_quadratic_model.html
│   ├── interaction_fe_quartile_model.html
│   └── interaction_mixed_model_comparisons.html
└── README.md
```

## Data

The time-series portion of the project uses World Athletics' top-100 annual marathon performances for men and women from 2010 through 2025. Late 2016 (`2016.72`) is used as the beginning of the supershoe era. The analysis is designed to estimate both the overall shift in marathon-time progression and whether that shift differs by gender or competitive rank.

Key variables in the existing analysis include:

| Variable | Description |
|----------|-------------|
| `time_s` | Finishing time in seconds |
| `frac_year` | Continuous year based on race date |
| `supershoe_era` | Indicator for the post-2016.72 supershoe era |
| `category` | Gender category (Men/Women) |
| `athlete` | Athlete identifier used in fixed/random effects |
| `venue` | Race venue |
| `after_2020` | Indicator for the post-pandemic period |

The analysis excludes 2020 because of the major disruption to the international racing calendar.

## Methods

The project combines two complementary approaches.

### 1. Marathon Time-Series Models

We use fixed-effects and mixed-effects regression models to estimate how elite marathon times changed before and after the introduction of supershoes. The models allow the supershoe-era time trend to differ by gender and are also extended across the annual performance distribution to test for competitive-balance effects.

The main specifications include:

1. **Fixed-effects models** controlling for athlete and venue heterogeneity.
2. **Gender-interaction models** allowing the supershoe-era progression to differ between men and women.
3. **Mixed-effects models** treating athletes as random effects.
4. **Quartile/rank models** testing whether the estimated effect varies across the top-100 performance distribution.

A representative model specification is:

```r
time_s ~ frac_year + supershoe_era + frac_year:supershoe_era +
         frac_year:supershoe_era:category + category + after_2020 +
         venue + athlete
```

### 2. OpenPose Stride Analysis

The biomechanics portion uses the OpenPose deep-learning framework to estimate human pose from marathon broadcast footage. OpenPose applies a multi-stage convolutional neural network to identify anatomical landmarks frame by frame. Confidence maps are used to locate joints, while Part Affinity Fields capture the directional relationships among those joints and allow the model to reconstruct coherent body poses.

This provides a form of markerless motion capture from natural race footage without requiring laboratory sensors or physical markers. We apply the approach to pre- and post-supershoe footage of Edna Kiplagat and Eliud Kipchoge and compare stride-cycle patterns, ground-contact time, and vector-based measures of off-ground acceleration.

The purpose of combining the two approaches is straightforward: the time-series models estimate whether marathon performance changed at the population level, while the OpenPose analysis provides a biomechanical look at how running mechanics may have changed at the athlete level.

## Results in Context

The analysis is motivated in part by the continued movement of elite marathon performance toward previously unusual thresholds. In 2026, Sabastian Sawe and Yomif Kejelcha recorded the first sub-2-hour marathon race times in the study framing, while Fotyen Tesfay and Tigst Assefa posted top-three women's marathon times. The broader question is whether results of this kind reflect ordinary long-run progression or a technology-driven break from the earlier trend.

Our estimates support a clear break around the beginning of the supershoe era. Before supershoes, marathon times in the sample were not steadily improving. After their introduction, both men's and women's times improved at a much faster rate. At the same time, the rank-based results suggest that these gains were not limited to the very top of the distribution.

The stride analysis gives the aggregate results a possible biomechanical foundation. The post-supershoe footage shows changes consistent with the proposed tradeoff between increased ground-contact time and greater acceleration away from the ground. The patterns are not identical for Kiplagat and Kipchoge, which is also consistent with the small gender difference observed in the time-series results.

## World Record Speed-Distance Analysis

`inspo_code.Rmd` places marathon performance in the broader context of running world records from 100 meters through the marathon. It compares current records with pre-supershoe records from September 25, 2016.

A logarithmic model,

```r
speed ~ log(distance)
```

captures the expected decline in sustainable speed as race distance increases. Highlighting the marathon within that relationship provides another way to assess whether marathon records have moved unusually far relative to shorter-distance events during the supershoe era.

**Outputs:**

- `mens_world_records_current.png` / `mens_world_records_old.png`
- `womens_world_records_current.png` / `womens_world_records_old.png`

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

The OpenPose portion of the project also requires the computer-vision/deep-learning environment used for the stride analysis.

## Usage

1. Clone the repository.
2. Place the required data files in the `data/` directory.
3. Open `analysis.Rmd` in RStudio.
4. Knit the document or run the chunks sequentially.

```r
rmarkdown::render("analysis.Rmd")
```

## Visualizations

The repository produces several types of figures, including:

- **Hexbin plots** showing standardized marathon-time distributions over the study period by gender.
- **Time-trend plots** comparing the pre-supershoe and supershoe eras.
- **Ridge plots** showing year-to-year changes in the performance distribution.
- **Marginal-effects plots** showing predicted marathon times from the interaction models.
- **OpenPose stride-cycle plots** comparing pre- and post-supershoe biomechanics.
- **Ground-contact-time and acceleration plots** summarizing the main stride-mechanics differences.

## References

1. Joubert, D. P., Dominy, T. A., & Burns, G. T. (2023). Effects of highly cushioned and resilient racing shoes on running economy at slower running speeds. *International Journal of Sports Physiology and Performance, 18*(2), 164-170.
2. World Athletics. *Athletic Shoe Regulations, Book C.*

## License

See [LICENSE](LICENSE) for details.

## Citation

```text
Parikh, B., Ehrlich, J., Griffiths, D., & Sanders, S. Supershoes, marathon performance, and stride biomechanics [Source code]. GitHub. https://github.com/Syracuse-University-Sport-Analytics/supershoes
```
