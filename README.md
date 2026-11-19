# econ3916-lab17-classification
# Classification: When 90% Accurate Is Worthless

## Objective
I built a logistic regression classifier for loan defaults and showed why accuracy is a poor measure when defaults are rare, then chose a decision threshold from the cost of each kind of error instead of using 0.5.

## Methodology
- I simulated 6,000 loan applications with a default rate of 9.6%, using three features: log income, debt-to-income ratio, and the number of prior defaults.
- I kept 30% of the applications as a test set, with the default rate the same in both sets, and scored a rule that approves everyone as a baseline.
- I fitted a logistic regression on the training rows and compared its accuracy and confusion matrix at the usual 0.5 threshold with the baseline.
- I read the coefficients as changes in log-odds and as odds ratios, and showed how one odds ratio gives different changes in probability at different starting probabilities.
- I traced precision, recall and accuracy across six thresholds.
- I priced the two errors: approving a loan that defaults, and declining an applicant who would have repaid. I compared the closed-form threshold with one I found by searching thresholds on the training rows, using cross-validated probabilities, and then priced that threshold once on the test rows.
- I wrote a function, `precision_recall_at()`, that gives any classifier's precision and recall on any rows at any threshold.

## Key Findings
- A rule that approves everyone is 90.4% accurate. My logistic regression also scored 90.4% accuracy at the 0.5 threshold, because it predicted 0 defaults: it made the same decisions as the rule that ignores the data.
- The odds ratio for one more prior default is 1.63. This multiplies the odds of default, not the probability, so the change in probability depends on the applicant's starting risk.
- Lowering the threshold raised recall and lowered precision, while accuracy was highest at 0.5, where the model caught no defaults.
- The threshold that minimised expected cost was 0.121, close to the closed-form threshold of 0.118 and far below 0.5.
- At 0.121 the expected cost on the test rows was 944 per applicant, against 1,147 per applicant at 0.5.
- At the closed-form threshold, precision was 0.181 and recall was 0.477.
- The main lesson: when one class is rare, accuracy can make a model that catches nothing look good. The threshold should come from the cost of each error, not from 0.5.
