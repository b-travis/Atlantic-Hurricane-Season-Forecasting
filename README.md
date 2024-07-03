### Atlantic-Hurricane-Season-Forecasting

**btravis**

#### Executive Summary
Objective: Build a predictive model to forecast an incoming hurricane season in the Atlantic basin. Target variables to include number of named storms, number of hurricanes, number of major hurricanes, and landfalling hurricanes in a given season (June 1st to November 30th) given prevailing climatological conditions at the beginning of the hurricane season or some other time (e.g. monthly forecasts).

#### Rationale
A common problem in hurricane forecasting is making beginning of season predictions on the number of hurricanes that will occur in the Atlantic basin. For example, universities, private institutions and government agencies like the National Oceanic and Atmospheric Administration (NOAA) publish “hurricane season outlooks” that look at meteorological and ocean conditions and attempt to predict the number of hurricanes. Many of these forecasts also predict how many major hurricanes (based on storm strength) or landfalling hurricanes (hurricanes that hit the mainland United States) will occur.

#### Data Sources
Hurricane/named storm information: hurdat2 dataset
Oceanic Nino Index from NOAA
Sea temperature and other data from the National Data Buoy Center (NDBC) | NOAA

#### Methodology
Regression: Linear Regression and Regression with R1 and R2 Regularization

#### Results
- Encountered challenges with overfitting on the seasonal forecasts (negative test r^2)
- Idenfitied some interesting observations as to the most important features in the monthly data
-   Oceanic Nino Index (ONI) negatively correlated with hurricane/storm activity, as expected (see notebook), while water temperature positively correlated

#### Next steps
- Add additional features (data): additional buoy data from NDBC, air temperature data, ...
- Interpolate for missing buoy data so that we can fit on more samples
- Fit model on different target variables (Accumulated Cyclone Energy, etc.)
- Monthly forecasting rather than full-season forecasts, so that we have 12x the data (12 samples per year rather than just 1)

#### Outline of project

- [season_forecast_draft1.ipynb](https://github.com/b-travis/Atlantic-Hurricane-Season-Forecasting/blob/main/season_forecast_draft1.ipynb)
- [https://github.com/b-travis/Atlantic-Hurricane-Season-Forecasting/blob/main/monthly_forecast_draft1.ipynb](https://github.com/b-travis/Atlantic-Hurricane-Season-Forecasting/blob/main/monthly_forecast_draft1.ipynb)


##### Contact and Further Information
