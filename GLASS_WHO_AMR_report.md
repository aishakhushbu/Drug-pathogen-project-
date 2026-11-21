## Global Antimicrobial Resistance in Bloodstream Acinetobacter spp.: A WHO 2023 Data Analysis

## Project Overview

# Antimicrobial resistance (AMR) is a major challenge in the treatment of bacterial infections. This project explores reported antimicrobial resistance in bloodstream Acinetobacter spp. using the World Health Organization's 2023 surveillance data.

The analysis examines resistance patterns across eight antibiotics, 85 countries and territories, and six WHO regions. It uses Python-based data analysis to investigate differences in resistance percentages, geographic variation, and the distribution of antimicrobial susceptibility testing observations.

## Research Objectives

- Examine reported resistance patterns across eight antibiotics.
- Compare pooled resistance percentages between antibiotics.
- Explore regional differences in bloodstream Acinetobacter resistance.
- Assess differences in surveillance coverage across countries and WHO regions.
- Explore associations between antibiotic categories, WHO regions, and resistance using binomial regression.

# Dataset

Source: WHO antimicrobial resistance surveillance data, 2023.

Dataset file: "Resistance to antibiotics in 2023_All-Bloodstream.csv"

The analysis focuses on:

- Specimen: Bloodstream
- Pathogen: Acinetobacter spp.
- Geographic coverage: 85 countries and territories
- WHO geographic coverage: Six regions
- Antibiotics: Eight

# Main Variables

Variable| Description
Specimen| Clinical specimen category
PathogenName| Bacterial pathogen
AntibioticName| Antibiotic tested
CountryTerritoryArea| Reporting country or territory
WHORegionName| WHO geographic region
InterpretableAST| Number of interpretable antimicrobial susceptibility tests
Resistant| Number of resistant test results
ResistancePercentage| Reported resistance percentage

# Methods and Analytical Workflow

## 1. Data loading and inspection

- Imported the CSV dataset using Pandas.
- Inspected dataset dimensions, data types, and descriptive statistics.
- Examined missing values and duplicate records.

## 2. Data quality assessment

- Checked missing values across all variables.
- Examined exact duplicate records.
- Investigated country-level and antibiotic-level observation coverage.

## 3. Antibiotic-level analysis

- Calculated mean, median, and standard deviation of reported resistance percentages.
- Aggregated interpretable susceptibility tests and resistant results.
- Calculated pooled resistance percentages for each antibiotic.

## 4. Geographic analysis

- Compared resistance across six WHO regions.
- Calculated region-specific pooled resistance percentages.
- Constructed antibiotic-by-region comparisons.
- Examined country-level differences and surveillance coverage.

$# 5. Statistical analysis

- Applied binomial generalized linear modeling (GLM) to examine associations between resistance, antibiotic categories, and WHO regions.
- Used country-clustered robust standard errors to account for within-country clustering.
- Calculated odds ratios and confidence intervals.
- Visualized model estimates using a forest plot.

## Key Findings

## Pooled resistance by antibiotic

Antibiotic| Pooled resistance (%)
Imipenem| 65.64
Meropenem| 58.88
Doripenem| 57.95
Gentamicin| 54.83
Amikacin| 53.90
Minocycline| 18.62
Tigecycline| 10.53
Colistin| 3.77

## Regional variation

The highest pooled regional resistance percentage was observed in the African Region (64.80%), followed by the European Region (62.99%).

The Western Pacific Region had the lowest pooled regional percentage (27.71%), followed by the Region of the Americas (28.57%).

These comparisons describe the available surveillance observations and should not be interpreted as population-wide estimates.

## Limitations

- Country and antibiotic observation counts are uneven.
- Some antibiotic-region combinations have limited observations or are unavailable.
- Surveillance coverage differs substantially between countries.
- Resistance percentages can be influenced by differences in the number of interpretable susceptibility tests.
- The binomial model showed substantial overdispersion, requiring caution when interpreting statistical estimates.
- The observational analysis does not establish causal relationships.

## Tools and Technologies

- Python
- Google Colab
- Pandas
- NumPy
- Statsmodels
- Matplotlib

# Repository Contents

- "whoamr_data.ipynb" — Jupyter notebook containing data inspection, processing, descriptive analysis, geographic comparisons, and statistical modeling.

# Reproducibility

1. Open the Jupyter notebook in Google Colab.
2. Upload the WHO 2023 bloodstream resistance CSV file when prompted.
3. Execute the notebook cells sequentially.
4. Review the descriptive statistics, regional comparisons, and regression outputs.

# Project Status

Exploratory data analysis and statistical modeling completed. Further work could include sensitivity analyses, improved assessment of overdispersion, and additional evaluation of surveillance coverage.

---

This is an independent, exploratory data-analysis project using secondary surveillance data. It is not a clinical study and does not provide treatment recommendations.
