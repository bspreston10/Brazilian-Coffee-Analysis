# Brazilian-Coffee-Analysis

# TABLE OF CONTENTS
- [Intro](#intro)
- [Lasso Regression Model](#lasso-regression-model)
- [Random Forest Model](#random-forest-model)
- [SARIMA Model](#sarima-model)
- [Final Insights](#final-insights)



# INTRO
Coffee is one of the most consumed beverages in the world, with over 2 billion cups enjoyed every day. As a globally traded commodity, coffee futures are influenced by a complex mix of factors, including economic trends, supply chain dynamics, and most notably, weather conditions in key producing regions. Brazil, being the largest coffee producer globally, plays a pivotal role in shaping coffee prices.

This analysis explores the predictive power of Brazilian weather data on coffee futures. By leveraging machine learning models, including Lasso Regression and Random Forest, we aim to identify how specific climate variables such as relative humidity, soil temperature, and soil moisture affect coffee price fluctuations. Our goal is to uncover patterns that can enhance forecasting accuracy, provide valuable insights for traders and analysts, and contribute to a more data-driven approach in understanding commodity markets.


## METRIC DESCRIPTION:
- **Mean Squared Error:** Measures the average squared difference between the predicted and actual stock prices. A lower MSE indicates that the models predictions are closer to the actual vallue.
- **Root Mean Squared Error:** Measures the standard deviation of the prediction errors in a model. More intuitve compared to MSE becuase it uses the real world unit that eh model is predicting (RSME of 3 means the model is $3 off from predicted stock value). A lower RSME indicates the models predictions are closer to teh actual value.
- **R-squared:** Measures how well a stock prediction model explains the variability of actual stock prices. Ranges from 0-1 where a value closer to one indicates the model performs better.

## PICKING FEATURES: 
Before running any predictive models, I collected data from major coffee-producing regions in Brazil, including Minas Gerais, São Paulo, and Espírito Santo—areas that significantly contribute to Brazil’s global coffee output. Recognizing that weather patterns play a crucial role in coffee crop yields, I sourced comprehensive climate data from the NOAA API, focusing on variables that are known to impact coffee growth and production cycles.

The dataset included the following key weather metrics:

- Soil Temperature: Monitored at various depths to understand how root zone conditions influence coffee plant health.
- Soil Moisture: Essential for assessing drought stress and irrigation needs, which directly affect coffee bean development.
- Air Temperature (°F): Captures daily fluctuations, critical for coffee flowering and bean ripening.
- Relative Humidity (%): Affects disease prevalence (e.g., coffee leaf rust) and influences plant transpiration rates.
- Precipitation (mm): Provides insights into rainfall patterns that impact flowering, fruit set, and harvest quality.
- Wind Speed and Direction: Though less direct, wind conditions can affect pollination, moisture loss, and crop resilience.

To enhance the predictive power of the models, I also engineered lagged variables (e.g., 30-day, 90-day, and 260-day lags) to capture delayed effects of climate events on coffee prices. Additionally, I calculated rolling correlations for key metrics like relative humidity and soil moisture, allowing the models to detect shifting weather trends over time.

# LASSO REGRESSION MODEL:
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

# RANDOM FOREST MODEL
After setting the benchmark model (lasso regression), I implemented a random forest model to try and capture the non-linear relationship shown in stock data. We see in the data below that the random forest model was able to capture these trends better than the lasso regression above:

| Metric | Score |
|--------|-------|
| MSE    | 41.08 |
| RSME   | 6.41  |
| R^2    | 0.97  |

<img width="850" alt="Screenshot 2025-02-03 at 10 19 49 AM" src="https://github.com/user-attachments/assets/6987ce31-e574-4092-9cd9-8c5ecb2bfebd" />

We can also look at the most important factors in the model:

| Feature                                           | Importance |
|---------------------------------------------------|------------|
| Rolling_corr_relative_humidity_2m (%)_bahia       | 0.39       |
| 260_lag_soil_temperature_7_to_28cm (°F)_bahia     | 0.22       |
| 260_lag_soil_moisture_7_to_28cm (m³/m³)_bahia     | 0.13       |

We see that the model is on average **$6.41 off of the actual values**, and **97% of the data is explained** in this model. Looking at this graphically, we see that the model does much better in predicting the actual values, except for extreme outliers at around 325, which is expected. We also see that rolling correlation for relative humidity in Bahia is the most importnat variable, then the soil temperature and soil moisture in Bahia. 

# SARIMA MODEL
Finally, I implemented a SARIMA model to account for the seasonality and time-dependent patterns inherent in stock data. The SARIMA model is well-suited for capturing both trend and seasonal components, which are common in financial time series. As shown in the data below, the SARIMA model was able to effectively model these temporal dynamics, providing improved performance over the lasso regression.

| Metric | Score |
|--------|-------|
| MSE    | 46.38 |
| RSME   | 6.81  |
| R^2    | 0.97  |

<img width="850" alt="Screenshot 2025-02-03 at 10 52 32 AM" src="https://github.com/user-attachments/assets/79eb038c-9157-46ae-b2d3-9d1c4c255cef" />

We see that the model is on average **$6.81 off of the actual values**, and **97% of the data is explained** in this model. Looking at this graphically, we see that the model does well in predicting values,  expect for a spike in the lower values. While this model performs well, the random forest is still technically better at predicting the stock price. 

# FINAL INSIGHTS
Through this analysis, we’ve demonstrated that Brazilian weather conditions have a significant impact on coffee futures prices. The Lasso Regression model served as an effective benchmark, highlighting the limitations of linear models when dealing with complex, non-linear relationships commonly found in commodity markets. Its performance, with an R-squared of 0.57 and an average error of $26, emphasized the need for more sophisticated models.

The Random Forest model captured the non-linear dynamics much more effectively. With an impressive R-squared of 0.97 and an average error of just $6.41, it clearly outperformed the Lasso Regression. The model identified the rolling correlation of relative humidity, soil temperature, and soil moisture in Bahia as the most influential factors affecting coffee prices. These insights underscore the critical role of weather variability in Brazil, particularly in key coffee-producing regions.

Finally, the SARIMA model was implemented to account for the seasonality and time-dependent patterns inherent in coffee price data. With an R-squared of 0.97 and an average error of $6.81, the SARIMA model demonstrated strong performance, nearly matching that of the Random Forest model. Its ability to capture temporal trends and seasonal fluctuations adds a valuable dimension to forecasting, especially when understanding cyclical patterns driven by climate and agricultural cycles.

In conclusion, incorporating weather data—particularly from regions like Bahia—can significantly enhance the predictive accuracy of coffee price models. While the Random Forest model slightly outperformed SARIMA in terms of raw error metrics, the SARIMA model’s strength lies in its capacity to model seasonality and long-term trends. Combining both machine learning and time series approaches provides a more robust framework for traders and analysts, offering strategic advantages in navigating the volatility of the coffee futures market.
