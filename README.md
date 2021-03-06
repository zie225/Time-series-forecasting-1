# :chart_with_upwards_trend: Time-series-forecasting

 This repository is aiming to Time Series Forecast over a given dataset. The goal of the exercise is to find the most appropriate model and make some predictions.

*Forecast done in R*

<p align="center">
  <img src="https://www.analyticsindiamag.com/wp-content/uploads/2018/12/timser.gif"/>
</p>



## Subject

The file Elec-train.xlsx ([dataset](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/dataset/Elec-train.xlsx)) contains electricity consumption (kW) and outdoor air temperature for one building.

These quantities are measured every 15 minutes, from 1/1/2010 1:15 to 2/16/2010 23:45. In addition, outdoor
air temperature are available for 2/17/2010. The goal is to forecast electricity consumption (kW) for
2/17/2010.

Two forecasts should be returned, in csv files entitled prediction and prediction with temperature:

1. the first one without using outdoor temperature,
2. the second one using outdoor temperature.

Of course, the goal is to get the best possible forecasting. 


 

## Solution  


### Visualization

R has a broad range of packages that make data visualization more exciting. We will focus on:
- [x] forecast
- [x] ggplot2

As used in the [code](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/model/TimesSeries.ipynb) and by using the 80:20 ration, the observation can easily be done.

  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/visuatraintest.png"/>
  <figcaption>Figure 1: Electricity Consumption (kW per hour)</figcaption>
  </p>


### 1. Forecast without outdoor temperature
 
It is not always an easy task to find the most appropriate model for a given dataset. However, we will stick on a searching methodology that has proven to be effective.

Starting with the Exponential Smoothing models, we will observe those not using seasonal patterns and then move to the ones that use them.

- **Exponential Smoothing**
  
  - **Non-seasonal pattern**
  
  In this section, we have focused on models that do not use seasonal pattern. A comparasion has been done in between the Simple Exponentional Smoothing and the Non-seasonam Holt-Winters. After setting the parameters, alpha in SES to NULL and (alpha, beta) to (NULL, NULL) we can observe it on Figure 2.
  
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/nonseasonal.png"/>
  <figcaption> Figure 2: Simple Exponential Smoothing vs Non-seasonal Holt-Winters </figcaption>
  </p>
  
 
  It is clearly seen on Figure 2 that both models are not appropriate.
   
  - **Seasonal pattern** 
  
  We will now focus the forecasts over the seasonal patterns and linear trends.
  
  - [ ] **Additive seasonal Holt-Winters**: paramaters (seasonal='additive', h=900)
  - [ ] **Multiplicative seasonal Holt-Winters**: paramaters (seasonal='multiplicative', h=900)
  - [ ] **Damped additive seasonal Holt-Winters**: paramaters (seasonal='additive', h=900, damped=TRUE)
  - [ ] **Damped multiplicative seasonal Holt-Winters**: paramaters (seasonal='multiplicative', h=900, damped=TRUE)
  
  _see_ [code](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/model/TimesSeries.ipynb)
  
  
  After setting the parameters, we can observe the plot on Figure 3.
    
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/seasonalandlinear.png"/>
  <figcaption> Figure 3: Additive seasonal vs Multiplicative seasonal vs Damped additive vs Damped multiplicative</figcaption>
  </p>
    
  The models on Figure 3 are still not suitable.
  We can try to stabilize the variance by Box-Cox transformation in the previous Additive HW models.
  - [ ] **Additive seasonal Holt-Winters**: paramaters (seasonal='additive', h=900, lambda = 'auto')
  - [ ] **Damped additive seasonal Holt-Winters**: paramaters (seasonal='additive', h=900, damped=TRUE, lambda = 'auto')
  
  _see_ [code](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/model/TimesSeries.ipynb)
  
  After setting the parameters, we can observe the plot on Figure 4.
  
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/boxcox.png"/>
  <figcaption> Figure 4: Stqbilization in Additive HW models</figcaption>
  </p>
 
  As we can see a small omprovement on the previous results, it is preferable to compute the root mean square error (RMSE) of each model to have a better view.
  - [ ] RMSE in Simple Exponentional Smoothing: 60.76483
  - [ ] RMSE in Non-seasonam Holt-Winters: 64.57402
  - [ ] RMSE in Additive seasonal Holt-Winters: 73.46383
  - [ ] RMSE in Multiplicative seasonal Holt-Winters: 73.40462
  - [ ] RMSE in Damped additive seasonal Holt-Winters: 60.91925
  - [ ] RMSE in Damped multiplicative seasonal Holt-Winters: 60.48178
  - [ ] RMSE in Additive seasonal Holt-Winters Stabilised: 70.18168
  - [ ] RMSE in Damped additive seasonal Holt-Winters Stabilised: 60.84705
  
  _see_ [code](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/model/TimesSeries.ipynb)
  
  **RECAP**:
  
The model with the lowest error so far is Damped multiplicative Holt-Winters, however; this is not because it is the best model (See that the prediction pattern is not correlate with pattern of test set).     
This low error is just because our calculation base on mean different and Damped multiplicative Holt-Winters gives us the linear model that situates around the middle of the graph.    
We should also consider that all alpha, beta, gamma and phi parameters used above are automatically chosen just to screen and see the pattern of each forecasting model.    
Therefore, if we really want to compare between each model, the parameters should be fixed.    
(For example, if you want to compare the effect of damped version to multiplicative Holt-Winters model, you should fix alpha, beta and gamma for both model (the only difference will be with or without phi parameter))    
But we will not consider them now since clearly we cannot use any exponential smoothing for our forecast.


 **ARIMA: AutoRegressive Integrated Moving Average**

As the Exponential Smoothing models cannot be suitable for our forecast, a better move would be trying the ARIMA models for example.

   - **SARIMA: Seasonal AutoRegressive Integrated Moving Average model**
  
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/sarima.png"/>
  <figcaption> Figure 5: SARIMA model</figcaption>
  </p>
  
  The model is not perfect although it looks better than the previous ones. We can see the following root mean square error (RMSE= 56.392)
  This model shows less error but the prediction pattern are still not so good.  
  We should look for a model that is more flexible like Neural Network Auto-Regression.
  
   - **Neural Network Auto-Regression**
  
  The parameters p and k are automatically chosen.
  
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/neralnetworkautoreg.png"/>
  <figcaption> Figure 6: Neural Network Auto-Regression</figcaption>
  </p>
  
  The root mean square error (RMSE) is 89.75295. Eventhough the error is higher than SARIMA model but the prediction pattern seems correct. 
This correlates with the information from auto-correlation function (acf) below.  
See that auto-correlation declines slowly as the number of lags increases. 
This is a property of non-stationarity that will effect the efficiency of several forecasting models.  
It also possible that our data might has no seasonal but cyclic pattern. In that case, they cannot be modelized by usual linear model.

_see_ [code](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/model/TimesSeries.ipynb)

  In addition, we can look the Auto-correlation pattern in order to have a clear view in Figure 7.
  
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/autocorr.png"/>
  <figcaption> Figure 7: Auto-correlation pattern</figcaption>
  </p>
  
  Moreover, observe the information from the Neural network model to improve the model precision
  
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/NNinfos.png"/>
  <figcaption> Figure 8: Neural network infos</figcaption>
  </p>
  
  Furthermore, we can try to add more neurons to the model in order to stabilize variance by Box-Cox transformation.
  _see_ [code](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/model/TimesSeries.ipynb)
  
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/stabvariance.png"/>
  <figcaption> Figure 8: Neural Network with more neurons</figcaption>
  </p>
  
  The root mean square error (RMSE) is 70.82671
  The RSME is the lower and this definitely is the best model so far!
  
  Let's clear see the prediction again. 
  
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/seepred.png"/>
  <figcaption> Figure 9: Suitable Neural Network with more neurons</figcaption>
  </p>
  
  _see_ [code](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/model/TimesSeries.ipynb)

It seems that we have found the most suitable model for our dataset, we will now predict the electricity consumption of 17/2/2010 based on the whole previous consumption information.
The prediction interval = 24 hr for the entire day of 17/2/2010. Therefore h =(24*60)/15 = 96 observations.

<p align="center">
<img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/pred.png"/>
<figcaption> Figure 10: Predictions without Temperatures</figcaption>
</p>

_see_ [Predictions](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/predictions/Prediction.csv)
  
### 2. Forecast using outdoor temperature

In this section, we will forecast electricity consumption (kW) for 2/17/2010 by using outdoor temperature.
After splitting the dataset into train and test sets, we have to verify the effect of temperature to electricity consumption.

<p align="center">
<img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/effect.png"/>
<figcaption> Figure 11: Temperature effect on Electricity</figcaption>
</p>

As the effect of temperature to electricity consumption are statisticaly significant. We can add trend and seasonal pattern to this regression.
_see_ [code](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/model/TimesSeries.ipynb)

<p align="center">
<img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/effecttrenddd.png"/>
<figcaption> Figure 11: Temperature effect on Electricity with trend</figcaption>
</p>

<p align="center">
<img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/effecttrendseason.png"/>
<figcaption> Figure 12: Temperature effect on Electricity with trend and season</figcaption>
</p>

**Comparison of the three previous models**

<p align="center">
<img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/comparrr.png"/>
<figcaption> Figure 13: Comparison of the three previous models</figcaption>
</p>

From the comparison, we will choose the model with trend only for the rest of the exercise because this model has the lowest AIC and the highest Adjusted R squared.

In addition, as Time series linear regression model assumes that the residuals are independent and identically distributed. Therefore we have to see the residuals of our model first.

_see_ [code](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/model/TimesSeries.ipynb)

<p align="center">
<img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/residualfirst.png"/>
<figcaption> Figure 14: Residual in the model Temperature~Electricity</figcaption>
</p>


Furthermore, the PART ONE of the exercise has proved that the residuals are dependent, so we can not go for the linear regression model.
Therefore, the appropriate model in case the residual are not independent to each other is the **Dynamic regression model**.

  - **Dynamic regression model**
  
  _see_ [code](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/model/TimesSeries.ipynb)
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/autocorresid.png"/>
  <figcaption> Figure 15: Autocorrelation of residuals</figcaption>
  </p>
  
  It is clearly seen that all the auto-correlations of the residuals have been arranged with this model so we can validate the model and then forecast the test set.
  
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/testpred.png"/>
  <figcaption> Figure 16: First predictions with Temperature</figcaption>
  </p>
  
  This is acceptable but as the best model we have so far is **Neural Network** maybe we should try them with temperature variable.
  _see_ [code](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/model/TimesSeries.ipynb)
  
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/secodpred.png"/>
  <figcaption> Figure 17: Second predictions with Temperature</figcaption>
  </p>
  
  The Root Mean Square Error in this case is: 83.42295, however we can try to zoom into the predictions.
  
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/zoompred.png"/>
  <figcaption> Figure 18: Second predictions with Temperature Zoomed</figcaption>
  </p>
  
  This model seems to be suitable for our dataset, we should therefore undertake the forecast electricity consumption on 17th Feb based on temperature of that day.
  
  <p align="center">
  <img src="https://github.com/IsmaelMekene/Metaheuristics--Stochastic-Optimization/blob/main/images/forecastwith.png"/>
  <figcaption> Figure 19: Predictions with Temperatures</figcaption>
  </p>
  
  _see_ [Predictions](https://github.com/IsmaelMekene/Time-series-forecasting/blob/main/predictions/Prediction%20with%20Temperature.csv)
  


