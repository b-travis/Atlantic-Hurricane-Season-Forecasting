### Atlantic-Hurricane-Season-Forecasting

**btravis**

#### Executive Summary
Objective: Build a predictive model to forecast an incoming hurricane season in the Atlantic basin. Target variables to include number of named storms, number of hurricanes, number of major hurricanes, and landfalling hurricanes in a given season (June 1st to November 30th) given prevailing climatological conditions at the beginning of the hurricane season.

The models outperformed baselines in predicting hurricane activity, but not by much, and there is likely room for improvement (better data preparation, inclusion of more data/features, etc.)

#### Rationale
The Atlantic hurricane season is carefully watched by meteorologists and storm enthusiasts, the property insurance industry, and by the communities at risk. Tropical Cyclones have inflicted more than $1.4 trillion in damage to the United States since 1980 (CPI adjusted), making it easily the most costly disaster in the U.S. over that time period.
Every year at the beginning of the hurricane season (which officially kicks off on June 1st), universities, private institutions and government agencies like the National Oceanic and Atmospheric Administration (NOAA) publish “hurricane season outlooks” that look at meteorological and ocean conditions and attempt to predict the number of hurricanes. Many of these forecasts also forecast how many major hurricanes (based on storm strength) or landfalling hurricanes (hurricanes that hit the mainland United States) will occur. For example, NOAA on 23 May 2024 published the article "NOAA predicts above-normal 2024 Atlantic hurricane season" detailing their official 2024 season forecast, forecasting "a range of 17 to 25 total named storms (winds of 39 mph or higher). Of those, 8 to 13 are forecast to become hurricanes (winds of 74 mph or higher), including 4 to 7 major hurricanes (category 3, 4 or 5; with winds of 111 mph or higher). Forecasters have a 70% confidence in these ranges."

Researchers at Colorado State University also publish a well regarded annual hurricane season forecast several times throughout the year; in a July 9th report this year they predicted 25 named storms, 12 hurricanes, and 6 major hurricanes, all well above the 1991-2020 historical averages of 14.4 named storms, 7.2 hurricanes, and 3.2 major hurricanes. This year there was broad consensus that the conditions are ripe for an overly active hurricane season, and unfortunately that seems to have begun to materialize with Major Hurricane Beryl having become the earliest Category 5 hurricane on record in the Atlantic. Most forecasts mention higher than average (in fact, record-breaking) sea surface temperatures and La Nina conditions as factors expected to drive the busier-than-usual hurricane season.

Sources:
- NOAA National Centers for Environmental Information (NCEI) U.S. Billion-Dollar Weather and Climate Disasters (2024). https://www.ncei.noaa.gov/access/billions/, DOI: 10.25921/stkw-7w73
- https://www.noaa.gov/news-release/noaa-predicts-above-normal-2024-atlantic-hurricane-season
- https://tropical.colostate.edu/forecasting.html

#### Data Sources
Hurricane/named storm information: hurdat2 dataset
Oceanic Nino Index from NOAA
Sea temperature and other data from the National Data Buoy Center (NDBC) | NOAA

NDBC Data was collected from two buoys: MidGulf (Station 42001) and South Hatteras (Station 41002). These stations were selected due to relatively few gaps in data going back to 1976. Features retained from these buoys:
- WTMP (water temperature)
- WSPD (wind speed)
- WVHT (wave height)
- PRES (air pressure)
- ATMP (air temperature)

Features I expect to be most important in the training of the model: May Mid-Gulf ocean temperature, which is meant to be a proxy for basin-wide sea surface temperature, and should be positively correlated with storm activity. It does seem to be somewhat loosely correlated.
The second feature: May Oceanic Nino Index (ONI), which should be negatively correlated with storm activity.

![alt text](https://github.com/b-travis/Atlantic-Hurricane-Season-Forecasting/blob/main/figures/EDA_NumStorms_and_ONI.png)

#### Methodology
Feature Engineering: As part of Grid Search, implemented Polynomial Features with degree ranging from 1 to 6 on the aforementioned features
Train-Test Split: Years 1976 to 2010 included in training; 2011 to 2022 set aside for testing
Scaling: Feature scaling via StandardScaler(). Only numeric features are used.
Tested 4 different models using sklearn implementations, with a Grid Search to find optimal hyperparameters:
- DummyRegressor(): strategy = ‘mean’; this creates a baseline to compare against
- LinearRegression(): only hyperparameter is polynomial degree for PolynomialFeatures()
- LinearRegression() with select features; features limited to the two ONI and Water Temperature features expected to be most important
- Ridge(): sklearn’s implementation of linear regression with L2 regularization. Hyperparameter: alpha (controls amount of regularization)
- Lasso(): sklearn’s implementation of linear regression with L1 regularization. Hyperparameter: C (controls amount of regularization)

#### Results
In the models I developed, the Test R2 scores were very poor, with values mostly below 0, indicating really poor model performance. (Believe it or not, the results shown are an improvement on several iterations of modeling... but there is a ton of room for improvement.)
Among the tested models, Ridge regularization yielded the best performance in terms of mean squared error and R2. Despite the low Test R2 scores, all the models outperformed the DummyRegressor() baseline model – perhaps there is at least some predictive value in these models.

The models predicting Total Accumulated Cyclone Energy also performed better for the most part than those predicting Number of Storms. It could be that while the number of storms to develop depends on many factors not captured by the beginning of season data, the conditions that create big storms (and therefore greater ACE) are slightly more correlated with the features including in the modeling here.

![Modeling performance metrics](https://github.com/b-travis/Atlantic-Hurricane-Season-Forecasting/blob/main/figures/results_MSE_R2.png)

Some years were predicted better than others. In 2020, for example, model missed an historically active hurricane season. Looks like data was missing for Mid-Gulf buoy for most of 2020, and in our data preparation we  forward-filled missing data. The last known temperature from January 2020 was 24 degrees C, much cooler than the May average of closer to 27 degrees. This is a likely factor in the model “missing” the very active 2020 season. Perhaps different data preparation could be used to populate the May 2020 Mid-Gulf buoy data with something closer to the May means rather than the January 2020 data.

Keep in mind in the following plots that post-2010 data was reserved for testing (i.e. not used in the training of the model):

![Actual vs Predicted: Number of Storms](https://github.com/b-travis/Atlantic-Hurricane-Season-Forecasting/blob/main/figures/actual_v_predicted_numstorms.png)
![Actual vs Predicted: Total ACE](https://github.com/b-travis/Atlantic-Hurricane-Season-Forecasting/blob/main/figures/actual_v_predicted_ACE.png)

#### Next steps
- Add additional features (data): additional buoy data from NDBC, air temperature data, ...
- Better interpolation of missing data
- Fit model on different target variables
- Monthly or two-month forecasting outlooks rather than full-season forecasts (would expect better predictive power on shorter time frames)

#### Outline of project

- [season_forecasting.ipynb](https://github.com/b-travis/Atlantic-Hurricane-Season-Forecasting/blob/main/season_forecasting.ipynb)
- https://github.com/b-travis/Atlantic-Hurricane-Season-Forecasting/blob/main/MLAI_Capstone_Hurricane_Forecasting_updated.pdf 


##### Contact and Further Information
Barrett Travis
barrettktravis at gmail.com
