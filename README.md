# A Whale Off the Port(folio)
<img src="https://user-images.githubusercontent.com/83671629/120809591-3be9d800-c518-11eb-9a0a-b017d3558984.jpg" width="800" height="500">


## Creating a tool to analyze and visualize major metrics of the portfolio across multiple areas such as volatility, returns, risk and Sharpe ratios. 

This tool analyzes and visualizes portfolios to determine which portfolio outperformed the others. Using some of the historical daily returns of several portfolios such as the firm's algorithmic portfolio, some that represent the portfolios of famous "whale" investors like Warren Buffett, and some from the big hedge and mutual funds, I created a custom portfolio of stocks to compare its performance to that of other portfolios and to larger market [(S&P TSX)]( https://en.wikipedia.org/wiki/S%26P/TSX_60)

This tool performs three main tasks:
 
1. [Reads in and Wrangle Returns Data](#Prepare-the-Data)

2. [Determines Success of Each Portfolio](#Conduct-Quantitative-Analysis)

3. [Chooses and Evaluates a Custom Portfolio](#Create-a-Custom-Portfolio)


**Files:**

* [Whale Analysis Code](/Portfolio/whale_analysis.ipynb)
* [algo_returns.csv](algo_returns.csv)
* [otex_historical.csv](otex_historical.csv)
* [sp_tsx_history.csv](sp_tsx_history.csv)
* [l_historical.csv](l_historical.csv)
* [shop_historical.csv](shop_historical.csv)
* [whale_returns.csv](whale_returns.csv)
* [bce_historical.csv](bce_historical.csv)
* [bns_historical.csv](bns_historical.csv)

### Preparing the Data

Reading and cleaning all CSV files for analysis. The CSV files included are whale portfolio returns, algorithmic trading portfolio returns, and S&P TSX 60 Index historical prices. 
To prep and clean the data I used the starter code file [Whale Analysis Code](whale_analysis.jpynb) and completed the following:

1. Using Pandas `read_csv` I read the following CSV files as a DataFrame and converted the dates to a `DateTimeIndex` format.

    * `whale_returns.csv`: Contains returns of some famous "whale" investors' portfolios.

    * `algo_returns.csv`: Contains returns from the in-house trading algorithms from Harold's company.

    * `sp_tsx_history.csv`: Contains historical closing prices of the S&P TSX 60 Index.

2. Detected and removed null values. 

3. In `sp_tsx_history.csv` file the `close`column had dollar signs and commas, removed those characters and converted the data type to object using panda `str.replace`.

4. Calculated the `S&P TSX 60` closing prices to daily returns using pandas `pct_change` command.

5. Joined `Whale Returns`, `Algorithmic Returns`, and the `S&P TSX 60 Returns` into a single DataFrame `joined_portfolio` using the `concat` command.

    
### Conduct Quantitative Analysis

Analyzing the data to see if any of the portfolios outperformed the stock market (i.e., the S&P TSX 60).

#### Performance Analysis

1. Calculated and plotted daily returns of `joined_portfolio` using `plot` command .

2. Calculate and plott cumulative returns for `joined_portfolio` using `.cumprod` command. 

![Graph1](https://user-images.githubusercontent.com/83671629/120839743-4ff10200-c537-11eb-8c44-69d6881b0397.png)

### As per the plotted analysis, other than PAULSON & CO.INC all the other portfolios outperform the S&P TSX. Since beginning of 2019, TIGER GLOBAL HATHAWAY INC has performed below S&P TSX.


#### Risk Analysis

1. The box plot for each of the returns loss as follows.
![graph2](https://user-images.githubusercontent.com/83671629/120839945-98a8bb00-c537-11eb-8b07-402073ae01b0.png)
 
2. Using the pandas command `.std()` I calculated the standard deviation for each portfolio.

			SOROS FUND MANAGEMENT LLC      	0.007828	TRUE
			PAULSON & CO.INC.              	0.006982	False
			TIGER GLOBAL MANAGEMENT LLC    	0.010883	True
			BERKSHIRE HATHAWAY INC         	0.012826	True
			Algo 1                         	0.007589	True
			Algo 2                         	0.008326	True
			S&P TSX                        	0.007034	False



3. From the above output we can determine that the `PAULSON & CO.Inc` portfolio is riskier than the S&P TSX.

4. I calculated the Annualized Standard Deviation on `joined_portfolio` using the following formula

      annual_std_dev_combined = std_dev_joined_portfolio * np.sqrt(252).

#### Rolling Statistics

Risk changes over time. Analyzing the rolling statistics for Risk and Beta helps to determine the changes.

1. I calculated the rolling standard deviation for all portfolios for a 21-day window using the pandas command `.rolling`.

2. I calculated the correlation using the pandas `'.corr()` command between each stock to determine which portfolios may mimic the S&P TSX 60. 

<img src = "https://user-images.githubusercontent.com/83671629/120841684-d3abee00-c539-11eb-9a50-4553a186e20f.png" width="700" hight="700" >
					
						
From observing the plot, It looks like Algo 1 and S&P are very strongly correlated. Algo 2 and S&P are least correlated.

3. I chose `Algo 2 portfolio ` to calculate and plot the 60-day rolling beta for it and the S&P TSX 60.


#### Rolling Statistics Challenge: Exponentially Weighted Average

An alternative method to calculate a rolling window is to take the exponentially weighted moving (ewm) average. This is like a moving window average, but it assigns greater importance to more recent observations. I calculated the `ewm` with a 21-day half-life.


### Sharpe Ratios

Investment managers and their institutional investors look at the return-to-risk ratio, not just the returns. After all, if you have two portfolios that each offer a 10% return, yet one is lower risk, you would invest in the lower-risk portfolio, right?

1. Using the daily returns, I calculated and plotted the Sharpe ratios using a bar plot.
![image](https://user-images.githubusercontent.com/83671629/120842616-ee329700-c53a-11eb-92d9-79555459da99.png)

### It looks like the algorithmic strategies outperformed both the market (S&P TSX 60) and the whales portfolios.

### Create a Custom Portfolio

I am now wondering whether I can choose my own portfolio that performs just as well as the algorithmic portfolios. 

1. I used [Google Sheets](https://docs.google.com/spreadsheets/) and the built-in Google Finance function to choose 2 stocks for my portfolio. I choose [Bell Canada Inc (BCE)]( https://www.google.com/search?q=bell+canada+stock&rlz=1C1CHBF_enCA953CA953&oq=bell+canada+s&aqs=chrome.0.0i433j0j69i57j0l7.12281j0j15&sourceid=chrome&ie=UTF-8) and [Scotio Bank (BNS)](https://www.google.com/search?q=scotiabank+ticker&rlz=1C1CHBF_enCA953CA953&oq=scoti&aqs=chrome.0.69i59j69i57j69i59j0i67j0i67i131i433j46i131i199i291i433j46i67i175i199j0i433i457j0i402l2.3582j0j15&sourceid=chrome&ie=UTF-8)

2. Along with BNS and BCE, I used the three stocks provided, [Loblaw(L)](https://www.google.com/search?q=loblaws+stock&rlz=1C1CHBF_enCA953CA953&sxsrf=ALeKk02GaUhzT2zXLbzDxTL3aHDWi6ZxdQ%3A1622831578271&ei=2nG6YLKGEJr0tAb39bigBg&oq=lo&gs_lcp=Cgdnd3Mtd2l6EAMYADIMCCMQJxCdAhBGEPoBMgQIIxAnMgQIIxAnMgQIABBDMgQIABBDMgQIABBDMgQIABBDMgQIABBDMgoILhDHARCvARBDMgQIABBDUPacCFjHnghgw7MIaABwAngAgAHDBogB-giSAQcwLjIuNi0xmAEAoAEBqgEHZ3dzLXdpesABAQ&sclient=gws-wiz) , [Shopify(SHOP)](https://www.google.com/search?q=shopify+stock&rlz=1C1CHBF_enCA953CA953&oq=shopf&aqs=chrome.1.69i57j0i10i131i433j0i10i433j0i10i131i433l3j0j0i10i433j0i10l2.4136j0j15&sourceid=chrome&ie=UTF-8) and Open [Text Corp (OTEX)](https://www.google.com/search?q=otex+stock&rlz=1C1CHBF_enCA953CA953&oq=otex&aqs=chrome.1.69i57j69i59j0i67l3j0l5.2789j0j15&sourceid=chrome&ie=UTF-8)


3. I downloaded the data as CSV files, combined them into a portfolio and calculated the portfolio returns of all 5 stocks.

3. I Calculated the weighted returns for my portfolio, assuming an equal number of shares per stock (1/5).

4. I added my portfolio returns to the DataFrame with the other portfolios (Whale, Algo and S&P).

5. I ran the following analyses:

    * Calculated the Annualized Standard Deviation.
    * Calculated and plotted rolling `std` with a 21-day window.
    * Calculated and plotted the correlation.
    * Calculated and plotted the 60-day rolling beta for my portfolio compared to the S&P 60 TSX.
    * Calculate the Sharpe ratios and generate a bar plot.
    
![my_port](https://user-images.githubusercontent.com/83671629/120848762-30f86d00-c543-11eb-8bc4-72036f49d279.png)

## My Portfolio outperformed Whale, Algo 2 and the market. Algo 1 outperformed my Portfolio.

![suce](https://user-images.githubusercontent.com/83671629/120849092-9ea49900-c543-11eb-9372-4a8bb91cd962.jpg)



## Resources

* [Pandas API Docs](https://pandas.pydata.org/pandas-docs/stable/reference/index.html)

* [Exponential weighted function in Pandas](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.ewm.html)

* [`GOOGLEFINANCE` function help](https://support.google.com/docs/answer/3093281)

* [Supplemental Guide: Fetching Stock Data Using Google Sheets](../../../01-Lesson-Plans/04-Pandas/Supplemental/googlefinance_guide.md)
