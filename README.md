## Objective:

- Evaluate potential mispricing of default risk  
- Train ML models (Logistic Regression, Random Forest, XGBoost) using loan origination features  
- Apply PCA + K-Means to identify borrower clusters with similar risk profiles  
- Analyze mispricing patterns across loan grades

## Exploratory Analysis

- By 2012 most of loans are funded, and thus held, by investors. While investors are funding most of the loans, Lending Club commits to hold only minimal amount of loans on their books. 
<img width="575" height="405" alt="Screenshot 2026-04-26 at 5 35 50 PM" src="https://github.com/user-attachments/assets/a101ccfa-78a9-48d5-9eff-5c708c59b0cf" />

- Data is unevenly distributed and the charged off category is underrepresented, which is expected for a well-performing loan portfolio.
<img width="651" height="442" alt="Screenshot 2026-04-26 at 5 37 31 PM" src="https://github.com/user-attachments/assets/279346f9-59e3-4f32-b62c-61054d9bc79a" />

- Every loan grade (A-G) had expanded over time; however, grades B and C were particularly strongly represented, which indicates that Lending Club focused on the medium-risk loan segment, 
<img width="651" height="442" alt="Screenshot 2026-04-26 at 5 38 18 PM" src="https://github.com/user-attachments/assets/36256d9d-73b0-4279-863f-10f40501bb7e" />
<img width="651" height="442" alt="Screenshot 2026-04-26 at 5 39 21 PM" src="https://github.com/user-attachments/assets/fd02a468-377c-415d-9217-5ff39e55aea2" />


## Models Used:

### PCA

- The cumulative variance curve indicates that 28 components are needed to explain 90% of total variance, which reflects the complexity and correlation structure of borrower
 attributes.
- The 2D PCA scatter plot shows loose but noticeable separation between charged-off and fully-paid loans, meaning the first two components capture meaningful risk-related patterns.
- Overall, PCA confirms strong feature redundancy and reveals latent structure that supports downstream clustering and mispricing analysis.

<img width="718" height="481" alt="Screenshot 2026-04-26 at 5 41 45 PM" src="https://github.com/user-attachments/assets/7bd958f4-5a2f-4c78-bdde-a293fdae51fa" />
<img width="718" height="481" alt="Screenshot 2026-04-26 at 5 42 07 PM" src="https://github.com/user-attachments/assets/4fcc98f5-8a89-4327-a4bf-9b0cc17327ae" />


### K-Means clustering revealed 3 main borrower risk groups with distinct characteristics:

Cluster 0: Higher income, higher DTI, high utilization, and a moderate default rate (8.9%).

Cluster 1: Lower income than average but moderate utilization; elevated default rate (13.8%).

Cluster 2: Mid-income group with the highest default rate (15.1%), reflecting riskier borrowers with higher interest rates.

Cluster 3: Higher-income borrowers but very high utilization, producing a moderate risk level (12.2%).

Cluster 4: Lowest income cluster with low interest rates and relatively low default rate (9.8%).

Cluster 5: High income but also high DTI; moderate default rate (12.5%).

<img width="749" height="813" alt="Screenshot 2026-03-17 at 9 01 54 PM" src="https://github.com/user-attachments/assets/3d0de0c4-be44-4a5d-a649-5364b5ca2f4e" />

### Logistic Regression
- Logistic Regression is advantageous as it is more likely to return well calibrated probability estimates, which allows direct interpretation as default likelihoods.

- Our ROC-AUC (0.772) score shows relatively strong ability to predict borrower default risk. Similar values of Train/Test Brier score of 0.0919 and 0.0918 indicate relatively well-calibrated predictions (with a score of 0 being perfect calibration).

<img width="522" height="481" alt="Screenshot 2026-04-26 at 5 45 16 PM" src="https://github.com/user-attachments/assets/0a6356d6-99c2-493e-897a-280fb53dfd04" />
<img width="777" height="449" alt="Screenshot 2026-04-26 at 5 46 41 PM" src="https://github.com/user-attachments/assets/8924c6e2-be6a-49a9-abb8-f02eb73caf26" />

### Random Forest / XGBoost
- Random Forest model was able to achieve an ROC-AUC score of 0.77, which was comparable to the score achieved by Logistic Regression (combined with PCA).
- XGBoost model achieved an ROC-AUC score of 0.78, which is comparable to both the Logistic Regression and Random Forest models, indicating similar effectiveness in ranking borrowers by default risk.
<img width="525" height="482" alt="Screenshot 2026-04-26 at 5 48 20 PM" src="https://github.com/user-attachments/assets/619b1e47-3f0b-4a6a-b576-b12a14c6dfb1" />
<img width="498" height="449" alt="Screenshot 2026-04-26 at 5 47 56 PM" src="https://github.com/user-attachments/assets/95d5ad33-8952-488d-aa3d-aae254047aad" />

- Model comparison based on ROC curves indicates that Logistic Regression, Random Forest, and XGBoost models show similar performance in terms of ranking borrowers by default risk (ROC-AUC), despite the model differences in complexity and handling of nonlinearities.

## Mispricing Analysis

- Define mispricing as:

  Mispricing = Predicted PD - Implied PD

- Positive mispricing indicates loans where our model assigns higher default risk than the grade implies (i.e., risk may be underpriced for those borrowers). Negative mispricing indicates loans where the model predicts lower risk than the grade implies (i.e., risk may be overpriced). 

- For this analysis, we specifically selected logistic regression as the final model because it produced the most stable and well-calibrated probability estimates. Mispricing requires PDs that behave like true probabilities, as the results are interpreted directly against Lending Club's grade-implied risk levels. Tree-based models (e.g., Random Forest, XGBoost) can achieve high accuracy but typically produce discontinuous, poorly calibrated, and overconfident probability outputs, which can distort mispricing results. Logistic regression's monotonic, interpretable probability structure makes it the most appropriate choice for comparing model-estimated risk to pricing signal.
  
- Mispricing was somewhat pronounced for grades D-G, suggesting potential opportunities for risk-adjusted investment.

- Results indicated that higher-grade loans (A and B) exhibited positive mispricing, indicating that model-implied PD (probability of default) was lower than
the level implied by market spreads.
-In contrast, lower-grade loans (D through G) displayed increasingly negative mispricing, implying that model-implied PD exceeded the reflected pricing,indicating that expected losses were underestimated, particularly in the lowest grades (F and G), where the discrepancy was largest. As a result, credit spreads in these
segments do not fully compensate for the underlying default risk, leading to overstated risk-adjusted returns.

<img width="749" height="585" alt="Screenshot 2026-03-17 at 9 07 31 PM" src="https://github.com/user-attachments/assets/60e6437f-0984-4ed5-8893-fec1daecb052" />

<img width="749" height="585" alt="Screenshot 2026-03-17 at 9 09 03 PM" src="https://github.com/user-attachments/assets/1a3491de-f08f-414c-86be-50ad03b20d16" />
<img width="769" height="495" alt="Screenshot 2026-03-17 at 9 09 21 PM" src="https://github.com/user-attachments/assets/5c1b0386-4a6a-4e3d-b239-101a0c7bfd12" />
