## Quantitative Investment Research & Strategy


## Usage Instructions

### FRED API KEY

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


### Install New Libraries

Two new libraries have been introduced for this project:  Pyfolio and Yahoo! Market Data Downloader (Select the hyperlink that will direct you to the full installation process.)

See [Requirements](Requirements.txt) for a full list of libraries that are required.  

Note that the Yahoo! Finance market data downloader should already be included as part of the standard libraries however this can be confirmed manually or through the installation process.

1. [pip install pyfolio-reloaded](https://pypi.org/project/pyfolio-reloaded/)

> pyfolio is a Python library for performance and risk analysis of financial portfolios

> At the core of pyfolio are various tear sheets that combine various individual plots and summary statistics to provide a comprehensive view of the performance of a trading algorithm.

> The tear sheet presents performance and risk metrics for the strategy separately during the backtest and out-of-sample periods

> *NB:  This library has known issues and only a subset of the functionality has been used.  The only approach to address this issue requires the user to change the source code in severaly modules, which is not best practice.  [pyfolio IndexError: index -1 is out of bounds for axis 0 with size 0 #661](https://github.com/quantopian/pyfolio/issues/661)  The benefits of using this library outweight this issue, as only the test period was of primary concern, although it is ideal to have the in-sample and out-of-sample results available.*

2. [pip install yfinance](https://pypi.org/project/yfinance/)

> Yahoo! Finance market data downloader

> Ever since Yahoo! finance decommissioned their historical data API, many programs that relied on it to stop working.

> yfinance aims to solve this problem by offering a reliable, threaded, and Pythonic way to download historical market data from Yahoo! finance.


Describe FRED API Process (Obtaining, installing & Saving key to text)

Describe pyfolio (Installing)

Describe yahooFinance (Should be part of standard libary but good to confirm)




AggregatedStatistics.ipynb


Current_rfc_model_algo_optimal_params_all_avg.ipynb