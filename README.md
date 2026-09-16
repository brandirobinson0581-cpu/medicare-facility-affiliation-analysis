# Medicare Facility Affiliation Network Analysis

## Published project

- [Read the published article](https://brandirobinson0581-cpu.github.io/medicare-facility-affiliation-analysis/)
- [View the public GitHub repository](https://github.com/brandirobinson0581-cpu/medicare-facility-affiliation-analysis)

## Project motivation

This project explores how individual healthcare providers are connected to Medicare-participating facilities. The goal is to understand which facility settings dominate the data, how broad provider affiliation networks are, and whether a transparent model can identify providers whose affiliations cross more than one type of care setting.

The analysis answers four questions:

1. Which facility settings account for the most affiliation records?
2. How broad are individual providers' facility networks?
3. Which primary facility settings are most associated with multi-facility providers?
4. How accurately can a model identify cross-setting providers, and what does it predict in a realistic planning scenario?

## Repository contents

| File or folder | Description |
|---|---|
| `facility_affiliation_analysis.ipynb` | Executed Jupyter notebook containing the complete CRISP-DM analysis, model, evaluation, and scenario prediction. |
| `blog_post.md` | A 200–500 word, nontechnical article summarizing the findings. |
| `blog_post.html` | A browser-ready version of the article with its charts embedded for reliable publishing. |
| `index.html` | The GitHub Pages entry point for the published article. |
| `data/README.md` | Instructions and the official CMS link for obtaining the public source data. |
| `requirements.txt` | Python libraries required to reproduce the analysis. |

## Libraries

- pandas and NumPy for data preparation and analysis.
- matplotlib and seaborn for visualizations.
- scikit-learn for preprocessing, logistic regression, and model evaluation.
- Jupyter Notebook for the documented analysis.

## CRISP-DM process

1. **Business understanding:** Define a network-planning question that can be answered with the available affiliation data.
2. **Data understanding:** Inspect the file structure, facility mix, missing values, duplicates, and provider-level distributions.
3. **Data preparation:** Preserve identifiers as strings, aggregate records to one row per NPI, create documented features, and encode the categorical facility setting.
   Facility-size features count providers in the training partition only. Facilities absent from that partition are treated as missing and imputed using training medians.

4. **Modeling:** Fit a class-weighted logistic regression to classify cross-setting providers.
5. **Evaluation:** Compare the model with the majority-class baseline and report accuracy, precision, recall, F1, ROC AUC, coefficients, and a confusion matrix.
6. **Deployment:** Use predictions only to prioritize human review, with calibration, access controls, data-freshness checks, and drift monitoring.

## Main results

- The file contains 2,254,034 affiliation records representing 940,577 unique NPIs and 40,965 unique nonmissing facility certification numbers.
- Hospitals account for 1,913,975 records, so raw national totals are heavily influenced by one setting.
- The median provider has two facility affiliations; 56.0% have more than one facility affiliation, while 16.1% cross more than one facility type.
- Multi-facility rates vary sharply by primary setting, from 9.0% for long-term care hospital-centered providers to 97.7% for dialysis-centered providers.
- On the held-out test set, the model achieves 85.6% accuracy, 53.3% precision, 82.9% recall, a 64.9% F1 score, and 0.948 ROC AUC. Accuracy is only 1.7 percentage points above the 83.9% majority-class baseline, which is why the additional metrics and limitations are necessary.

## Running the notebook

1. Use Python 3.10 or newer.
2. Create and activate a virtual environment.
3. Install the dependencies with `pip install -r requirements.txt`.
4. Start Jupyter with `jupyter notebook`.
5. Open `facility_affiliation_analysis.ipynb` and run all cells from top to bottom.

The compressed source snapshot is included as `data/Facility_Affiliation.csv.gz` and is read directly by the notebook. The official CMS source is linked in `data/README.md`.

## Limitations

The source is an administrative affiliation snapshot. It does not measure care quality, patient outcomes, referral volume, or the strength and timing of an affiliation. Model coefficients are associations, not causal effects. Because the complete affiliation records already reveal whether a provider crosses settings, the model is only an illustrative screening exercise for a situation in which summarized profile data arrives before the setting details. It is not a future-behavior forecast and should not replace direct calculation when complete records are available.

## Acknowledgments

The source data comes from the Centers for Medicare & Medicaid Services [Facility Affiliation Data](https://data.cms.gov/provider-data/dataset/27ea-46a8), accessed September 12, 2026. The project uses open-source Python libraries listed in `requirements.txt`.
