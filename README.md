# Access to Exercise Opportunities and Health Outcomes

A county-level econometrics project investigating one question: **does access to
exercise opportunities reduce obesity rates, or is that relationship mostly
explained by something else, like income?**

I wanted this to be more than "fit a regression, report a number." It's really a
project about isolating a real effect from a confounded one, then checking
whether that effect holds up two different ways (in-sample inference and
out-of-sample prediction).

## The data

- **Source:** [County Health Rankings & Roadmaps 2025](https://www.countyhealthrankings.org/)
- **Size:** 3,053 U.S. counties (after cleaning, see note below)
- **Variables:** adult obesity rate, physical inactivity rate, access to exercise
  opportunities, food environment index, median household income, uninsured rate,
  child poverty rate, and percent rural

**A data-cleaning note worth flagging:** the raw file also contains 51 rows that
aren't counties at all. They're state-level aggregates (one per state + DC),
identifiable because their FIPS code ends in `000`. I check for and remove these
before any analysis, since mixing a state's summary number in with individual
counties would quietly distort the results.

## What I did

**Exploratory plots.** Physical inactivity vs. obesity (a sanity check that
confirms the data behaves as expected), exercise access vs. obesity (motivates the
rest of the project, since the relationship is real but noisy, raising the
question of omitted-variable bias), and exercise access vs. physical inactivity
(access alone doesn't guarantee people actually use it).

**Omitted-variable bias analysis.** I build three nested OLS models, all with
heteroskedasticity-robust (HC3) standard errors:
- Model 1: obesity ~ exercise access (naive baseline)
- Model 2: + median household income (the main suspected confounder)
- Model 3: + food environment, insurance, poverty, and rurality (the rest of the controls)

Watching how the exercise-access coefficient changes as each control is added is
the core of the omitted-variable-bias story.

**Robustness checks.** A Variance Inflation Factor (VIF) table to actually
quantify the multicollinearity warning statsmodels flags in Model 3, and a
side-by-side regression table comparing all three models at once.

**Heterogeneity.** Splitting counties into high- and low-income groups to see
whether exercise access matters equally across income levels.

**Geographic visualization.** A choropleth map of obesity rates across all
contiguous U.S. counties.

**Predictive modeling.** An 80/20 train/test split, comparing out-of-sample R²
across the same three-model progression, plus a logistic regression classifying
counties as high- or low-obesity.

## Key findings

**Income is the dominant confounder.** The naive model says a full move from no
exercise access to universal access is associated with a 7.6-percentage-point
drop in obesity (R² = 0.14). Add income, and that coefficient collapses by about
64% (to -0.028) while R² jumps to 0.40, meaning income alone explains most of the
variation that exercise access appeared to "explain" in the naive model. Adding
the remaining controls barely moves R² further (0.40 to 0.41), and VIF confirms
this isn't a multicollinearity artifact: every variable comes in under a VIF of 5.
The exercise-access effect survives, smaller but statistically significant
(p < 0.01) in every single model.

**The effect is roughly twice as strong in wealthier counties.** Splitting by
income, exercise access is associated with a noticeably larger obesity reduction
in high-income counties than low-income ones, plausibly because access alone
isn't enough: people also need the time, money, and flexibility to use it. (Worth
noting: this comparison uses the baseline, no-controls specification, the same
one shown above to be confounded, so some of this gap could reflect other
income-related factors rather than exercise access behaving differently on its
own.)

**The result holds out-of-sample, not just in-sample.** A linear regression
predicting obesity in unseen counties gets R² = 0.385 once income is included, and
almost all of that predictive power comes from income alone (R² jumps from 0.11
to 0.375 just by adding it). A logistic classifier separating high- vs.
low-obesity counties gets about 69% accuracy.

## Tools

Python, pandas, NumPy, statsmodels, scikit-learn, geopandas, matplotlib, seaborn.

Techniques: OLS regression with robust (HC3) standard errors, omitted-variable-bias
analysis, Variance Inflation Factor, train/test split, linear and logistic
regression, confusion matrices, choropleth mapping.

## Running it

```bash
git clone https://github.com/RachelNefesh/county-health-exercise-analysis.git
cd county-health-exercise-analysis
pip install -r requirements.txt
jupyter notebook county_health_exercise_analysis.ipynb
```

You'll also need the County Health Rankings 2025 analytic data CSV
(`analytic_data2025_v3.csv`), downloaded from the link above and placed in the
project folder. The notebook also pulls a Census county-boundary shapefile
automatically for the choropleth map.

Run it top to bottom (**Kernel → Restart & Run All**) to reproduce everything.

## What's in this repo

| File | What it is |
|------|------------|
| `county_health_exercise_analysis.ipynb` | The full analysis, with code, charts, and write-up of each step |
| `requirements.txt` | Python packages needed to run it |
| `README.md` | This file |
