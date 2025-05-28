**Stock Market Price Analysis App**
# This Python app allows users to analyze historical stock price data for any publicly traded company using data from Yahoo Finance.



```python
!pip install yfinance matplotlib pandas seaborn

import yfinance as yf
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# to make plots look nicer
sns.set(style='darkgrid')
```

    Requirement already satisfied: yfinance in c:\users\veron\anaconda3\lib\site-packages (0.2.61)
    Requirement already satisfied: matplotlib in c:\users\veron\anaconda3\lib\site-packages (3.8.0)
    Requirement already satisfied: pandas in c:\users\veron\anaconda3\lib\site-packages (2.1.4)
    Requirement already satisfied: seaborn in c:\users\veron\anaconda3\lib\site-packages (0.12.2)
    Requirement already satisfied: numpy>=1.16.5 in c:\users\veron\anaconda3\lib\site-packages (from yfinance) (1.26.4)
    Requirement already satisfied: requests>=2.31 in c:\users\veron\anaconda3\lib\site-packages (from yfinance) (2.31.0)
    Requirement already satisfied: multitasking>=0.0.7 in c:\users\veron\anaconda3\lib\site-packages (from yfinance) (0.0.11)
    Requirement already satisfied: platformdirs>=2.0.0 in c:\users\veron\anaconda3\lib\site-packages (from yfinance) (3.10.0)
    Requirement already satisfied: pytz>=2022.5 in c:\users\veron\anaconda3\lib\site-packages (from yfinance) (2023.3.post1)
    Requirement already satisfied: frozendict>=2.3.4 in c:\users\veron\anaconda3\lib\site-packages (from yfinance) (2.4.2)
    Requirement already satisfied: peewee>=3.16.2 in c:\users\veron\anaconda3\lib\site-packages (from yfinance) (3.18.1)
    Requirement already satisfied: beautifulsoup4>=4.11.1 in c:\users\veron\anaconda3\lib\site-packages (from yfinance) (4.12.2)
    Requirement already satisfied: curl_cffi>=0.7 in c:\users\veron\anaconda3\lib\site-packages (from yfinance) (0.11.1)
    Requirement already satisfied: protobuf>=3.19.0 in c:\users\veron\anaconda3\lib\site-packages (from yfinance) (3.20.3)
    Requirement already satisfied: websockets>=13.0 in c:\users\veron\anaconda3\lib\site-packages (from yfinance) (15.0.1)
    Requirement already satisfied: contourpy>=1.0.1 in c:\users\veron\anaconda3\lib\site-packages (from matplotlib) (1.2.0)
    Requirement already satisfied: cycler>=0.10 in c:\users\veron\anaconda3\lib\site-packages (from matplotlib) (0.11.0)
    Requirement already satisfied: fonttools>=4.22.0 in c:\users\veron\anaconda3\lib\site-packages (from matplotlib) (4.25.0)
    Requirement already satisfied: kiwisolver>=1.0.1 in c:\users\veron\anaconda3\lib\site-packages (from matplotlib) (1.4.4)
    Requirement already satisfied: packaging>=20.0 in c:\users\veron\anaconda3\lib\site-packages (from matplotlib) (23.1)
    Requirement already satisfied: pillow>=6.2.0 in c:\users\veron\anaconda3\lib\site-packages (from matplotlib) (10.2.0)
    Requirement already satisfied: pyparsing>=2.3.1 in c:\users\veron\anaconda3\lib\site-packages (from matplotlib) (3.0.9)
    Requirement already satisfied: python-dateutil>=2.7 in c:\users\veron\anaconda3\lib\site-packages (from matplotlib) (2.8.2)
    Requirement already satisfied: tzdata>=2022.1 in c:\users\veron\anaconda3\lib\site-packages (from pandas) (2023.3)
    Requirement already satisfied: soupsieve>1.2 in c:\users\veron\anaconda3\lib\site-packages (from beautifulsoup4>=4.11.1->yfinance) (2.5)
    Requirement already satisfied: cffi>=1.12.0 in c:\users\veron\anaconda3\lib\site-packages (from curl_cffi>=0.7->yfinance) (1.16.0)
    Requirement already satisfied: certifi>=2024.2.2 in c:\users\veron\anaconda3\lib\site-packages (from curl_cffi>=0.7->yfinance) (2024.6.2)
    Requirement already satisfied: six>=1.5 in c:\users\veron\anaconda3\lib\site-packages (from python-dateutil>=2.7->matplotlib) (1.16.0)
    Requirement already satisfied: charset-normalizer<4,>=2 in c:\users\veron\anaconda3\lib\site-packages (from requests>=2.31->yfinance) (2.0.4)
    Requirement already satisfied: idna<4,>=2.5 in c:\users\veron\anaconda3\lib\site-packages (from requests>=2.31->yfinance) (3.4)
    Requirement already satisfied: urllib3<3,>=1.21.1 in c:\users\veron\anaconda3\lib\site-packages (from requests>=2.31->yfinance) (2.0.7)
    Requirement already satisfied: pycparser in c:\users\veron\anaconda3\lib\site-packages (from cffi>=1.12.0->curl_cffi>=0.7->yfinance) (2.21)
    


```python
# Define the stock ticker symbol and date range
ticker_symbol = 'AAPL'  
start_date = '2022-01-01'
end_date = '2024-12-31'

# Fetch historical data
stock_data = yf.download(ticker_symbol, start=start_date, end=end_date)

# Display the first few rows
stock_data.head()
```

    YF.download() has changed argument auto_adjust default to True
    

    [*********************100%***********************]  1 of 1 completed
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>Price</th>
      <th>Close</th>
      <th>High</th>
      <th>Low</th>
      <th>Open</th>
      <th>Volume</th>
    </tr>
    <tr>
      <th>Ticker</th>
      <th>AAPL</th>
      <th>AAPL</th>
      <th>AAPL</th>
      <th>AAPL</th>
      <th>AAPL</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2022-01-03</th>
      <td>178.645645</td>
      <td>179.499574</td>
      <td>174.425140</td>
      <td>174.542917</td>
      <td>104487900</td>
    </tr>
    <tr>
      <th>2022-01-04</th>
      <td>176.378357</td>
      <td>179.558473</td>
      <td>175.809076</td>
      <td>179.254206</td>
      <td>99310400</td>
    </tr>
    <tr>
      <th>2022-01-05</th>
      <td>171.686691</td>
      <td>176.839648</td>
      <td>171.411868</td>
      <td>176.290001</td>
      <td>94537600</td>
    </tr>
    <tr>
      <th>2022-01-06</th>
      <td>168.820663</td>
      <td>172.059668</td>
      <td>168.467317</td>
      <td>169.507721</td>
      <td>96904000</td>
    </tr>
    <tr>
      <th>2022-01-07</th>
      <td>168.987534</td>
      <td>170.921120</td>
      <td>167.868606</td>
      <td>169.694226</td>
      <td>86709100</td>
    </tr>
  </tbody>
</table>
</div>




```python
# plot chart
plt.figure(figsize=(12, 6))
plt.plot(stock_data['Close'], label=f"{ticker_symbol} Closing Price", color='blue')
plt.title(f"{ticker_symbol} Closing Price Over Time")
plt.xlabel("Date")
plt.ylabel("Price (USD)")
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()
```


    
![png](output_3_0.png)
    



```python
# Calculate 50-day and 200-day moving averages
stock_data['MA50'] = stock_data['Close'].rolling(window=50).mean()
stock_data['MA200'] = stock_data['Close'].rolling(window=200).mean()

# Plot chart
plt.figure(figsize=(12, 6))
plt.plot(stock_data['Close'], label='Close Price', alpha=0.5)
plt.plot(stock_data['MA50'], label='50-Day MA', color='orange')
plt.plot(stock_data['MA200'], label='200-Day MA', color='green')
plt.title(f"{ticker_symbol} Close Price with Moving Averages")
plt.xlabel("Date")
plt.ylabel("Price (USD)")
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

```


    
![png](output_4_0.png)
    



```python
# Calculate daily returns
stock_data['Daily Return'] = stock_data['Close'].pct_change()

# Plot histogram of returns
plt.figure(figsize=(10, 5))
sns.histplot(stock_data['Daily Return'].dropna(), bins=50, kde=True)
plt.title(f"{ticker_symbol} Daily Returns Distribution")
plt.xlabel("Daily Return")
plt.ylabel("Frequency")
plt.tight_layout()
plt.show()
```

    C:\Users\veron\anaconda3\Lib\site-packages\seaborn\_oldcore.py:1119: FutureWarning: use_inf_as_na option is deprecated and will be removed in a future version. Convert inf values to NaN before operating instead.
      with pd.option_context('mode.use_inf_as_na', True):
    


    
![png](output_5_1.png)
    



```python

```
