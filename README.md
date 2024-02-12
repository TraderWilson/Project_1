![](https://github.com/TraderWilson/Project_1/blob/main/Image/Background.png)

# Deciphering the Dance: Analyzing the Correlation Between BTC & US Stocks

## **Background**

The intersection of traditional investing with cryptocurrencies is ever evolving. The integration of Bitcoin with traditional stock markets has sparked a new wave of investment strategies. Our project explores the relationship between the US stock market and Bitcoin. We leverage historical data to uncover potential correlations to Bitcoin (BTC). Through advanced data analysis techniques and the use of Python libraries like pandas, numpy, and matplotlib, we dissect and analyze the intricacies of this relationship.

We investigate how BTC can be used as a leading indicator and seek patterns that could lead to smarter investment decisions. The project encompasses setting global parameters for analysis, correlation studies between assets, in-depth data exploration, and the application of Monte Carlo simulations to construct efficient portfolios. Additionally, we compare the outcome of a stock portfolio against Bitcoin's performance and backtest trading strategies to evaluate its real-world applicability.

This endeavor not only bridges the gap between finance and data science but also provides skeptical investors with viable alternatives to cryptocurrencies. By revealing how traditional and digital assets may complement or influence each other, our project contributes to a more informed, data-driven approach to investment in today's diverse financial ecosystem

## **File**
* [stocks_list.csv](https://www.nasdaq.com/market-activity/stocks/screener)

## **Objectives**
* **Data Acquisition:** Utilize `yfinance` to fetch historical stock data.
* **Data Exploration:** Employ libraries such as `pandas` and `numpy` for data manipulation and exploration.
* **Visualization:** Use `matplotlib`, `seaborn`, and `hvplot` to create insightful visualizations of comparison with the stock market and BTC.
* **Performance Analysis:** Filter and analyze stocks based on `correlation`/`beta`/`Sharpe Ratio` with BTC,  to verify our hypothesis.

## **Hypothesis**
It is possible to construct a portfolio of traditional stocks that closely emulates the performance characteristics of Bitcoin (BTC) potentially capturing similar substantial returns without direct investment in BTC itself. This premise opens up a unique avenue for investors looking to diversify their portfolios while seeking the high returns historically associated with Bitcoin.

## **How to Run the Project**

### 1. **Install Packages** 
* `pandas` for data manipulation; 
* `numpy` for numerical operations;
* `yfinance` for fetching assets prices;
* `matplotlib` , `seaborn` and `hvplot` for visualization.

### 2. **Set Up Global Parameters**
* In this initial phase, we establish the foundation for our analysis by defining global parameters. These parameters include the date range for our data, the list of stock symbols to analyze alongside Bitcoin, and any financial metrics of interest (e.g., closing price, volume). This step is crucial as it ensures that all subsequent analyses operate under a consistent set of assumptions and data scope. 
* Due to the positively skewed distribution of Volume, we first filtered our stocks by using the median of the Volume within our yfinance historical stock data. By using the median of Volume this allowed us to filter out the stocks that have little to no liquidity, to give us a more practical meaningful dataset. 

```python
filter_stocks = full_stocks[full_stocks['Volume'] >= full_stocks['Volume'].quantile(0.5)]
```
### 3. **Data Exploration**
* This section is dedicated to a thorough examination of the dataset, employing statistical analyses and visualizations to uncover the relationship with BTC and stocks, such as correlation, beta and Sharpe Ratio.
* According to the correlation_matrix, beta and btc_sharpe_ratio, and using statistical analysis of returns, including volatility and average returns, we construct 5 portfolios to see how their performances are relative with BTC, which will be shown in part 5. 
```python
correlation_matrix['BTC'].hvplot.hist(bins = 100, xlabel = 'Correlation', title = "Distribution of Assets' Correlation with BTC")

beta.hvplot.hist(bins = 3600, title = 'Distribution of Beta relative with BTC', xlabel = 'Beta', hover_color = 'orange', width = 900)

btc_sharpe_ratio = (btc_avg_rtn - interest_rate) / btc_vol
```
![correlation](https://github.com/TraderWilson/Project_1/blob/main/Image/Distribution%20of%20Assets'%20Correlation%20with%20BTC.png)

* In this part, according to the function `shift_cor`, we also find out that BTC would be a leading indicator within a week, which will be proved in part 6.
```python
def shift_cor(days, daily_rtn, assets):
```
### 4. **Finding the Efficient Frontier Using Monte Carlo Simulations**
* We employ Monte Carlo simulations to map out the efficient frontier for a portfolio that includes selected stocks. This approach involves simulating n_portfolios times of  asset weight combinations to identify those that yield the highest returns for a given level of risk.

* After Monte Carlo Simulation, we use the function `print_portfolio_summary` to get the optimal assets weights for each portfolio.

```python
def print_portfolio_summary(perf, weights, assets, name):
```
### 5. **Comparison with Portfolio and BTC**
* This analysis contrasts the risk and return profiles of a diversified portfolio against Bitcoin alone. By comparing the performance of mixed-asset portfolios to the benchmark (Bitcoin), we can assess the impact of diversification on investment returns.
```python
(port_1_plot * btc_plot).opts(title = 'Cumulative Returns Comparison with Filter 1 and BTC')

(port_2_plot * btc_plot).opts(title = 'Cumulative Returns Comparison with Filter 2 and BTC')

(port_3_plot * btc_plot).opts(title = 'Cumulative Returns Comparison with Filter 3 and BTC')

(port_4_plot * btc_plot).opts(title = 'Cumulative Returns Comparison with Filter 4 and BTC')

(port_5_plot * btc_plot).opts(title = 'Cumulative Returns Comparison with Filter 5 and BTC')
```
!<img src='https://github.com/TraderWilson/Project_1/blob/main/Image/Performance%20Comparison%20with%20Filter%201%20and%20BTC.png' width='250'>

### 6. **Backtesting Trading Strategy**
* Finally, we backtest trading strategies based on historical data to ascertain their potential effectiveness. This involves applying the identified correlations and portfolio allocations to past data to simulate trading performance, offering insights into the strategies' viability in real-world conditions.

* We employ two trading strategies to see how the portfolios performance are relative to BTC. The common part is we set up EMA 8, EMV 20, MA 50 and MA200 moving average for each portfolio. The difference is as follows:
> 1. Portfolio's own price moving average strategy. That means the trading signal comes from portfolio's price itself.
```python
 def strategy_ma(days, df_price, df_rtn):
```
> 2. BTC price moving average strategy. That means the trading signal comes from BTC price, not the portfolio itself. As mentioned in part 3, there is a hint that BTC would be a leading indicator within a week.
```python
def strategy_ma_2(days, df_price, df_rtn, btc_df):
```
* In this part, we prove that BTC is a short term leading indicator, but not for long term.

## **Conclusion**
Our project embarked on a mission to uncover if a traditional stock portfolio could emulate the high returns of Bitcoin (BTC) without direct investment in the cryptocurrency. Here are the succinct outcomes:

### **Correlation and Portfolio Optimization:**
Our analysis identified stocks with historical performance correlations to BTC. Through Monte Carlo simulations, we optimized a portfolio that showed a promising degree of correlation to BTC's returns, validating our hypothesis to a certain extent.

### **Performance Comparison and Backtesting:**
Comparing this portfolio to BTC revealed challenges in matching BTC's unique return profile exactly, due to differences in volatility and market behavior. Backtesting provided practical insights, suggesting our approach has merit but also highlighting the complexity of replicating cryptocurrency returns with stocks.

### **BTC as a Short-Term Leading Indicator:**
Through the analysis of 1-7 day shift correlations and backtesting with BTC's moving average signals, we demonstrate that BTC serves as a short-term leading indicator to a notable degree.

## Limitations and Future Research
Acknowledging the limitations of historical data reliance, future research is encouraged to explore dynamic asset selection and include more sophisticated risk management to refine the portfolio construction process.