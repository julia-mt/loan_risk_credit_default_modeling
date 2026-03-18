## Objective:

- Evaluate potential mispricing of default risk  
- Train ML models (Logistic Regression, Random Forest, XGBoost) using loan origination features  
- Apply PCA + K-Means to identify borrower clusters with similar risk profiles  
- Analyze mispricing patterns across loan grades

## Key Findings:

### PCA + K-Means clustering revealed 3 main borrower risk groups with distinct characteristics:

Cluster 0: Higher income, higher DTI, high utilization, and a moderate default rate (8.9%).

Cluster 1: Lower income than average but moderate utilization; elevated default rate (13.8%).

Cluster 2: Mid-income group with the highest default rate (15.1%), reflecting riskier borrowers with higher interest rates.

Cluster 3: Higher-income borrowers but very high utilization, producing a moderate risk level (12.2%).

Cluster 4: Lowest income cluster with low interest rates and relatively low default rate (9.8%).

Cluster 5: High income but also high DTI; moderate default rate (12.5%).

<img width="749" height="813" alt="Screenshot 2026-03-17 at 9 01 54 PM" src="https://github.com/user-attachments/assets/3d0de0c4-be44-4a5d-a649-5364b5ca2f4e" />

- Mispricing was somewhat pronounced for grades D-G, suggesting potential opportunities for risk-adjusted investment.

<img width="749" height="585" alt="Screenshot 2026-03-17 at 9 07 31 PM" src="https://github.com/user-attachments/assets/60e6437f-0984-4ed5-8893-fec1daecb052" />

<img width="749" height="585" alt="Screenshot 2026-03-17 at 9 09 03 PM" src="https://github.com/user-attachments/assets/1a3491de-f08f-414c-86be-50ad03b20d16" />
<img width="769" height="495" alt="Screenshot 2026-03-17 at 9 09 21 PM" src="https://github.com/user-attachments/assets/5c1b0386-4a6a-4e3d-b239-101a0c7bfd12" />
