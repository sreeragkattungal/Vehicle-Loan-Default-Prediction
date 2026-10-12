# Vehicle-Loan-Default-Prediction

## Business Impact: Risk-Based Rejection Strategy

### Objective
Evaluate whether predicted probabilities of default (PD) can help identify 
high-risk loan applicants while reducing the default rate of the approved portfolio.

### Approach
- Used the best-performing XGBoost model to generate calibrated PDs using Platt scaling.
- Selected the high-risk cutoff using the 80th percentile of validation PDs.
- Froze the cutoff at approximately 30.17% PD.
- Applied the same cutoff to the held-out test set to evaluate the rejection strategy.
- Analyzed defaults captured, approvals lost, and the default rate among remaining approved loans.

### Held-Out Test Results

| Metric | Result |
|---|---:|
| Original test portfolio | 34,974 loans |
| Original default rate | 21.71% |
| High-risk PD cutoff | > 30.17% |
| Applicants rejected | 7,015 (20.06%) |
| Defaults among rejected applicants | 2,568 |
| Share of all defaults captured | 33.83% |
| Non-defaulters rejected | 4,447 |
| Remaining approved portfolio | 27,959 loans |
| Defaults in approved portfolio | 5,024 |
| Default rate after rejection | 17.97% |

## Model Performance

Three models were trained and compared: Logistic Regression, XGBoost, 
and a PyTorch Multilayer Perceptron (MLP).

### Held-Out Test Results

| Model | ROC-AUC | KS Statistic | Recall |
|---|---:|---:|---:|
| Logistic Regression | 0.6409 | 0.2055 | 65.00% |
| XGBoost | **0.6616** | **0.2344** | 67.06% |
| PyTorch MLP | 0.6529 | 0.2230 | 67.81% |

XGBoost achieved the highest ROC-AUC and KS statistic and was selected 
as the final model based on validation performance.

### Evaluation Metrics
- **ROC-AUC:** Measures how well the model ranks defaulters above non-defaulters.
- **KS Statistic:** Measures the maximum separation between the cumulative 
  score distributions of the two classes.
- **Recall:** Measures the proportion of actual defaulters identified 
  at the selected classification threshold.

Probability calibration was performed using Platt scaling to improve 
the alignment between predicted probabilities and observed default rates.

### Key Business Finding

Rejecting approximately 20% of applicants with the highest predicted 
probabilities of default captured 33.83% of observed defaults in the 
held-out test set. The observed default rate among the remaining approved 
applicants decreased from 21.71% to 17.97%.

This represents a reduction of approximately 3.74 percentage points 
in the approved portfolio's default rate.

### Interpretation

The rejection strategy concentrated a disproportionate share of observed 
defaults into a relatively small group of high-risk applicants.

However, the strategy also rejected 4,447 applicants who did not default. 
Therefore, the results illustrate the trade-off between reducing credit 
risk and rejecting potentially good borrowers.






These results come from a historical test-set simulation and do not 
guarantee equivalent improvements in a real lending operation.
