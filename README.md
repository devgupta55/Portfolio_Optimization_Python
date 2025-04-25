# 📈 Portfolio Optimization Case Study

In this case study, we explore how to construct and optimize an investment portfolio of four selected stocks using historical stock price data. The goal is to compare a simple **equal-weighted portfolio** to an **optimized portfolio** based on return-to-risk trade-offs.

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
  # stocks['MSFT'].head()
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
![image](https://github.com/user-attachments/assets/b859144a-3161-4646-8abd-0d0dcbb9bc24)

