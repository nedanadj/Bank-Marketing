# Comparing Classifiers: Bank Marketing Term Deposit Prediction

## What this is

A Portuguese bank ran 17 phone campaigns between 2008 and 2010 trying to sell a term deposit product. Only about 11% of calls actually converted. This project compares four classifiers - Logistic Regression, KNN, Decision Tree, and SVM - to predict ahead of time whether a client is likely to say yes, so the bank can prioritize calls instead of dialing everyone.

Full writeup: [`prompt_III_solution.ipynb`](./prompt_III_solution.ipynb)

## Data

- Source: [UCI Bank Marketing dataset](https://archive.ics.uci.edu/ml/datasets/bank+marketing), from Moro, Cortez & Rita (2014).
- `bank-additional-full.csv` - all 41,188 contacts, used for exploring the data.
- `bank-additional.csv` - a 4,119-row sample, used for actually fitting/tuning models (the dataset docs recommend this smaller file for anything computationally heavier, like SVM).

## What I did

1. Checked the data for missing values, duplicates, and weird sentinel values (`pdays = 999` means "never contacted before").
2. Built a baseline model using just demographic/bank-client features (age, job, marital, education, default, housing, loan) and compared it to a majority-class baseline (~88.7%).
3. Fit all four classifiers with default settings on that same limited feature set to see where they stood.
4. Added campaign-timing and macroeconomic features (dropping `duration`, which isn't known until after a call happens and would leak the answer), then ran grid search on all four models using ROC-AUC as the tuning metric.
5. Looked at logistic regression's coefficients to see what's actually driving predictions.

## What I found

- Demographics alone barely beat guessing "no" for everyone. Adding campaign/economic context made a real difference.
- Whether someone said yes to a previous campaign is one of the strongest predictors in the dataset.
- Economic conditions at the time of the campaign matter a lot - see the notebook for the specific indicators.
- Getting called more times during the same campaign doesn't seem to help conversion.
- Best tuned model (logistic regression, ~0.78 test ROC-AUC) is meaningfully better than random at ranking who's likely to subscribe.

## Recommendations

Full list is at the end of the notebook, but the short version: pilot this with a ranked call list before trusting it fully, retrain periodically since economic conditions shift, never let `duration` into a deployed version of the model, and set a calling threshold based on actual cost/benefit instead of the default 0.5 cutoff.

## Files

```
.
├── README.md
├── prompt_III_solution.ipynb
├── CRISP-DM-BANK.pdf
└── data/
    └── bank-additional/
        ├── bank-additional-full.csv
        ├── bank-additional.csv
        └── bank-additional-names.txt
```

## Citation

Moro, S., Cortez, P., & Rita, P. (2014). A data-driven approach to predict the success of bank telemarketing. *Decision Support Systems*.
