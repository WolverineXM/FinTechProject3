## Quantitative Investment Research & Strategy Develpment

### Project Hypothesis

Existing emperical evidence exists describing the relationship between [Credit Risk/Spreads](https://www.investopedia.com/terms/c/creditspread.asp) and equity markets.  For the purpose of this project, the the [SPDR® S&P 500® ETF Trust](https://www.ssga.com/us/en/intermediary/etfs/funds/spdr-sp-500-etf-trust-spy) was selected as a proxy for the U.S. Equity Market.  A paper published by the Bank for International Settlements (BIS), [Explaining Credit Default Swap Spreads with Equity Volatility and Jump Risks of Individual Firms](https://www.bis.org/publ/work181.pdf) concluded:

>>> A structural model with stochastic volatility and jumps implies particular relationships between observed equity returns and credit spreads. 

>>> Their empirical results suggest that volatility risk alone predicts 50% of CDS spread variation, while jump risk alone forecasts 19%.

In addition, the [Merton Model](https://www.investopedia.com/terms/m/mertonmodel.asp), which is an analysis model used to assess the credit risk of a company's debt, also finds a relationship between credit spreads and equity valuation levels. Analysts and investors utilize the Merton model to understand how capable a company is at meeting financial obligations, servicing its debt, and weighing the general possibility that it will go into credit default.

In 1974, economist Robert C. Merton proposed this model for assessing the structural credit risk of a company by modeling the company's equity as a call option on its assets. This model was later extended by Fischer Black and Myron Scholes to develop the Nobel-prize winning Black-Scholes pricing model for options.

> In short, the model predicts a non-linear negative link between the default likelihood and asset value of a firm.

Given the cost of obtaining Credit Default Swap data (preferred data but cost prohibitive), the use of Option Adjusted Spreads (OAS) is explored.  In addition, it is hypothesized that the use of a Machine Learning algorithm can be used to predict future equity market states.  See [Option-Adjusted Spread (OAS)](https://www.investopedia.com/terms/o/optionadjustedspread.asp) for a basic refresher on OASs.

The Random Forest Classifier Model and RandomizedSearchCV are used to develop a Naive Model and Optimal Model to determine if either model can deliver an annual return higher than the [SPDR® S&P 500® ETF Trust](https://www.ssga.com/us/en/intermediary/etfs/funds/spdr-sp-500-etf-trust-spy).  The Optimal Model also attempts to achieve a superior annual rate of return by using model parameters obtained from RandomizedSearchCV (process described below).

### Project Notebooks

[SourceData.ipynb](SourceData.ipynb)

This notebook retrieves data for both the feature and target sets:

The data for the Feature Set is obtained from [FRED](https://fredhelp.stlouisfed.org/fred/about/about-fred/what-is-fred/) using their API services and library named `from full_fred.fred import Fred`.  See the instructions within the "Usage Instructions" section below on obtaining and saving the API from FRED.

The Feature Set Variable Names & FRED Mnemonic:
1. [ICE BofA US High Yield Index Option-Adjusted Spread (BAMLH0A0HYM2)](https://fred.stlouisfed.org/series/BAMLH0A0HYM2)
2. [ICE BofA US Corporate Index Option-Adjusted Spread (BAMLC0A0CM)](https://fred.stlouisfed.org/series/BAMLC0A0CM)
3. [ICE BofA BBB US Corporate Index Option-Adjusted Spread (BAMLC0A4CBBB)](https://fred.stlouisfed.org/series/BAMLC0A4CBBB)
4. [ICE BofA BB US High Yield Index Option-Adjusted Spread (BAMLH0A1HYBB)](https://fred.stlouisfed.org/series/BAMLH0A1HYBB)
5. [ICE BofA CCC & Lower US High Yield Index Option-Adjusted Spread (BAMLH0A3HYC)](https://fred.stlouisfed.org/series/BAMLH0A3HYC)

Of note, this notebook also obtains data for other economic time series, but only OAS data for the above data series are used for modeling purposes.

The data for the Target Set data is obtained from yahoo Finance using their `import yfinance as yf` library.  As noted above, the [SPDR® S&P 500® ETF Trust](https://www.ssga.com/us/en/intermediary/etfs/funds/spdr-sp-500-etf-trust-spy) is the target set.


[rfc_model_feature_set_analysis.ipynb](rfc_model_feature_set_analysis.ipynb)

This note book is used for data visualization and analysis of Feature Set variables to gain an understanding of their relationship with one another and how they may impact the quality of modeling results.  Analysis concludes that the daily rate of return for the OAS time series should be used for modeling, as the linear relationship that appears for the OAS Levels data is removed.  

A simple line plot confirms that some relationship exists between OAS levels and this relationship is likely time dependent as well.  The OAS levels are transformed into daily percentage changes and these values are used for modeling purposes.

The Pearson Correlation Coefficient was used to confirm that the correlation between the daily percentage change in OAS is less than the correlation for OAS levels.

The hvplot.scatter_matrix() method was used to also confirm that this apparent relationship between OAS levels is removed when transforming them into daily percentage changes.

An analysis on the distribution (density plot) for the OAS Levels and Daily Percentage Changes using hvplot.kde() also concluded that daily percentage changes were more appropriate for modeling purposes.

An understanding of the disperion of OAS levels and rates of change was gained through the use of hvplot.violin().

hvplot.lag_plot() was used to gain an understanding of any lagged relationships between the feature set variables.

Finally, an analysis of the two features exhibiting the highest degree of positive correlation was conducted using .hvplot.bivariate()

[rfc_model_target_feature_set_lag_analysis.ipynb](rfc_model_target_feature_set_lag_analysis.ipynb)

Data visualization for lagged analysis between feature set and target set 

[Current_rfc_model_optimal_lag_grid_automated.ipynb](Current_rfc_model_optimal_lag_grid_automated.ipynb)

iterates over list of lagged feature set to produce Naive results and determine optimal parameter for lagged feature

[Current_rfc_model_algo_optimal_params_all_avg.ipynb](Current_rfc_model_algo_optimal_params_all_avg.ipynb)

uses the mean optimal parameters from Current_rfc_model_optimal_lag_grid_automated.ipynb and model is new training and testing occurs for the October 1, 2021 to October 15, 2021 period based on the optimal parameters

[AggregatedStatistics.ipynb](AggregatedStatistics.ipynb)

Based on the results from the October 1, 2021 to October 15, 2021 training and testing periods, the Mean Annualized Return Per Feature Lag In Days was calculated for the Naive Model and the lagged period with the highest average return was selected for Forward Testing purposes.  The standard deviation and skew are also calculated for future use.

This notebook also calculates the mean parameter values accross all lags to arrive at mean parameter values used for the Optimal Model.  Based on the October 1, 2021 - October 15, 2021 analysis, the paramter values used for the Optimal Model were:

n_estimators          477.0
min_samples_split      31.0
max_features            3.0
max_depth            2988.0

[Current_rfc_optimal_model_accuracy_feature_importance.ipynb](Current_rfc_optimal_model_accuracy_feature_importance.ipynb)

Optimal Model Version that also allows for the computation of training & testing accuracy scores, feature importance (Gini Importance or Mean Decrease in Impurity (MDI))

[nieve_vs_random_grid_search_backtest_comparison.ipynb](nieve_vs_random_grid_search_backtest_comparison.ipynb)

Compares the testing period return path for the Optimal Model, Naive Model, & Equity Secuity (SPY).  Initial development of capture statistics for both versions of the model.  

[rfc_model_naive_forward_testing_v0004.ipynb](rfc_model_naive_forward_testing_v0004.ipynb)
Forward testing for Naive Model

[rfc_model_optimal_forward_testing_v0004.ipynb](rfc_model_optimal_forward_testing_v0004.ipynb)
Forward testing for Optimal Model

### Usage Instructions

#### FRED API KEY

The FRED API is used to obtain Option Adjusted Spread data, which represents the feature set data for model purposes.  The following steps outline the process for obtaining the required API key and saving for use in the FRED library.

1. Sign-Up for a free account and API Key at [St. Loius Fed User Account & API Key Request](https://research.stlouisfed.org/useraccount/apikey)

>>> What is FRED? Short for Federal Reserve Economic Data, FRED is an online database consisting of hundreds of thousands of economic data time series from scores of national, international, public, and private sources. FRED, created and maintained by the Research Department at the Federal Reserve Bank of St. Louis, goes far beyond simply providing data: It combines data with a powerful mix of tools that help the user understand, interact with, display, and disseminate the data. In essence, FRED helps users tell their data stories. The purpose of this article is to guide the potential (or current) FRED user through the various aspects and tools of the database.

2. Record the API Key obtained in step 1 within a text file named `FRED_API_KEY.txt` and save this file within the same folder as the jupyter lab code.


![](images/fred_api_save_location.PNG)


3. Install [full_fred](https://pypi.org/project/full-fred/) using the following command within your environment:  `pip install full-fred`
![](images/pip_install_full_fred.png)

>>> full_fred is a Python interface to FRED (Federal Reserve Economic Data) that prioritizes user preference, flexibility, and speed. full_fred's API translates to Python every type of request FRED supports: each query for Categories, Releases, Series, Sources, and Tags found within FRED's web service has a method associated with it in full_fred. full_fred minimizes redundant queries for the sake of users and FRED's servers. After a request for data is made to FRED web service the retrieved data is stored in a dictionary, accessible and fungible

4. [Full Fred Module Dependencies](https://github.com/7astro7/full_fred/blob/master/requirements.txt)

The following block of code with notebook [SourceData.ipynb](SourceData.ipynb) can be used to confirm the successful intallation of full_fred:

![The following block of code confirms successfull module installation](images/fred_api_dependency.PNG)


#### Pyfolio Library

1. [pip install pyfolio-reloaded](https://pypi.org/project/pyfolio-reloaded/)

> pyfolio is a Python library for performance and risk analysis of financial portfolios

> At the core of pyfolio are various tear sheets that combine various individual plots and summary statistics to provide a comprehensive view of the performance of a trading algorithm.

> The tear sheet presents performance and risk metrics for the strategy separately during the backtest and out-of-sample periods

> *NB:  This library has known issues and only a subset of the functionality has been used.  The only approach to address this issue requires the user to change the source code in severaly modules, which is not best practice.  [pyfolio IndexError: index -1 is out of bounds for axis 0 with size 0 #661](https://github.com/quantopian/pyfolio/issues/661)  The benefits of using this library outweight this issue, as only the test period was of primary concern, although it is ideal to have the in-sample and out-of-sample results available.*

#### yFinance

1. [pip install yfinance](https://pypi.org/project/yfinance/)

> Yahoo! Finance market data downloader

> Ever since Yahoo! finance decommissioned their historical data API, many programs that relied on it to stop working.

> yfinance aims to solve this problem by offering a reliable, threaded, and Pythonic way to download historical market data from Yahoo! finance.


#### Full Library Requirements

Refer to the [Requirements](Requirements.txt) for the all the liraries required to execute the notebooks.
