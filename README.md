# Brazilian-Coffee-Analysis
[Python | Excel] Market analysis showcasing the predictive ability of Brazilian weather on coffee futures
TABLE OF CONTENTS

INTRO

METRIC DESCRIPTION:
**Mean Squared Error:** Measures the average squared difference between the predicted and actual stock prices. A lower MSE indicates that the models predictions are closer to the actual vallue.
**Root Mean Squared Error:** Measures the standard deviation of the prediction errors in a model. More intuitve compared to MSE becuase it uses the real world unit that eh model is predicting (RSME of 3 means the model is $3 off from predicted stock value). A lower RSME indicates the models predictions are closer to teh actual value.
**R-squared:** Measures how well a stock prediction model explains the variability of actual stock prices. Ranges from 0-1 where a value closer to one indicates the model performs better.

PICKING FEATURES: 


LASSO REGRESSION MODEL:
The inital model I tested was the lasso regression model to create a benchmark for other models. The reason I chose to implement a lasso regression model instead of a simple linear regression was due to two main reasons. 
1. Simple linear regression is prone to overfit, which creates a model that performs well on the training data, but performs much worse on any other testing data due to the model overestimating the importance of certain variables. Lasso regression helps with this problem as it uses regularization to combat overfitting.
2. Another common problem with simple linear regression models is multicollinearity, which again is combatted in lasso regression from regularization.

After running the lasso regression model, we achieved underperforming metrics on the test dataset:

| Metric | Score |
|--------|-------|
| MSE    | 688.09|
| RSME   | 26.23 |
| R^2    | 0.57  |

<img width="850" alt="Screenshot 2025-02-03 at 9 50 29 AM" src="https://github.com/user-attachments/assets/88fce8ad-1a00-4cea-83d2-5d27f77953a0" />

We see that the model is on average **$26 off of the actual values**, and **only 57% of the data is explained** in this model. Looking at this graphically, we see that the model does well predicting values between 200 and 250, however when we get to more extreme values the model's performance worsens. The reason behind this model's poor performance, is likely due to the model trying to capture a linear trend in the stock that exhibits non-linear relationship.

RANDOM FOREST MODEL
After setting the benchmark model (lasso regression), I implemented a random forest model to try and capture the non-linear relationship shown in stock data. We see in the data below that the random forest model was able to capture these trends better than the lasso regression above:

| Metric | Score |
|--------|-------|
| MSE    | 41.08 |
| RSME   | 6.41  |
| R^2    | 0.97  |

<img width="850" alt="Screenshot 2025-02-03 at 10 19 49 AM" src="https://github.com/user-attachments/assets/6987ce31-e574-4092-9cd9-8c5ecb2bfebd" />


FINAL INSIGHTS

