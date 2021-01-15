# FORECAST COVIDE-19 Fatalities
ARIMA model to predict Covid19 Fatalities in US

## Objective
This project was conducted to describe the procedures of forecasting the US COVID-19 death toll in 2021 with a 90% confidence interval. 

## Introduction
Many medical research studies have explored and investigated the relationship between COVID-19 fatalities and clinical features, including age, minimum oxygen saturation during encounter, and health-care setting of the patient encounter. These clinical features are highly related to the fatalities, but the time budget and data accessibility made me look into simpler but reliable methods (or approaches). Also, other factors, such as social distancing and wearing masks, are substantial elements that affect the incidence and fatality of COVID-19, but it is difficult to obtain a suitable dataset. Considering the constraints, alongside the rich theory and relatively easy to implement family of ARIMA models, I opted to stay within that arena when forecasting Covid-19 fatalities. All pertinent details of building and evaluating the model are described in the subsequent sections

## Dataset
US COVID-19 data was obtained from [covidtracking.com](covidtracking.com). The dataset includes the cumulative records of daily test numbers, new cases, deaths, hospitalized patients, etc. Then, I performed simple feature engineering to extract non-cumulative daily statistics from the dataset. Note that the obtained dataset period is between January 22, 2020, when the first COVID-19 case was confirmed and reported to CDC, and October 19, 2020. Overall, the original dataset includes 272 records and 10 features

Daily New Cases                                    |  Daily Fatalities
:-------------------------------------------------:|:-------------------------------------------------------:
![Daily New Cases](./COVID-19_Daily_New_Case.png)  |  ![Daily Fatalities](./COVID-19_Daily_Fatalities.png)

## Time Series Model Development

1)	Building the Model
The accuracy of an ARIMA model is characterized by three parameters: p, d, and q. p and q are the order of the AR (i.e., Auto Regressive) and MA (i.e., Moving Average) terms, respectively. d is the number of differencing required to make the time series data stationary. In the AR model, Y at time t (Yt) depends on the model’s lags. Similarly, Yt in the MA model depends on the lagged forecast errors. Identifying the optimal values of p, d, and q is critical in obtaining promising results.  

When an ARIMA model integrates a seasonal component, it becomes a seasonal ARIMA model (i.e., SARIMA). Besides, a SARIMA with exogenous factors becomes a SARIMAX model. To determine the most suitable model type and the values for p, d, and q, I used the ‘auto_arima’ function from the pmdarima python library. 

The time series data were split into the training and testing sets. Two different split scenarios, 90:10 and 95:5, were applied. Note that the time series data size is only 272, but the model has to forecast daily fatalities for 438 consecutive days (e.g., from 10/20/20 to 12/31/21) if no vaccine or treatment is available until the end of 2021. 

SARIMAX is identified as the optimal model, and the values for p, d, q, and the seasonal factors are (8,1,1) and (0,1,1,12). For the model with 95:5 split, SARIMAX was also identified as the optimal model, and the estimated values were (7,1,2) and (1,1,1,12).
  
2)	Testing the Model
As shown in Figures 2 and 3, the model was tested by predicting daily fatalities for the period between 10/10/20 and 10/19/20. In Figure 2, the predicted fatalities are far less than the actual numbers, especially during the period between 10/13 and 10/17.  The MAPE of this is 27.86%. On the other hand, the test result with the 95:5 split in Figure 3 shows a better result. The resulting MAPE of the model in Figure 3 is 13.99%.

## Forecast Scenarios
I used the model trained with the 95:5 split to predict the COVID-19 fatalities in 2021. The estimate of the fatalities was performed under five different scenarios, as listed below.

1.	Scenario 0: Vaccine will be available by the end of 2020. Assume that no fatalities in 2021  
2.	Scenario 1: Vaccine will be available at the end of the first quarter of the year 2021
3.	Scenario 2: Vaccine will be available at the end of the second quarter of the year 2021
4.	Scenario 3: Vaccine will be available at the end of the third quarter of the year 2021
5.	Scenario 4: Vaccine will be available at the end of the year 2021

For scenarios 1 through 4, assume that there will be no fatalities after the vaccine is released. 

## Notebook
Forecast_COVID-19_Fatalities.ipynb
