# homework-pandas
# Unit 4 Homework Assignment: A Whale Off the Port(folio)

## Summary

This work is a homework for the analysis of financial market data. In this work we transform differnt styles of historical market data into portfolio-style daily returns' dataframe under a matching date range out from different funds/portfolios. We then calculate and measure the performance and risk matics of each fund, compare them to S&P TSX 60 index, and plot these measures for visualized interpretation.

Below is a summarized knowledge points.

## The Data

We work on 7 portfolios, compare to 1 index.

Read from [a csv file](Resources/whale_returns.csv), 4 whale funds' daily return 2015-03-03 to 2019-05-01, including <mark>(1)</mark> SOROS FUND MANAGEMENT LLC, <mark>(2)</mark> PAULSON & CO.INC., <mark>(3)</mark> TIGER GLOBAL MANAGEMENT LLC and <mark>(4)</mark> BERKSHIRE HATHAWAY INC. 

Read from [another csv file](Resources/algo_returns.csv), 2 algorithmic trading funds from Harold's company (a fictional company for the homework) daily return 2014-06-05 to 2019-05-01 from anthor csv file, called <mark>(5)</mark> Algo 1 and <mark>(6)</mark> Algo 2. 

Assemble 1 new portfolio called <mark>(7)</mark> My Port out from 3 stocks' historical data 2018-01-02 to 2019-12-30: [SHOP](Resources/shop_historical.csv), [OTEX](Resources/otex_historical.csv) and [L](Resources/l_historical.csv). 

We also take <mark>S&P TSX60</mark> data from [a csv](Resources/sp_tsx_history.csv) which contains the closing index from 2012-10-01 to 2019-12-30, as some kind of baseline for the analysis.

All 6 csv files are provided, can be found in the Resouces folder. We can also pull data from websites but it shall be out of scope for now.

## The wrangling techniques

First, csv to dataframe with Date as the index: 
>    pd.read_csv(Path('file_path'),index_col='Date',infer_datetime_format=True,parse_dates=True)

Outline the dataframe (df): 
>    df; df.sample()\
>    df.isnull().sum() 

Cleanup the dataframe (df): 
>    df.dropna() \
>    df['Column'].str().strip('$')\
>    df['Column'].str().replace(',','')\
>    pd.to_numeric(df['Column'].str())
    
Tidy up the dataframe (df):
>    df.rename(columns={'old name':'new name'})\
>    df.columns=[list of all columns]\
>    df.drop(labels=[list of column keys],axis=1)\
>   df.reset_index()
    
Iterate dataframe (df):
>    for index,value in df.items():
    
Combine dataframes having overlapping indexes into one combined_df:
>    pd.concat([list of dataframes],axis=1,join='inner')
    
Create dataframe from pandas Series:
>    df = pd.DataFrame({'column name' :pandas Series})

## The performance matrics

Daily Return (percentage): a dataframe (df) with index as Date
>   df.pct_change()

Cumulated Return - cumulated percentage
>   (1+df).cumprod 

Exponentially Weighted Average (of any measure)
>   ewa = df.ewm(halflife=n-days).mean()

Weighted portfolio return
>   df.dot([list of weights sum to 1])

## The risk matrics

From a daily return dataframe (df), calculate

Standard Deviation:
>   df.std()

Annual STD:
>   df.std() * np.sqrt(252)

Rolling STD:
>   df.rolling(window=n-days).std()

## Comparison matrics

Compare with S&P TSX60, and each other. All in the same dataframe (df)

Correlation:
>   df.corr()

Beta over S&P TSX60:
>   df['One column'].rolling(window=n-days).cov(df['S&P TSX60']) / df['S&P TSX60'].rolling(window=n-days).var()

## Sharpie Ratio

Since all the quantitative analysis are over past data, the purpose can be only to assess. If we are to use the assessment to direct investment, the Sharpie Ratio would be the top indicator.

Annual Sharpie Ratio
>   annual return / annual std\
Where annual return = daily-return.mean()*252, annual std = daily-std * np.sqrt(252)


## Visualization - plot DataFrame (df)

Default plot is a line plot having the index (in our case, the Date) as the x and the df value (e.g. daily return, cumulated return, rolling return) as the y. Each column in a different color.
>   df.plot()

Box plot - x is the column names (long string overlapping) y is the daily return
>   df.plot().box(rot=90)

Bar plot
>   df.plot(kind="bar",title="title")

Correlation "plot" - use style to show shades of colors
>   df.corr().style.background_radiant
