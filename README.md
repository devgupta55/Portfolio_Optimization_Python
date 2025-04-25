# 📈 Portfolio Optimization Case Study

In this case study, we explore how to construct and optimize an investment portfolio of four selected stocks using historical stock price data. The goal is to compare a simple **equal-weighted portfolio** to an **optimized portfolio** based on return-to-risk trade-offs.
This project was completed as part of the **Business Intelligence & Data Analyst (BIDA) Certification** by the Corporate Finance Institute (CFI). It demonstrates core skills in data analysis, portfolio optimization, and financial performance evaluation using Python.

---

## 🚀 Objectives

We aim to build two $10,000 portfolios using four stocks: `AMD`, `AAPL`, `MSFT`, and `ORCL`.

- ✅ Import 2 years of historical data for the selected stocks  
- ✅ Construct an **equal-weighted portfolio**  
- ✅ Analyze and **visualize performance** of the equal-weighted portfolio  
- ✅ Calculate performance metrics (returns, volatility, Sharpe Ratio)  
- ✅ Simulate **10,000 random portfolios** with varied weights  
- ✅ Find and visualize the **optimal portfolio** (based on Sharpe Ratio and/or volatility)  

---

## 🧰 Tools & Libraries

- Python 3.x  
- pandas  
- numpy  
- matplotlib  

---

## 📦 Data Source

- CSV files for each stock:  
  - `AMD.csv`  
  - `AAPL.csv`  
  - `MSFT.csv`  
  - `ORCL.csv`  
- Each file includes `Date` and `Adjusted Close` columns.

---

## 📊 Step-by-Step Breakdown

### 1. Import Packages & Connect to Data

We read in CSV files for all four stocks, using the `Date` column as the index. The `Adj Close` column is used to compute returns.

📌 **Code Snippet:**
```python
# Import packages needed for case study
  import pandas as pd
  import numpy as np
  import matplotlib.pyplot as plt
  %matplotlib inline
```
```python
# List the four stock ticker symbols for our portfolio
stock_list = ['AMD', 'AAPL', 'MSFT', 'ORCL']

# Create an empty dictionary to store our stock info
stocks = {}

# Loop through each stock in the stock_list
for i_stock in stock_list:
    stocks[i_stock] = pd.read_csv(str(i_stock + '.csv'), parse_dates=True, index_col = 'Date')
```
```python
# Examine the 'AMD' Adj Close from the stocks dictionary
stocks['AMD'].head()
```
📺**Output**<br>
![image](https://github.com/user-attachments/assets/4ad8de82-8e2e-4f97-8a0a-365f9904862e)

### 2. Create the Equal-Weighted Portfolio

To create the equal-weighted portfolio, we need to add some additional columns to the DataFrames in the `stocks` dictionary. The three columns that we will build are:

- **Normalized Return** = Adjusted Close / Adjusted Close on the `startdate` of the portfolio  
- **Allocation** = Normalized Return * 0.25 (equal weighting for each of the four stocks)  
- **Position Value** = Allocation * 10,000 (value of the portfolio)

📌 **Code Snippet:**
```python
# Create 'Normalized Return' column for each stock
for stock_name, stock_data in stocks.items():
  first_adj_close = stock_data.iloc[0]['Adj Close'] # Select the first row from the Adj Close column
  stock_data['Normalized Return'] = stock_data['Adj Close'] / first_adj_close
```
```python
stocks['AAPL'].head()
```
📺**Output**<br>
![image](https://github.com/user-attachments/assets/cce068df-e3f4-41d8-9658-2fe9a98a1276)

```python
# Create allocation for each stock - equally weighted in our initial portfolio
for stock_name, stock_data in stocks.items():
    stock_data['Allocation'] = stock_data['Normalized Return'] * 0.25
```
```python
stocks['MSFT'].head()
```
📺**Output**<br>
![image](https://github.com/user-attachments/assets/cfded6d1-8a80-40ac-81f6-85d61de8c26e)

```python
# Set the value of the portfolio to $10k
for stock_name, stock_data in stocks.items():
  stock_data['Position Value'] = stock_data['Allocation'] * 10000
```
```python
stocks['ORCL'].head()
```
📺**Output**<br>
![image](https://github.com/user-attachments/assets/1491dbc8-bf14-4ed8-8aee-d8caa272079b)


### 3. Visualize the Portfolio Performance

To visualize the performance of the portfolio, we can create two line charts that show the return of the portfolio, and the return of the individual stocks, over time.

- Let's build a new DataFrame that contains just the `position value` for each stock, as well as the total value for the portfolio. 
- We can use this DataFrame to create the two visuals.

📌 **Code Snippet:**
```python
# Create position_values dictionary
position_values = {}

for stock_name, stock_data in stocks.items():
  position_values[stock_name] = stock_data['Position Value']
```
```python
# Convert the position_values dictionary to a DataFrame
 position_values = pd.DataFrame(data=position_values)

 position_values.head()
```

📺**Output**<br>
![image](https://github.com/user-attachments/assets/3b87949d-05b8-413e-997c-7f19f032d2ba)

```python
# Add 'Total' column to position values, summing the other columns
position_values['Total'] = position_values.sum(axis=1)
```

```python
position_values.head()
```
📺**Output**<br>
![image](https://github.com/user-attachments/assets/62824903-3012-4011-bc91-3aa74ea5b539)

```python
# View the total portfolio
plt.figure(figsize=(12, 8))

plt.plot(position_values['Total'])

plt.title('Equal-Weighted Portfolio Performance')
plt.ylabel('Total Value');
```

📺**Output**<br>
<img src = "https://github.com/user-attachments/assets/50bef4b0-59dd-43d2-9dc8-44e88ec993cc" width = "600">

```python
# View the four stocks in the portfolio
plt.figure(figsize=(12, 8))

plt.plot(position_values.iloc[:,0:4])

plt.title('Equal-Weighted Portfolio Stock Performance')
plt.ylabel('Total Value');
```
📺**Output**<br>
<img src = "https://github.com/user-attachments/assets/ea7a2196-a7f8-49ef-8273-7de53210d4e1" width = "600">

### Calculate Performance Metrics for the Portfolio
Now that we have created and visualized the equal-weighted portfolio, we can calculate a few metrics to further measure the performance of the portfolio. We will create five performances metrics:
 - Cumulative Return
 - Mean Daily Return
 - Standard Deviation Daily Return
 - Sharpe Ratio
 - Annualized Sharpe Ratio

```python
# Define the end and start value of the portfolio
end_value = position_values['Total'][-1]
start_value = position_values['Total'][0]

# Calculate the cumulative portfolio return as a percentage
cumulative_return = end_value / start_value - 1

print(str(round(cumulative_return*100,2)), '%')
```

```python
# Create a 'Daily Returns' column
position_values['Daily Return'] = position_values['Total'].pct_change()

position_values.head()
```

📺**Output**<br>
![image](https://github.com/user-attachments/assets/dcfafda9-8551-4322-9946-498fe5a639e0)

```python
# Calculate the mean Daily Return 
mean_daily_return = position_values['Daily Return'].mean()

print('The mean daily return is:', str(round(mean_daily_return, 4)))
```
```
The mean daily return is: 0.0022
```

```python
# Calculate the standard deviation of Daily Return 
std_daily_return = position_values['Daily Return'].std()

print('The std daily return is:', str(round(std_daily_return, 4)))
```

```
The std daily return is: 0.0207
```

## 🔗 Access the Full Code

To explore the complete implementation and analysis, [click here to view the full Jupyter Notebook](./Portfolio_Optimization_Python.ipynb) in this repository.

---






