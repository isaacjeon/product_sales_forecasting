# Product Sales Forecasting Project

This project involves the daily sales of a single style of work pants aggregrated across different sizes and colors for a single store. The goal is to analyze and identify important factors and trends in the data, and forecast sales for up to 28 days into the future.

The `report.pdf` PDF file contains a full write-up of the results of this project. The code is included in the Jupyter notebooks, and work was done through Google Colab. 

## Dataset

The dataset was sourced from a small private retailer. I have preprocessed the dataset to include only relative non-sensitive information. With permission from the owner of the dataset, the details of the stripped-down can be seen within the Jupyter notebook files, especially within `processing_and_eda.ipynb`. The preprocessed data contains three columns:
- `date`: Contains entries for all dates from 2018-01-01 to 2025-06-01.
- `qty`: The quantity of the product sold on the respective date. This is the target value. Entries for 2020-03-20 to 2020-05-07 are missing as a result of a temporary closure of the store due to California's stay-at-home mandate during the COVID-19 pandemic. In addition, the store is fully closed on Christmas and New Year's Day and as a result will always have 0 sale counts. There may be days with reduced store hours such as Thanksgiving Day which may have non-zero values, but are not specified.
- `base_price`: This is the base retail price (ignoring higher prices for larger sizes and reverting discounted prices) for the product on the respective date. The prices are rounded up to the nearest dollar (e.g. $21.99 -> 22).

## Tools and Methods

The `processing_and_eda.ipynb` file contains the code data cleaning, processing, and analysis as well as feature engineering steps. Tools for analysis include KDE plot for the target value, ACF and PACF plots, bar plots, correlation matrix, as well as STL decomposition for trend and seasonality analysis.

Notebooks for each model are in the `model` folder. The project utilizes four models: Holt-Winters (triple exponential smoothing), SARIMAX, LightGBM, and Meta's Prophet. It also includes an ensemble using the average of the forecasted values of the best predictions for each of the four models. To improve model performance, several features were engineered based on calendar dates (e.g. day of week, month, holidays), price changes, as well as lag variables and moving averages of previous sales. Models were evaluated using expanding window cross-validation to simulate real-world forecasting scenarios.

## Analysis of Results

The seasonal-trend decomposition revealed a nonlinear trend, with a significant increase in sales between 2020 and 2021, followed by a plateau through 2023 and a slight decline into 2024. Seasonal spikes were observed during the Christmas and back-to-school shopping periods. The residual component showed increased noise between 2020 and 2023, potentially reflecting external factors not fully captured by typical seasonal or trend components. For instance, during this period, store employees observed shifts in purchasing behavior following the COVID-19 pandemic as well as changes in fashion among younger consumers influenced by social media trends.

The Prophet model produced the most accurate forecasts based on root mean square error (RMSE) out of the four individual models, likely due to higher flexible and ability identify harder-to-catch seasonal patterns. LightGBM offered strong performance with significantly faster training times, especially after feature selection. An ensemble of all four models yielded improved and more stable results despite being based on the simple average of values.

There seems to be high variance for daily values compounded by the smaller scale of the original dataset, which makes forecasting at a daily level difficult. Aggregating predictions as weekly sums can result in more acceptable predictions. Although daily forecasts proved challenging with limited data, performance improved at lower granularities, suggesting practical value for small retailers who may not have the luxury of large amounts of high quality data. This project shows that even small datasets can yield meaningful results when paired with appropriate models and domain-informed feature engineering.
