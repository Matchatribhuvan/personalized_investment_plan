# personalized_investment_plan

This Python script provides an in-depth analysis of stock price data for various companies, with a focus on their volatility, growth potential, and expected returns on investment (ROI). The code uses the pandas library for data manipulation, plotly for visualizations, and statistical techniques to evaluate stock performance over time. Additionally, it compares the risk and potential return of different stocks, helping investors make informed decisions regarding stock selection, portfolio allocation, and investment strategies.

What the Code Does

Data Import and Preprocessing:
The code loads stock price data from a CSV file and converts the 'Date' column to a datetime format.
It checks for missing values and uses forward fill (ffill) to handle any missing data by propagating the last valid observation.

Stock Price Trends Visualization:
A line chart is created for each company's stock price over time using plotly.graph_objs. This provides a clear visual of how the stock prices for various companies have fluctuated over the selected date range.

Volatility Analysis:
The code calculates the standard deviation (volatility) for the stock prices of each company. It then prints the top 10 companies with the highest volatility, which indicates the level of risk associated with each stock.

Growth Analysis:
The percentage change (growth) in stock prices is computed for each company, showing how much each stock's price has changed over the time period. The code calculates and prints the average growth for each company.

Return on Investment (ROI) Calculation:
The script computes the ROI for each company, comparing the initial and final stock prices. It then identifies and prints the top 10 companies with the highest ROI, which indicates the most profitable investments based on historical price changes.

Stock Selection Based on ROI and Volatility:
The code applies a filtering strategy where it selects companies with an ROI greater than the median and volatility lower than the median. This helps in identifying stocks with a good balance of return and risk.

Investment Allocation Based on Volatility:
The script allocates investment proportions to selected stocks based on their inverse volatility (less volatile stocks get a larger proportion of the investment). This is a risk-based strategy to balance the portfolio.

Comparison of Risk and ROI for Different Companies:
Bar charts are generated to compare the volatility and expected ROI for mutual fund companies versus growth-rate companies. This helps to visually assess the risk-return trade-off for these two types of investments.

Expected ROI Comparison:
A similar bar chart is created to compare the expected ROI for mutual fund companies and growth-rate companies, helping investors decide which category to focus on.

Future Value of Monthly Investments:
The script calculates the future value of monthly investments (₹5000) over various time periods (1, 3, 5, and 10 years), assuming the average ROI from selected mutual fund companies. This helps estimate how much a regular investment could grow over time, considering compound interest.

Visualization of Investment Growth:
A line chart is plotted to show how the future value of investments grows over different periods (1 to 10 years), allowing investors to understand the impact of long-term investing in mutual funds.

What is the Use of the Code?

Stock Performance Evaluation: The code helps investors analyze and compare the stock performance of multiple companies over time, assessing volatility, growth, and returns.
Portfolio Optimization: By considering volatility and ROI, the code aids in selecting stocks that align with an investor’s risk tolerance and return expectations.

Investment Strategy: The analysis of historical data, ROI, and volatility supports the creation of data-driven investment strategies, such as risk-based portfolio allocation.
Long-Term Investment Planning: The future value calculations help investors understand how regular investments can grow over time, aiding in retirement planning or wealth accumulation.
Risk and Return Comparison: The code allows investors to compare the risk (volatility) and return (ROI) of different types of investments, such as mutual funds and growth-rate stocks, to determine the most suitable investment strategy.
Concepts That Are Used in the Code

Stock Market Analysis:
The code calculates key metrics like volatility (standard deviation), ROI, and growth to evaluate stocks’ historical performance.
Statistical Methods:

Volatility: Standard deviation of stock prices, indicating the level of risk.
Percentage Change: Used to calculate stock growth over time.
Return on Investment (ROI): Measures the profitability of an investment by comparing the final price to the initial price.

Data Manipulation:
The code uses pandas for data cleaning (handling missing values) and calculation (e.g., ROI, growth, volatility). It also groups and sorts data to identify the best-performing stocks.

Visualization with Plotly:
Line Charts: Used for visualizing stock price trends.
Bar Charts: Display comparisons of volatility and ROI between different groups of companies (mutual funds vs growth-rate stocks).
Scatter Plots: (for stock price trends) to identify patterns visually.

Investment Strategy:
The code applies risk-based allocation using inverse volatility, where stocks with lower volatility receive a higher proportion of the investment.

Compounding: The future value of investments is calculated using the compound interest formula, assuming monthly contributions.

Portfolio Theory:
The code simulates a basic form of portfolio construction, selecting stocks based on ROI and volatility to balance risk and return.

Takeaways

Risk-Return Tradeoff: The code demonstrates the importance of balancing risk (volatility) with return (ROI) in stock selection and portfolio allocation. Volatile stocks might provide high returns but come with higher risk.

Investment Allocation Based on Risk: By allocating investments based on inverse volatility, the code applies a common strategy in portfolio management that prioritizes less risky assets.

Growth Potential: The growth analysis helps investors identify stocks with the highest potential for price appreciation, although high volatility often accompanies high growth.

Long-Term Growth: Regular monthly investments in stocks or mutual funds can compound significantly over time. The code shows that even modest monthly contributions can lead to substantial wealth accumulation in the long term.

Data-Driven Decision Making: The code emphasizes the importance of data analysis in making informed investment decisions, guiding investors towards a balanced portfolio that aligns with their risk tolerance and financial goals.

Overall, the code provides a powerful framework for analyzing stock performance, comparing different investment options, and planning for long-term financial growth through data-driven insights.

code

import pandas as pd
data = pd.read_csv("C:\\Users\\Ravindra\\Downloads\\stocks.csv")
print(data.head())
data['Date'] = pd.to_datetime(data['Date'])
print(data.isnull().sum())
data.fillna(method='ffill', inplace=True)
import plotly.graph_objs as go
import plotly.express as px
fig = go.Figure()
for company in data.columns[1:]:
    fig.add_trace(go.Scatter(x=data['Date'], y=data[company],
                             mode='lines',
                             name=company,
                             opacity=0.5))
fig.update_layout(
    title='Stock Price Trends of All Indian Companies',
    xaxis_title='Date',
    yaxis_title='Closing Price (INR)',
    xaxis=dict(tickangle=45), 
    legend=dict(
        x=1.05,
        y=1,
        traceorder="normal",
        font=dict(size=10),
        orientation="v"
    ),
    margin=dict(l=0, r=0, t=30, b=0),  
    hovermode='x',
    template='plotly_white'
)
fig.show()
all_companies = data.columns[1:]
volatility_all_companies = data[all_companies].std()
print(volatility_all_companies.sort_values(ascending=False).head(10))
growth_all_companies = data[all_companies].pct_change() * 100
average_growth_all_companies = growth_all_companies.mean()
print(average_growth_all_companies.sort_values(ascending=False).head(10))
initial_prices_all = data[all_companies].iloc[0]
final_prices_all = data[all_companies].iloc[-1]
roi_all_companies = ((final_prices_all - initial_prices_all) / initial_prices_all) * 100
print(roi_all_companies.sort_values(ascending=False).head(10))
roi_threshold = roi_all_companies.median()
volatility_threshold = volatility_all_companies.median()
selected_companies = roi_all_companies[(roi_all_companies > roi_threshold) & (volatility_all_companies < volatility_threshold)]
print(selected_companies.sort_values(ascending=False))
selected_volatility = volatility_all_companies[selected_companies.index]
inverse_volatility = 1 / selected_volatility
investment_ratios = inverse_volatility / inverse_volatility.sum()
print(investment_ratios.sort_values(ascending=False))
top_growth_companies = average_growth_all_companies.sort_values(ascending=False).head(10)
risk_growth_rate_companies = volatility_all_companies[top_growth_companies.index]
risk_mutual_fund_companies = volatility_all_companies[selected_companies.index]
fig = go.Figure()
fig.add_trace(go.Bar(
    y=risk_mutual_fund_companies.index,
    x=risk_mutual_fund_companies,
    orientation='h',  # Horizontal bar
    name='Mutual Fund Companies',
    marker=dict(color='blue')
))
fig.add_trace(go.Bar(
    y=risk_growth_rate_companies.index,
    x=risk_growth_rate_companies,
    orientation='h',  
    name='Growth Rate Companies',
    marker=dict(color='green'),
    opacity=0.7
))
fig.update_layout(
    title='Risk Comparison: Mutual Fund vs Growth Rate Companies',
    xaxis_title='Volatility (Standard Deviation)',
    yaxis_title='Companies',
    barmode='overlay',  
    legend=dict(title='Company Type'),
    template='plotly_white'
)
fig.show()
expected_roi_mutual_fund = roi_all_companies[selected_companies.index]
expected_roi_growth_companies = roi_all_companies[top_growth_companies.index]
fig = go.Figure()
fig.add_trace(go.Bar(
    y=expected_roi_mutual_fund.index,
    x=expected_roi_mutual_fund,
    orientation='h',  
    name='Mutual Fund Companies',
    marker=dict(color='blue')
))
fig.add_trace(go.Bar(
    y=expected_roi_growth_companies.index,
    x=expected_roi_growth_companies,
    orientation='h',  
    name='Growth Rate Companies',
    marker=dict(color='green'),
    opacity=0.7
))
fig.update_layout(
    title='Expected ROI Comparison: Mutual Fund vs Growth Rate Companies',
    xaxis_title='Expected ROI (%)',
    yaxis_title='Companies',
    barmode='overlay',  
    legend=dict(title='Company Type'),
    template='plotly_white'
)
fig.show()
import numpy as np
monthly_investment = 5000  # Monthly investment in INR
years = [1, 3, 5, 10]  # Investment periods (in years)
n = 12  # Number of times interest is compounded per year (monthly)
avg_roi = expected_roi_mutual_fund.mean() / 100  # Convert to decimal
def future_value(P, r, n, t):
    return P * (((1 + r/n)**(n*t) - 1) / (r/n)) * (1 + r/n)
future_values = [future_value(monthly_investment, avg_roi, n, t) for t in years]
fig = go.Figure()
fig.add_trace(go.Scatter(
    x=[str(year) + " year" for year in years],
    y=future_values,
    mode='lines+markers',
    line=dict(color='blue'),
    marker=dict(size=8),
    name='Future Value'
))
fig.update_layout(
    title="Expected Value of Investments of ₹ 5000 Per Month (Mutual Funds)",
    xaxis_title="Investment Period",
    yaxis_title="Future Value (INR)",
    xaxis=dict(showgrid=True, gridcolor='lightgrey'),
    yaxis=dict(showgrid=True, gridcolor='lightgrey'),
    template="plotly_white",
    hovermode='x'
)
fig.show()




