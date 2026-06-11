# county-health-exercise-analysis
OLS regression analysis of exercise access and obesity rates across U.S. counties
# Exercise Access and Obesity Rates Across U.S. Counties

## Overview
This project analyzes the relationship between access to exercise 
opportunities and adult obesity rates across 3,100+ U.S. counties 
using 2025 County Health Rankings data.

## Research Question
Does access to exercise opportunities reduce obesity rates, and does 
this effect differ by income level?

## Methods
- Data cleaning and exploratory analysis in Python and Pandas
- Three OLS regression models with robust standard errors to 
  identify and control for omitted variable bias
- Coefficient stability analysis across models
- Geographic visualization using GeoPandas choropleth map
- Predictive modeling with scikit-learn train/test split

## Key Findings
- Exercise access is negatively associated with obesity rates 
  even after controlling for income, poverty, food environment, 
  and rurality
- Income is the dominant confounder — adding it reduced the 
  exercise access coefficient by 64%
- The effect of exercise access on obesity is more than twice 
  as strong in high income counties versus low income counties, 
  suggesting structural barriers limit utilization in poorer areas

## Tools Used
Python, Pandas, NumPy, statsmodels, scikit-learn, GeoPandas, 
Matplotlib, Seaborn
