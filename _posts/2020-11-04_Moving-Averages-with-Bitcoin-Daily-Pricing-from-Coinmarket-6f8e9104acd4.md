# Moving Averages with Bitcoin Daily Pricing from Coinmarket

The full Kaggle Note is here

![](https://cdn-images-1.medium.com/max/1200/1*pv2AJGKWmqbFC9NjAWXuCg.jpeg)
undefined

The full Kaggle Jupyter Notebook is [**here**](https://www.kaggle.com/paulrohan2020/crypto-data-eda)

And the [**Github link for the same Jupyter Notebook**](https://github.com/rohan-paul/Cryptocurrency-Kaggle/blob/main/Notebooks/BTC_EDA-Moving-Average_Kaggle_NB.ipynb)

#### Getting the Data

I have uploaded to this [Kaggle Dataset](https://www.kaggle.com/paulrohan2020/crypto-data) which is a zipped file with 26,320 `.csv` files containing the top cryptocurrencies on https://coinmarketcap.com/ by market cap worldwide. After 20:45:05 on August 4, data was collected every five minutes for three months.

Also, I have uploaded the 9,432 .csv files based on which this below EDA analysis done into my [Github repository](https://github.com/rohan-paul/Cryptocurrency-Kaggle/tree/main/Notebooks/kaggle/input/crypto-data).

This dataset is from **CoinMarketCap Data** From August 4 to November 4, 2017

Filenames represent the date and time at which the data was collected:

ymd-hms.

The data in cr\_20170804–210505.csv was collected on August 4, 2017 at 21:05:05.

#### The Columns in the Data

symbol, ranking by market cap, name, market cap, price,circulating supply, volume,% 1h,% 24h,% 1wk

#### Basics on Moving Average

Moving averages are one of the most often-cited data-parameter in the space of Stock market trading, technical analysis of market and is extremely useful for forecasting long-term trends. And beyond its use in financial time series, this is intensively used in signal processing to neural networks and it is being used quite extensively in many other fields. Basically, any data that is in a sequence.

The most commonly used Moving Averages (MAs) are the simple and exponential moving average. Simple Moving Average (SMA) takes the average over some set number of time periods. So a 10 period SMA would be over 10 periods (usually meaning 10 trading days).

Rolling mean/Moving Average (MA) smooths out price data by creating a constantly updated average price. This is useful to cut down “noise” in our price chart. Furthermore, this Moving Average could act as “Resistance” meaning from the downtrend and uptrend of stocks you could expect it will follow the trend and less likely to deviate outside its resistance point.

#### Factors to choose the Simple Moving Average (SMA) window or period

In order to find the best period of an SMA, we first need to know how long we are going to keep the stock in our portfolio. If we are swing traders, we may want to keep it for 5–10 business days. If we are position traders, maybe we must raise this threshold to 40–60 days. If we are portfolio traders and use moving averages as a technical filter in our stock screening plan, maybe we can focus on 200–300 days.

#### Now some real-world Exploratory Data Analysis with real Crypto-currency data from Coinbase

```
from mpl_toolkits.mplot3d import Axes3Dfrom sklearn.preprocessing import StandardScalerimport matplotlib.pyplot as plt # plottingimport numpy as npimport os # for accessing directory structureimport pandas as pdimport glob
```

```
#for dirname, _, filenames in os.walk('/kaggle/input'):#    for filename in filenames:#         print(os.path.join(dirname, filename))# Above code will print it all like below, this was just for the initial checking# I am commenting out as this folder has 26000+ file names to pring# /kaggle/input/crypto-data/train_BTC_combined.csv# /kaggle/input/crypto-data/Crypto-Coinmarketcap/cr_20170822-152505.csv# /kaggle/input/crypto-data/Crypto-Coinmarketcap/cr_20170812-020505.csv# /kaggle/input/crypto-data/Crypto-Coinmarketcap/cr_20170813-065506.csv# .....# Defining this input variable as I will be using this in few placesfile_dir = './kaggle/input/crypto-data/'# In Kaggle this file will be as below per Kaggle's file-tree-structure# file_dir = '/kaggle/input/crypto-data/Crypto-Coinmarketcap/'
```

```
# First defining variables for the first 2 files to see their structurenRowsRead = 1000 # specify 'None' if want to read whole file# These .csv may have more rows in reality, but we are only loading/previewing the first 1000 rowsdf1 = pd.read_csv(file_dir+'cr_20170804-034052.csv', delimiter=',', nrows = nRowsRead)df2 = pd.read_csv(file_dir+'cr_20170804-035004.csv', delimiter=',', nrows = nRowsRead)
```

```
# Let's check 1st file: /kaggle/input/crypto-data/cr_20170804-034052.csvnRowsRead = 1000 # specify 'None' if want to read whole file# cr_20170804-034052.csv may have more rows in reality, but we are only loading/previewing the first 1000 rows# df1.dataframeName = 'cr_20170804-034052.csv'nRow, nCol = df1.shapeprint(f'There are {nRow} rows and {nCol} columns')
```

```
There are 1000 rows and 18 columns
```

```
# Let's take a quick look at what the data looks like:df1.head(5)
```

undefined
undefined

Distribution graphs (histogram/bar graph) of sampled columns:

```
# plot_per_column_distribution(df1, 10, 5)
```

```
# Let's check 2nd file: /kaggle/input/crypto-data/cr_20170804-035004.csvdf2.dataframeName = 'cr_20170804-035004.csv'nRow, nCol = df2.shapeprint(f'There are {nRow} rows and {nCol} columns')
```

```
There are 1000 rows and 10 columns
```

```
# Let's take a quick look at what the data looks like:df2.head(5)
```

undefined
undefined

Distribution graphs (histogram/bar graph) of sampled columns:

```
# plot_per_column_distribution(df2, 10, 5)
```

```
print(df1.shape)print(df1.dtypes)
```

```
(1000, 18)symbol          objectranking          int64by              objectmarket          objectcap             objectname            objectmarket.1        objectcap.1           objectprice           objectcirculating     objectsupply          objectvolume          object%               object1h              object%.1            float6424h            float64%.2            float641wk            float64dtype: object
```

```
# Given I have 26,000+ .csv files, I dont have enough GPU power to combine all of them# either in Kaggle Kernel or in my local Machine. Hence, I will take 9432 of those files.# There's no precise reason behind the number 9432 - I just could copy that many files at a time in my machine# combine those 9432 file's contents to a single pandas data-frame# So let's print all the files in the directory.!ls $file_dir | wc -l  # 9432
```

```
9432
```

```
# Defining a variable for to hold a Python-list of .csv files in that directory# files_list = glob.glob(os.path.join(file_dir, "*.csv"))all_files = glob.glob(os.path.join(file_dir, "*.csv"))# lets take the first 1400 .csv file (from which I shall create a combined-dataframe)# Note in the original .zipped folder (uploaded to Kaggle) there are 26,000+ files.# But for the sake of running this data in local file-system.files_list = all_files[:9432]# lets create dataframes and print them to see if it workingdf1 = pd.read_csv(files_list[0])df2 = pd.read_csv(files_list[1])df1.head()
```

undefined
undefined

```
df2.head()
```

undefined
undefined

#### Code to combine 9432 .csv files into a single dataframe and then

#### Filter data for ‘Symbol’ column == ‘BTC’

#### generating a .csv file out that combined-single dataframe to work with.

As we can see above, all these files have the same columns so it seems reasonable to concatenate everything into one dataframe. However, I want to keep track of the file names because that’s the only reference to the date of the records.

*   First, creating a list of dataframes with the filenames in a “file\_name” column
*   Then concatenate them all into one big dataframe

#### The below are the scripts for that, but I have commented-out all of these lines, as obviously I dont want to run this huge process-intensive step every time of creating a single DataFrame out of 9432 .csv files.

```
# dataframes = [pd.read_csv(file).assign(file_name=os.path.basename(file).strip(".csv")) for file in files_list]# combined_df = pd.concat(dataframes, ignore_index=True)# combined_df.head()
```

```
# combined_df.shape
```

The above dataframe has all the SYMBOLS of all the crypto-currencies as was in the individual .csv files. But now I want to extract ONLY the symbol ‘BTC’ for Bitcoin for further analysis.

Below is the code for that.

```
# Creating a dataframe, by filtering only the rows where the column 'Symbol' is 'BTC'# btc_df = combined_df[combined_df['symbol'] == 'BTC']# btc_df.shape
```

#### Now generating a .csv file contain which will be used as a training dataset

This file is created out that combined-single dataframe (that I earlier created from 9432 .csv files ) — as obviously I don't want to run this huge process-intensive step of creating a single Data-Frame out of 9432 .csv files.

Below code is commented out as well, because I have run this script just once to create the file. And then I have saved the file (to be used as an input) both in Kaggle and also for my local machine

```
# btc_df.to_csv("train_BTC_combined.csv", index=False)# Passing index=False so as not to not write out an 'unnamed' index column to the combined dataframe
```

```
# Now that I have already created a .csv with the combined dataframe for BTC, lets use that going forward.original_btc_train = pd.read_csv("train_BTC_combined.csv")# The same code above will be as below in Kaggle because of the Kaggle's file-tree-structure for uploaded input-data# original_btc_train = pd.read_csv("/kaggle/input/crypto-data/train_BTC_combined.csv")original_btc_train.head()
```

undefined
undefined

```
original_btc_train.shape
```

```
(9431, 25)
```

Lets analyze the dataframe with .info() method. This method prints a concise summary of the data frame, including the column names and their data types, the number of non-null values, the amount of memory used by the data frame.

```
original_btc_train.info()
```

```
<class 'pandas.core.frame.DataFrame'>RangeIndex: 9431 entries, 0 to 9430Data columns (total 25 columns): #   Column                 Non-Null Count  Dtype  ---  ------                 --------------  -----   0   symbol                 9431 non-null   object  1   ranking by market cap  9430 non-null   float64 2   name                   9431 non-null   object  3   market cap             9430 non-null   object  4   price                  9431 non-null   object  5   circulating supply     9430 non-null   object  6   volume                 9430 non-null   object  7   % 1h                   9430 non-null   object  8   % 24h                  9430 non-null   object  9   % 1wk                  9430 non-null   object  10  file_name              9431 non-null   object  11  ranking                1 non-null      float64 12  by                     1 non-null      object  13  market                 1 non-null      object  14  cap                    1 non-null      object  15  market.1               1 non-null      object  16  cap.1                  1 non-null      object  17  circulating            1 non-null      object  18  supply                 0 non-null      float64 19  %                      0 non-null      float64 20  1h                     0 non-null      float64 21  %.1                    0 non-null      float64 22  24h                    0 non-null      float64 23  %.2                    0 non-null      float64 24  1wk                    0 non-null      float64dtypes: float64(9), object(16)memory usage: 1.8+ MB
```

As shown above, the data sets do not contain null values but some of the columns where I expected numerical or float values, instead contain object Dtype like the ‘market cap’ column.

Also starting with ‘file\_name’ upto ‘1wk’ columns dont have

```
# All Features Listprint("All Features list", original_btc_train.columns.tolist())print("\nMissing Values", original_btc_train.isnull().any())print("\nUnique Values ", original_btc_train.nunique())
```

```
All Features list ['symbol', 'ranking by market cap', 'name', 'market cap', 'price', 'circulating supply', 'volume', '% 1h', '% 24h', '% 1wk', 'file_name', 'ranking', 'by', 'market', 'cap', 'market.1', 'cap.1', 'circulating', 'supply', '%', '1h', '%.1', '24h', '%.2', '1wk']Missing Values symbol                   Falseranking by market cap     Truename                     Falsemarket cap                Trueprice                    Falsecirculating supply        Truevolume                    True% 1h                      True% 24h                     True% 1wk                     Truefile_name                Falseranking                   Trueby                        Truemarket                    Truecap                       Truemarket.1                  Truecap.1                     Truecirculating               Truesupply                    True%                         True1h                        True%.1                       True24h                       True%.2                       True1wk                       Truedtype: boolUnique Values  symbol                      1ranking by market cap       1name                        2market cap               9181price                    8789circulating supply       2001volume                   8969% 1h                      612% 24h                    2021% 1wk                    3370file_name                9431ranking                     1by                          1market                      1cap                         1market.1                    1cap.1                       1circulating                 1supply                      0%                           01h                          0%.1                         024h                         0%.2                         01wk                         0dtype: int64
```

```
# Remove the '$' and the extra commas from the 'market cap' column# In the real world data set, to see if there are are non-numeric values in the column.# my first approach was to try to use astype() as below commented out line# original_btc_train['market cap'].astype('float')# The output would be - could not convert string to float: '$70,846,063,125 '# So now, let’s try removing the ‘$’ and ‘,’ using str.replace :original_btc_train['market cap'] =original_btc_train['market cap'].str.replace(',', '')original_btc_train['market cap'] =pd.to_numeric(original_btc_train['market cap'].str.replace('$', ''))# In above I using to_numeric() as otherwise the 'market cap' column will continue to be dtype objectoriginal_btc_train['market cap']
```

```
0       7.084606e+101       7.671529e+102       7.141177e+103       6.715872e+104       6.724371e+10            ...     9426    7.130818e+109427    7.047523e+109428    7.234822e+109429    6.852041e+109430    6.749006e+10Name: market cap, Length: 9431, dtype: float64
```

```
original_btc_train.head()
```

undefined
undefined

```
# Basic statisticsoriginal_btc_train.describe()
```

```
# and now if I run beloworiginal_btc_train['market cap'].astype('float')
```

```
0       7.084606e+101       7.671529e+102       7.141177e+103       6.715872e+104       6.724371e+10            ...     9426    7.130818e+109427    7.047523e+109428    7.234822e+109429    6.852041e+109430    6.749006e+10Name: market cap, Length: 9431, dtype: float64
```

```
btc_train = original_btc_train.set_index('file_name')btc_train.head()
```

undefined
undefined

```
market_cap = btc_train[['market cap']]market_cap.head()
```

undefined
undefined

#### Rolling Mean (Moving Average) — to determine a trend

A simple moving average, also called a rolling or running average is formed by computing the average price of a security over a specific number of periods. Most moving averages are based on closing prices; for example, a 5-day simple moving average is the five-day sum of closing prices divided by five. As its name implies, a moving average is an average that moves. Old data is dropped as new data becomes available, causing the average to move along the time scale. The example below shows a 5-day moving average evolving over three days.

```
Daily Closing Prices: 11,12,13,14,15,16,17First day of 5-day SMA: (11 + 12 + 13 + 14 + 15) / 5 = 13Second day of 5-day SMA: (12 + 13 + 14 + 15 + 16) / 5 = 14Third day of 5-day SMA: (13 + 14 + 15 + 16 + 17) / 5 = 15
```

undefined
undefined

The first day of the moving average simply covers the last five days. The second day of the moving average drops the first data point (11) and adds the new data point (16).

So the simple moving average is the unweighted mean of the previous M data points. The selection of M (sliding window) depends on the amount of smoothing desired since increasing the value of M improves the smoothing at the expense of accuracy.

The moving average is used to analyze the time-series data by calculating averages of different subsets of the complete dataset. Since it involves taking the average of the dataset over time, it is also called a moving mean (MM) or rolling mean. Moving averages are widely used in finance to determine trends in the market and in environmental engineering to evaluate standards for environmental quality such as the concentration of pollutants.

The easiest way to calculate the simple moving average is by using the pandas.Series.rolling method. This method provides rolling windows over the data. On the resulting windows, we can perform calculations using a statistical function (in this case the mean). The size of the window (number of periods) is specified in the argument window.

```
market_cap.rolling(window=3).mean()
```

undefined
undefined

```
market_cap['ma_rolling_3-Day'] = market_cap['market cap'].rolling(window=3).mean().shift(1)market_cap['ma_rolling_30-Day'] = market_cap['market cap'].rolling(window=30).mean().shift(1)market_cap['ma_rolling_3-Months'] = market_cap['market cap'].rolling(window=90).mean().shift(1)market_cap
```

```
/home/paul/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:1: SettingWithCopyWarning: A value is trying to be set on a copy of a slice from a DataFrame.Try using .loc[row_indexer,col_indexer] = value insteadSee the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy  """Entry point for launching an IPython kernel./home/paul/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: A value is trying to be set on a copy of a slice from a DataFrame.Try using .loc[row_indexer,col_indexer] = value insteadSee the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy  /home/paul/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:3: SettingWithCopyWarning: A value is trying to be set on a copy of a slice from a DataFrame.Try using .loc[row_indexer,col_indexer] = value insteadSee the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy  This is separate from the ipykernel package so we can avoid doing imports until
```

undefined
undefined

```
colors = ['steelblue', 'red', 'purple', 'black']market_cap.plot(color=colors, linewidth=2, figsize=(20,6))
```

```
<matplotlib.axes._subplots.AxesSubplot at 0x7fe2baad6c50>
```

undefined
undefined

#### Weighted moving average

Weighted moving average = (t weighting factor) + ((t-1) weighting factor-1) + ((t-n) \* weighting factor-n)/n

**weighted moving average assigns a specific weight or frequency to each observation, with the most recent observation being assigned a greater weight than those in the distant past to obtain the average.**

**Example**

Assume that the number of periods is 10, and we want a weighted moving average of four stock prices of $70, $66, $68, and $69, with the first price being the most recent.

Using the information given, the most recent weighting will be 4/10, the previous period before that will be 3/10, and the next period before that will be 2/10, and the initial period weighting will be 1/10.

The weighting average for the four different prices will be calculated as follows:

#### WMA = \[70 x (4/10)\] + \[66 x (3/10)\] + \[68 x (2/10)\] + \[69 x (1/10)\]

WMA = $28 + $19.80 + $13.60 + $6.90 = $68.30

undefined
undefined

The accuracy of this model depends largely on your choice of weighting factors. If the time series pattern changes, you must also adapt the weighting factors.

When creating a weighting group, you enter the weighting factors as percentages. The sum of the weighting factors does not have to be 100%.

```
def weighted_mov_avg(weights):    def calc(x):        return (weights*x).mean()    return calcmarket_cap['market cap'].rolling(window=3).apply(weighted_mov_avg(np.array([0.5,1,1.5]))).shift(1)market_cap['wma_rolling_3'] = market_cap['market cap'].rolling(window=3).apply(weighted_mov_avg(np.array([0.5,1,1.5]))).shift(1)market_cap.plot(color=colors, linewidth=2, figsize=(20,6))
```

undefined
undefined

#### Exponentially weighted moving average (EMA)

undefined
undefined

The formula states that the value of the moving average(S) at time t is a mix between the value of raw signal(x) at time t and the previous value of the moving average itself i.e. t-1. It is basically a value between the previous EMA and the current price The degree of mixing is controlled by the parameter a (value between 0–1).

The ‘a’ in the above is called the smoothing factor and sometime also denonted as **𝛼** ( alpha ) is defined as:

undefined
undefined

where 𝑛 is the number of days in our span. Therefore, a 10-day EMA will have a smoothing factor:

So the above Formulae can also be written as by simply re-arranging the terms in the above formulae

#### Exponential moving average = (Closing Price — Previous EMA) \* (2/(Alpha + 1)) + Previous EMA

So,

*   if a = 10%(small), most of the contribution will come from the previous value of the signal. In this case, “smoothing” will be very strong.
*   if a = 90%(large), most of the contribution will come from the current value of the signal. In this case, “smoothing” will be minimum.

By looking at the documentation, we can note that the .ewm() method has an adjust parameter that defaults to True. This parameter adjusts the weights to account for the imbalance in the beginning periods (if you need more detail, see the Exponentially weighted windows section in the [pandas documentation](https://pandas.pydata.org/pandas-docs/stable/user_guide/computation.html#exponentially-weighted-windows)).

```
# Now lets implement the ewm() function of Pandasmarket_cap['market cap'].ewm(span=3, adjust=False, min_periods=0).mean()
```

```
file_namer_20170815-071005    7.084606e+10r_20170831-114005    7.378068e+10r_20170904-085005    7.259623e+10r_20170819-065505    6.987747e+10r_20170818-210005    6.856059e+10                         ...     r_20170904-132006    6.978009e+10r_20170817-190505    7.012766e+10r_20170829-064005    7.123794e+10r_20170824-044005    6.987917e+10r_20170821-103506    6.868462e+10Name: market cap, Length: 9431, dtype: float64
```

```
market_cap['ewm_window_3'] = market_cap['market cap'].ewm(span=3, adjust=False, min_periods=0).mean().shift(1)market_cap
```

```
/home/paul/anaconda3/lib/python3.7/site-packages/ipykernel_launcher.py:1: SettingWithCopyWarning: A value is trying to be set on a copy of a slice from a DataFrame.Try using .loc[row_indexer,col_indexer] = value insteadSee the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy  """Entry point for launching an IPython kernel.
```

undefined
undefined

```
# market_cap.plot(color=colors, linewidth=2, figsize=(20,6))# Now that we have quite a few columns in the 'market_cap' dataframe# if I plot the moving averages of all of them, it would become so dense# and can not separately visible# so lets plot only the column 'ewm_window_3'market_cap[['ewm_window_3']].plot(color=colors, linewidth=2, figsize=(20,6))
```

```
<matplotlib.axes._subplots.AxesSubplot at 0x7fe2baa65150>
```

undefined
undefined

#### What Is Exponential Smoothing?

Exponential smoothing is a time series forecasting method for univariate data. Exponential smoothing forecasting is a weighted sum of past observations, but the model explicitly uses an exponentially decreasing weight for past observations. Specifically, past observations are weighted with a geometrically decreasing ratio.

The underlying idea of an exponential smoothing model is that, at each period, the model will learn a bit from the most recent demand observation and remember a bit of the last forecast it did.

The smoothing parameter (or learning rate) alpha will determine how much importance is given to the most recent demand observation.

undefined
undefined

Where 0 <= alpha <= 1

alpha is a ratio (or a percentage) and of how much importance the model will allocate to the most recent observation compared to the importance of demand history. The one-step-ahead forecast for time T+1 is a weighted average of all of the observations in the series y1,…,yT

```
market_cap['market cap'].ewm(alpha=0.7, adjust=False, min_periods=3).mean()
```

```
file_namer_20170815-071005             NaNr_20170831-114005             NaNr_20170904-085005    7.247460e+10r_20170819-065505    6.875348e+10r_20170818-210005    6.769664e+10                         ...     r_20170904-132006    7.040298e+10r_20170817-190505    7.045355e+10r_20170829-064005    7.177982e+10r_20170824-044005    6.949823e+10r_20170821-103506    6.809251e+10Name: market cap, Length: 9431, dtype: float64
```

```
market_cap['esm_window_3_7'] = market_cap['market cap'].ewm(alpha=0.7, adjust=False, min_periods=3).mean()market_cap
```

undefined
undefined

```
market_cap[['esm_window_3_7']].plot(color=colors, linewidth=2, figsize=(20,6))
```

```
<matplotlib.axes._subplots.AxesSubplot at 0x7fe2baa04610>
```

undefined
undefined

```
# Now also plot it along with 'market cap'market_cap[['market cap','esm_window_3_7']].plot(color=colors, linewidth=2, figsize=(20,6))
```

undefined
undefined

#### Trading Strategy based on Moving Average

There are quite a few very popular strategies that Traders regularly executes based on Moving Averages. Let's checkout couple of them.

First, note the fact that a moving average timeseries (for both SMA or EMA) lags the actual price behavior. And also the assumption that when a change in the long term behavior of the asset occurs, the actual price timeseries will react faster than the EMA one. Therefore, we will consider the crossing of the two as potential trading signals.

*   When the price of an asset crosses the EMA timeseries of the same from below, we will close any existing short position and go long (buy) one unit of the asset.
*   And when the price crosses the EMA timeseries from above, we will close any existing long position and go short (sell) one unit of the asset.

For some more of these strategies have a look at [**this article**](https://blackwellglobal.com/3-simple-moving-average-strategies-for-day-trading/)

By [Rohan Paul](https://medium.com/@paulrohan) on [November 4, 2020](https://medium.com/p/6f8e9104acd4).

[Canonical link](https://medium.com/@paulrohan/moving-averages-with-bitcoin-daily-pricing-from-coinmarket-6f8e9104acd4)

Exported from [Medium](https://medium.com) on December 12, 2020.