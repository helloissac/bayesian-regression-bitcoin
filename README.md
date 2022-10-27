# Bayesian Regression of Latent Source Modelling for Predicting Price Variation of Bitcoin
- The Bayesian regression for “latent source model” was used for binary classification. Bayesian regression established theoretical and empirical efficacy of the method for the setting of binary classification.

## Overview
- This project aims to predict the price variations of bitcoin, a virtual cryptographic currency. 
- Implemented the Bayesian regression model to predict the future price variation of bitcoin.
- These predictions could be used as the foundation of a bitcoin trading strategy. 
- To make these predictions, a machine learning technique - Bayesian regression was implemented in Python.

## Logic 
1. Compute the price variations (Δp<sub>1</sub>, Δp<sub>2</sub>, and Δp<sub>3</sub>) for train2 using train1 as input to the Bayesian regression equation. Make sure to use the similarity metric in place of the Euclidean distance in Bayesian regression. If the ratio is > 1, declare y = 1, else declare y = 0. In general, to estimate the conditional expectation of y, given observation x, the estimation is produced as below.
<center>
  <img src="https://github.com/issacjohannli/bayesian-regression-bitcoin/blob/main/equations/equation-1.png" width="950" height="200">
</center>
2. Compute the linear regression parameters (w<sub>0</sub>, w<sub>1</sub>, w<sub>2</sub>, w<sub>3</sub>) by finding the best linear fit (Equation 8). The ols function of statsmodels.formula.api was used. The model should be fit using Δp<sub>1</sub>, Δp<sub>2</sub>, and Δp<sub>3</sub> as the covariates. Note: the bitcoin order book data was not available. The final estimation ∆p is produced as below.
<center>
  <img src="https://github.com/issacjohannli/bayesian-regression-bitcoin/blob/main/equations/equation-2.png" width="950" height="200">
</center>
3. Use the linear regression model computed in Step 2 and Bayesian regression estimates, to predict the price variations for the test dataset. Bayesian regression estimates for test dataset are computed in the same way as they are computed for train2 dataset – using train1 as an input.
4. Once the price variations are predicted, compute the mean squared error (MSE) for the test dataset (the test dataset has 50 vectors => 50 predictions).

## Data
The datasets were saved in the /data folder. The original raw data can be found here:\
http://api.bitcoincharts.com/v1/csv/  \
The datasets from this site have three attributes:
1. Time in epoch,
2. Price in USD per bitcoin
3. Bitcoin amount in a transaction (buy/sell).

However, only the first two attributes were relevant to this project.

- To make the data to have evenly space records, all the records were taken within a 20 second window and replaced it by a single record as the average of all the transaction prices in that window. Not every 20 second window had a record; therefore those missing entries were filled using the prices of the previous 20 observations and assuming a Gaussian distribution. The raw data that has been cleaned was given in the file dataset.csv.

- Finally, the data was divided into a total of 9 different datasets. The whole dataset was partitioned into three equally sized (50 price variations in each) subsets: train1, train2, and test. The train sets were used for training a linear model, while the test set was for evaluation of the model. There were three csv files associated with each subset of data: *_90.csv, *_180.csv, and *_360.csv. In _90.csv, for example, each line represented a vector of length 90 where the elements are 30 minute worth of bitcoin price variations as there were 20 second intervals and a price variation in the 91st column. Similarly, the *_180.csv represented 60 minutes of prices and *_360.csv represented 120 minutes of prices.
