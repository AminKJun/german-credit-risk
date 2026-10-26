# german-credit-risk
# Credit risk scoring on the German Credit data

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AminKJun/german-credit-risk/blob/main/credit_risk_analysis.ipynb)

A lender approves a loan for someone who then defaults, or turns away someone who would have repaid. The first mistake costs far more than the second. This project builds three credit-risk models and then chooses the approval cut-off from the business cost of each mistake, not the default 0.5.

## Results (200 held-back test applicants, 60 of them bad credits)

| Policy | Approval rate | Bad rate among approved | Cost per applicant |
|---|---|---|---|
| Approve everyone | 100% | 30.0% | 1.500 |
| Model, default cut-off 0.50 | 84.5% | 21.9% | 0.965 |
| Model, cost-based cut-off 0.24 | 41.5% | 9.6% | 0.525 |

* Cost uses the matrix from the UCI documentation: approving a bad applicant costs 5, rejecting a good one costs 1.
* The cut-off of 0.24 was chosen on training data only, using out-of-fold predictions. The test set was used once, at the end.
* The three models are close (test AUC 0.795 random forest, 0.794 gradient boosting, 0.805 logistic regression, 95% interval for the random forest 0.722 to 0.864), so the cut-off mattered more than the model.
* Checking account status is by far the most influential input.

## What the notebook does

1. Loads and inspects the data.
2. Excludes two protected attributes (`personal_status_and_sex`, `foreign_worker`) from every model.
3. Compares logistic regression, random forest and gradient boosting with 5-fold cross-validation on the training set.
4. Chooses the approval cut-off that minimises expected cost.
5. Evaluates once on the test set, with a bootstrap interval on AUC.
6. Explains the model with permutation importance.
7. Lists what the results do and do not show.

## Run it

Click the Colab badge above, or locally:

```
pip install -r requirements.txt
jupyter notebook credit_risk_analysis.ipynb
```

No data file is needed. The notebook loads the copy of the dataset bundled with the `scorecardpy` package.

## Data and licence

Hofmann, H. (1994). Statlog (German Credit Data). UCI Machine Learning Repository. https://doi.org/10.24432/C5NC77. Licensed CC BY 4.0. The code in this repository is MIT licensed.

## Limitations

The data is small (1,000 applicants) and old, and UCI notes that its coding information contains errors. The 5 to 1 cost ratio is a convention of this dataset, not a real lender's economics. Excluding protected attributes does not remove proxies for them, and I did not measure error rates across groups. Full notes are at the end of the notebook.

## Author

Armeen Junaid
