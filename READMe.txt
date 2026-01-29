Data Quality & Preprocessing
- Data Leakage: The duration variable was identified as a source of data leakage (it is not known before a call is made) and was removed to ensure a realistic predictive model.
- Feature Engineering: The pdays column contained the placeholder value 999 (representing "never contacted"), which skewed the distribution. This was converted into a binary flag (pdays_contacted).
- Imbalanced Classes: The dataset is heavily imbalanced, with approximately 89% of clients rejecting the offer (y='no') and only 11% subscribing (y='yes'). This established a high baseline accuracy of ~89%.

Key Exploratory Insights
- Visualizations indicated that certain job categories (e.g., retired and student) tend to have higher subscription rates relative to their population size compared to blue-collar workers.
- The Euribor 3-month rate and employment variation rate showed strong correlations with subscription success. Lower interest rates generally correlated with a higher likelihood of subscribing to a term deposit.
- The heatmap revealed high multicollinearity between economic indicators (e.g., nr.employed, euribor3m, emp.var.rate), suggesting that these variables provide redundant information.

Model Performance
- Logistic Regression provided a strong, interpretable baseline. It performed well in terms of training time but struggled with recall due to the class imbalance.
- After tuning max_depth, the Decision Tree offered good interpretability and captured non-linear relationships. It likely showed a smaller gap between Train and Test accuracy compared to the unpruned version, indicating successful mitigation of overfitting.
- While powerful, SVM was significantly slower to train. Unless it provides a substantial jump in F1-Score, its computational cost makes it less ideal for a real-time marketing environment compared to lighter models.

Next Steps
- Address class imbalance using techniques like SMOTE or using class weights i.e. penalizing the model more for missing a 'Yes' should be implemented to improve recall. The current models likely favor the majority class ('No').
- Due to the high correlation seen in the heatmap, we should remove redundant economic variables (e.g. keep euribor3m but drop employed) to reduce noise and improve model stability.
- Instead of the default 0.5 decision threshold, we should tune the probability threshold to capture more potential subscribers. This would increase recall at the cost of some precision, which is often an acceptable trade-off in marketing campaigns where missing a potential sale is costly.

Strategic Business Recommendations
- The marketing team should prioritize clients who have been previously contacted (pdays_contacted=1), as past engagement is a strong predictor of future success. Additionally, focus campaigns on retired and student demographics, as they show higher conversion rates.
- Launch aggressive campaigns during periods of lower interest rates (euribor3m) or specific economic conditions identified in the boxplots, as consumer confidence appears tied to these macro-indicators.
- Do not use "call duration" as a KPI for prediction, but do use it for benchmarking sales agent performance post-call.

Model Selection Recommendation
- Primary Choice: logistic regression; second choice: decision trees
- Despite potentially lower raw accuracy than complex ensembles, Logistic Regression offers interpretability (we can see exactly which coefficients drive the decision) and speed. In a marketing context, explaining why a customer was flagged as a lead is often as important as the prediction itself.