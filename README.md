# TimeSeries Forecasting
While working as a Data Science consultant, our company was engaged to review the sales forecasting methodology, bring efficiency to the current processes, and make the forecasts more data driven. This was for the sales organization in a Fortune 500 financial company. I led the project and worked with the client treasury and technology teams to:
1. Map the current process(es)
2. Identify areas for improvement
3. Build a data pipeline to convert manual spreadsheet data into a relational database
4. Design the data warehouse to hold years of data, multiple forecasts, budgets, etc...
5. Create time-series forecast models at different levels of granularity
6. Create reporting tools for the treasury and sales teams to use in finalizing their budget and sales goals each quarter.

Since the client and the data are subject to strict non-disclosure rules, I've sanitized the data and the code to protect the innocent. The products have been translated into the sections and instruments of a symphony orchestra and the values modified to obfuscate the data. Much of the final, productional code remains with the client. However, here in this repository, I will resurrect or recreate as much as possible for general use (and my own memory).

## Data Wrangling
The raw data for this project started as a series of spreadsheets with product lines as rows and months/years as columns. As with most spreadsheets, there as a good amount of extraneous formatting, rows, and columns added for visual aesthetics that complicated the data extraction. I used a python script to consume multiple years of these somewhat consistent spreadsheets to extract over 3 years (36+ months) of data. There was additional iteration required with the client to identify changing product names and other potential inconsistencies in the data. This file is not included in the repo. Finally, these files needed to be converted from their "wide" format to "long" so that it could be more easily used in modeling.

The resulting data, (further obfuscated by changing values, names, and dates) is found in `Revenue Data - Long.xlsx`. This file also contains the relationships between products (now symphonic instruments) and the product groups. 

## Modeling - One Group
To work out the forecasting methodology, I started with the `Forecast_ARIMA.Rmd` file to experiment with different modeling methods and review results. Before digging into the modeling, there is a quick look at the data (EDA) at the product group level to see if revenues still look reasonable after the transformation from reality to 'symphony'.

Next, looking at a single product group (Brass), I used the timeseries library (ts) to decompose the time series into seasonal and trend components. I then removed the trend component from the data, which I modeled separately from the time series. I used R's `auto.arima` function to fit an ARIMA model to the series after removing the trend. Then I modeled the trend with a 4th order poly fit. The result was two models - one for the time series, one for the trend - that I could then use to predict future values.

## Extending the Model
Once working through the methodology for one product group, I extended the process to loop over all of the different products and product groups. We did some experimentation to determine if there was sufficient data to create forecasts at the product group or individual product level. We determined that the seasonality component of the models was better determined at the product group level, while the trend and time series components had sufficient data to model at the product level.

*Note: the code for the extended models and the final process lives with the client and is not in this repository - at least not for now.*
