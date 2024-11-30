# Customer-Retention-Forecasting-and-Churn-Risk-Assessment
Overview
Customer attrition, often called churn, is when clients discontinue a service. This is a critical business metric in industries like telecom, internet services, insurance, and security monitoring, where customer retention is often more cost-effective than acquisition. Companies in these sectors analyze churn rates to focus their customer retention efforts on individuals most likely to leave. Using predictive models, churn analysis identifies customers at high risk of churning, allowing targeted retention strategies.

In this project, I combined customer survival analysis with a predictive churn model and developed a web app that visualizes these insights. This app helps explain why customers might discontinue the service and estimates each customer’s potential lifetime value (CLV).

Project Structure
php
Copy code
├── Images/                             # Contains images
├── static/                             # Holds plots for gauge chart, hazard and survival curves, SHAP values in Flask app
│   └── images/
│       ├── hazard.png
│       ├── surv.png
│       ├── shap.png
│       └── new_plot.png
├── templates/                          # HTML templates for Flask app
│   └── index.html
├── Customer Survival Analysis.ipynb    # Notebook for Kaplan-Meier, log-rank test, and Cox proportional hazards model
├── Exploratory Data Analysis.ipynb     # Customer data analysis
├── Churn Prediction Model.ipynb        # Churn prediction using Random Forest
├── app.py                              # Flask app main script
├── app-pic.png                         # App interface screenshot
├── explainer.bz2                       # SHAP explainer
├── model.pkl                           # Random Forest model
├── survivemodel.pkl                    # Cox proportional hazards model
├── requirements.txt                    # Dependencies
├── Procfile                            # For Heroku deployment
├── LICENSE.md                          # License file
└── README.md                           # Project summary and instructions
Survival Analysis
Survival analysis evaluates the time until an event of interest occurs (e.g., customer churn), allowing us to model factors influencing customer lifespan and retention. Key objectives include:

Assessing how the likelihood of customer churn evolves over time.
Understanding how customer characteristics impact churn.
Identifying factors driving churn risk.
Visualizing survival and hazard curves for individual customers.
Calculating expected customer lifetime value (CLV).
Kaplan-Meier Survival Curve
The Kaplan-Meier curve shows how the probability of a customer staying with the service decreases over time. Key insights:

Over 60% of customers remain after 72 months, with a steady decrease in retention from 3-60 months.
After 60 months, churn probability rises more sharply.
Log-Rank Test
The log-rank test helps identify statistically significant differences in churn probability across customer groups. Insights include:

Features like gender and phone service type are not significant (p-value > 0.05).
Young customers with families tend to have a lower churn risk, possibly due to busier lives or higher income.
Customers who do not subscribe to additional services like online backup or device protection have a higher churn risk.
Fiber optic internet and month-to-month contracts are associated with higher churn rates.
Customers who pay automatically are less likely to churn, suggesting convenience is a factor.
Cox Proportional Hazards Model
The Cox model is used to assess the risk (hazard rate) of churning based on several factors. Using this model, we can estimate a customer's remaining "lifespan" with the company and plot individual survival and hazard curves.

Customer Lifetime Value (CLV)
Customer lifetime value is calculated by multiplying a customer's monthly charges by their expected remaining lifespan, which is determined by the survival function (assuming a churn probability threshold of 10%).

Churn Prediction
The project includes a machine learning model (Random Forest) to predict churn based on various factors.

Analysis Insights
Tenure and Churn: Longer tenure correlates with lower churn rates, indicating increased loyalty over time.
Service Adoption and Churn: New customers who don’t adopt additional services (e.g., streaming) have higher churn rates.
Internet Service and Contract Type: Month-to-month contracts with fiber optic internet see higher churn rates.
Payment Method and Contract Type: Month-to-month customers prefer electronic or mailed checks, possibly due to the ease of cancellation.
Monthly Charges: Higher monthly fees are associated with increased churn.
Model Implementation
I used an ensemble-based Random Forest model due to the complexity and non-linearity of the data. The model is balanced to address class imbalance, with false negatives weighted higher than false positives. Hyperparameters were tuned via Grid Search Cross Validation to avoid overfitting, achieving a final F1 score of 0.62 and ROC-AUC of 0.85.

Model Interpretability
The model’s predictions are explained using:

Permutation Importance: Randomly shuffles feature values to assess their impact on model performance.
Partial Dependence Plot: Shows how churn probability changes with specific feature values.
SHAP Values: A game-theoretic approach to attribute predictions to individual features, helping explain why a customer is likely to churn.
Flask App
The app deploys the final tuned Random Forest model and Cox proportional hazards model through a Flask interface. Users can input customer data to view:

Churn probability
SHAP values explaining key churn factors
Survival and hazard curves
Estimated customer lifetime value (CLV)
