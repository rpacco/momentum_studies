<h1 align="center">
    Momentum Studies on Brazilian market 🚀🚀
</h1>

## 📕About
<div style="text-align: justify">
The intention of this work is to test momentum trading in Brazil. That will be
performed through an exploratory data analysis and backtests of momentum criteria
strategies.

The first one, <a name="ancora">absolute momentum strategy</a>, uses a regime filter that allows to hold/sell Brazilian index, which is a positive X months (X to be defined on studies) lookback of its own returns (IBOVESPA, from B3). 
The second one, <a name="ancora2">relative momentum strategy</a>, consists in a calculation to be performed with exponential regression on all Brazilian index securities, and having all stocks ranked by their regression slope times its R2 to create a momentum coefficient. Then, it is built a portfolio of the first 5 stocks of the momentum ranking.


## 🔎Methodology
### <a id="ancora">Absolute momentum</a>
On this study adapting Gary methodology, it was used data from January, 2003 until December, 2020.
From that, it could be performed the X months lookback returns for all our data, in a monthly basis. Then, it was performed calculations ranging from 2 to 12 months lookback returns. For example, a 2 month lookback return calculation:
$$
return_{2monthsLB} = \frac{S_{t}}{S_{t-2}}
$$

where:

$return_{2monthsLB}$ = return of a stock on a 2 month lookback period

$S_{t}$ = stock price at current time

$S_{t-2}$ = stock price 2 months ago

In case $return_{2monthsLB} > 0$, the investor should become/stay long the Brazilian index (IBOVESPA) for the next month. Otherwise, the investor should liquidate its position for the next month.

### <a id="ancora2">Relative momentum</a>
The other strategy, adapting Clenow methodology\cite{clenow2015}, consists of performing a monthly exponential regression calculation\cite{scikit-learn} on the last X months returns of all Brazilian index constituents (using historical data of Brazilian index BOVESPA constituints from Microsoft Qlib\cite{qlib} ). Firstly, it is applied a regime filter to consider only constituints which last closing price is above its own 100-day moving average. Then, it is taken into account the last X months adjusted closing price, (X will range from 2 to 6 months lookback of price data from all index stocks from January, 2003 until December, 2020). Then, for instance, in a 2-month lookback regression:

$$
    \begin{bmatrix}
    logS_{1}\\ 
    logS_{2}\\ 
    logS_{3}\\ 
    \vdots\\
    logS_{i}
    \end{bmatrix}= a + b\begin{bmatrix}
    t_{1}\\ 
    t_{2}\\ 
    t_{3}\\ 
    \vdots\\ 
    t_{i}
    \end{bmatrix}
$$

where:

$logS_{i}$ = logarithm of Stock price at day i

$a$ = intercept of the exponential regression

$b$ = coefficient of the exponential regression

$t_{i}$ = day i (last business day in a 2-month cycle)

Also, it will be estimated the $R^{2}$ of each stock regression, in order to take into account the volatility of the stock (higher the $R^{2}$, means more adherence to the regression estimation, which, in this case, means that the lower should be its volatility). For that:
$$
    R^{2} = 1-\frac{\sum (logS_{i} - \hat{logS_{i}})^{2}}{\sum (logS_{i} - \bar{logS})^{2} }
$$

$logS_{i}$ = logarithm of Stock price at day i

$\hat{logS_{i}}$ = logarithm of Stock price at day i estimated by exponential regression

$\bar{logS}$ = mean Stock price logarithm

Finally, knowing $b$ and $R^{2}$ for each stock exponential regression, these two are multiplied, as follows:
$$
    m_{S} = b \times R^{2}
$$

where:

$m_{S}$ = Stock (S) momentum coefficient

Lastly, after performing regression on all stocks of the index, and with its respective $m_{S}$, a ranking can be made and build a portfolio from the top 5 stocks (equally weighted) to be held for the next month period.

## 👨‍💻Data wrangling
Several data sources were used in order to complete the calculation. Regarding <a id="ancora">Absolute momentum</a>, Yfinance python module was able to provide enough data for this specific study.

However, concerning <a id="ancora2">Relative momentum</a>, it was necessary to gather data from Qlib python module (Ibovespa historical stock constituints, updated each 4 months). Stock daily closing prices were wrangled from Yfinance module and, when not found at this source, webscrapping from [Fundamentus](https://www.fundamentus.com.br/) was done. Important to notice that a fraction of the index constituints had no free data available neither on Yfinance module nor at Fundamentus website.

##  🧮Calculation and 🧾Results
- Absolute momentum 💻[Notebook](\abs_momentum.ipynb)
- Relative momentum 💻[Notebook](\rel_momentum.ipynb)

</div>