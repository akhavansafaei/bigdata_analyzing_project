# Big Data Cryptocurrency Analysis Project

## Overview

This project is a comprehensive big data analysis platform for cryptocurrency market data. It crawls, processes, and analyzes historical price data for thousands of cryptocurrencies from Yahoo Finance, using distributed computing frameworks and machine learning techniques to derive insights and predictions.

## Project Structure

```
bigdata_analyzing_project/
├── 1/
│   ├── bigdata_ghasemi (1).ipynb    # Data collection and web scraping notebook
│   └── weather_data.csv             # Weather data for 2020 (366 records)
├── 2/
│   ├── bigdata_ghasemi2.ipynb       # PySpark analysis and ML notebook
│   ├── merged_data_pandas.csv       # Merged cryptocurrency dataset (397,587 records)
│   └── symbols_list.txt             # List of 8,000 cryptocurrency symbols
└── README.md
```

## Features

### Data Collection
- **Web Scraping**: Automated crawling of cryptocurrency symbols from Yahoo Finance
- **Historical Data Retrieval**: Collects daily OHLCV (Open, High, Low, Close, Volume) data for 8,000+ cryptocurrencies
- **Time Period**: Full year 2020 (January 1, 2020 - December 31, 2020)
- **Successfully Processed**: 1,685 cryptocurrency symbols with complete historical data

### Data Processing
- **Big Data Framework**: Uses Apache PySpark for distributed data processing
- **Data Merging**: Combines individual cryptocurrency datasets into a single unified DataFrame
- **Data Volume**: 397,587 total records across all cryptocurrencies

### Analytics & Insights

#### Statistical Analysis
1. **Price Analysis**:
   - Mean closing prices per symbol
   - Maximum and minimum closing prices
   - Daily average closing prices
   - Price percentage changes over time

2. **Volume Analysis**:
   - Mean trading volume per symbol
   - Maximum and minimum trading volumes
   - Daily average trading volumes
   - Total trading volume per symbol

3. **Volatility Metrics**:
   - High-Low price ranges
   - Percentage price changes
   - Identification of volatile periods

4. **Symbol Performance**:
   - Top performing cryptocurrencies by mean close price
   - Most traded cryptocurrencies by volume
   - Growth analysis over specified time periods

#### Advanced Analytics
- **Growth Detection**: Functions to identify most growing symbols over custom time periods
- **Absolute Change Tracking**: Identifies symbols with highest absolute price changes
- **Time Series Analysis**: Tracks trends and patterns in cryptocurrency prices

### Machine Learning

#### Price Prediction Model
- **Algorithm**: Linear Regression with lagged features
- **Feature Engineering**: Time-lagged closing prices (configurable lag periods)
- **Training Period**: January 2020 - September 2020
- **Testing Period**: October 2020 - December 2020
- **Evaluation Metric**: RMSE (Root Mean Squared Error)
- **Visualization**: Predicted vs. actual price comparison plots

## Technologies Used

- **Python 3.x**: Core programming language
- **Apache PySpark 3.5.0**: Distributed big data processing
- **Pandas**: Data manipulation and CSV operations
- **BeautifulSoup4**: HTML parsing for web scraping
- **Requests**: HTTP library for API calls
- **Matplotlib**: Data visualization
- **tqdm**: Progress bars for long-running operations
- **PySpark ML**: Machine learning library for time series prediction

## Data Sources

- **Primary Source**: Yahoo Finance (finance.yahoo.com)
- **Cryptocurrency Data**: Historical OHLCV data via Yahoo Finance Chart API
- **Symbol List**: Scraped from Yahoo Finance cryptocurrency listings
- **Weather Data**: Weatherbit API (for correlation studies)

## Installation

### Prerequisites
```bash
pip install pyspark==3.5.0
pip install pandas
pip install beautifulsoup4
pip install requests
pip install matplotlib
pip install tqdm
```

### Setup
1. Clone the repository
2. Install dependencies
3. Ensure sufficient disk space for data storage (individual CSVs and merged dataset)

## Usage

### Notebook 1: Data Collection (`1/bigdata_ghasemi (1).ipynb`)

This notebook handles data acquisition:

1. **Scrape Cryptocurrency Symbols**:
   - Crawls Yahoo Finance crypto page
   - Extracts 8,000 cryptocurrency symbols
   - Saves to `symbols_list.txt`

2. **Download Historical Data**:
   - Iterates through symbol list
   - Fetches daily price data for 2020
   - Saves individual CSV files per symbol
   - Success rate: 1,685 symbols successfully downloaded

**Key Functions**:
```python
# Fetch historical data for a single symbol
url = f'https://query1.finance.yahoo.com/v8/finance/chart/{symbol}'
params = {
    'period1': 1577836800,  # Jan 1, 2020
    'period2': 1609372800,  # Dec 31, 2020
    'interval': '1d'
}
```

### Notebook 2: Analysis & ML (`2/bigdata_ghasemi2.ipynb`)

This notebook performs advanced analytics:

1. **Data Merging**:
```python
# Initialize Spark session
spark = SparkSession.builder.appName("MergeDataFrames").getOrCreate()

# Merge all cryptocurrency CSVs into single DataFrame
merged_df = spark.createDataFrame([], schema=schema)
# Union all individual dataframes
```

2. **Statistical Analysis Examples**:
```python
# Mean closing price by symbol
symbol_mean_df = merged_df.groupBy("Symbol").agg(mean("Close").alias("Mean_Close"))

# Daily average volume
daily_avg_volume_df = merged_df.groupBy("Date").agg(avg("Volume").alias("AvgVolume"))
```

3. **Growth Analysis**:
```python
# Find top N most growing symbols in a period
most_growing_symbols = find_most_growing_symbols(
    merged_df,
    start_date="2020-11-01",
    num_days=30,
    n=5
)
```

4. **Price Prediction**:
```python
# Train and predict using Linear Regression
predictions_df = train_and_predict_close_time_series(
    merged_df,
    symbol="BTC-USD",
    lag_num=10
)
```

## Key Findings

### Top Cryptocurrencies by Mean Close Price (2020)
1. **SZCB-USD**: $43,594.22
2. **TBTC-USD**: $20,678.07
3. **YFI-USD**: $18,733.27
4. **HBTC-USD**: $15,869.94
5. **RENBTC-USD**: $14,253.33

### Most Traded Cryptocurrencies by Volume
1. **USDT-USD**: 44.5B average daily volume
2. **BTC-USD**: 33.0B average daily volume
3. **ETH-USD**: 14.2B average daily volume
4. **LTC-USD**: 3.7B average daily volume
5. **XRP-USD**: 3.4B average daily volume

### Example Growth Analysis (Nov 1-30, 2020)
- **CPAY-USD**: +12,867% growth
- **FXC-USD**: +3,798% growth
- **DEFI-USD**: +3,045% growth

### Absolute Price Changes (Nov 1-30, 2020)
- **YFI-USD**: +$15,698
- **BTCB-USD**: +$6,348
- **WBTC-USD**: +$5,969
- **BTC-USD**: +$5,888

## Analysis Capabilities

The project provides insights into:

1. **Market Trends**: Identify overall cryptocurrency market movements
2. **Volatility Patterns**: Detect high-volatility periods and symbols
3. **Trading Activity**: Understand which cryptocurrencies are most actively traded
4. **Price Correlations**: Analyze relationships between different cryptocurrencies
5. **Growth Opportunities**: Identify rapidly growing cryptocurrencies
6. **Predictive Analytics**: Forecast future price movements using ML

## Insights & Use Cases

### For Traders
- Identify high-volume trading opportunities
- Track price volatility for risk management
- Discover emerging cryptocurrencies with high growth potential

### For Analysts
- Historical price analysis across thousands of cryptocurrencies
- Volume trend analysis
- Market-wide correlation studies

### For Researchers
- Large-scale cryptocurrency dataset for academic research
- Machine learning model development and testing
- Time series analysis methodologies

## Data Schema

```python
StructType([
    StructField("Timestamp", StringType(), True),
    StructField("Date", DateType(), True),
    StructField("Open", DoubleType(), True),
    StructField("High", DoubleType(), True),
    StructField("Low", DoubleType(), True),
    StructField("Close", DoubleType(), True),
    StructField("Volume", DoubleType(), True),
    StructField("Symbol", StringType(), True)
])
```

## Performance Considerations

- **PySpark**: Enables distributed processing of 397K+ records
- **Incremental Loading**: Processes CSV files in batches to manage memory
- **Progress Tracking**: Uses tqdm for monitoring long-running operations
- **Lazy Evaluation**: Leverages Spark's lazy evaluation for optimization

## Future Enhancements

Potential areas for expansion:

1. **Real-time Data**: Integrate live cryptocurrency price feeds
2. **Advanced ML Models**: Implement LSTM, GRU, or Prophet for better predictions
3. **Sentiment Analysis**: Incorporate social media sentiment data
4. **Dashboard**: Build interactive visualization dashboard (Streamlit/Dash)
5. **Alert System**: Price movement and volume anomaly detection
6. **Portfolio Optimization**: Multi-asset portfolio analysis tools
7. **Extended Time Range**: Expand dataset beyond 2020
8. **Additional Features**: Include market cap, circulating supply, etc.

## Notes

- Data collection respects Yahoo Finance rate limits
- User-agent headers are used to simulate browser requests
- Some symbols may fail to download due to data availability
- The merged dataset is stored in both PySpark and Pandas formats for flexibility
- Weather data in directory `1/` can be used for correlation analysis

## License

This project is for educational and research purposes.

## Disclaimer

This project is for educational purposes only. The analysis and predictions should not be considered financial advice. Cryptocurrency trading carries significant risk. Always conduct your own research before making investment decisions.

## Author

Data Science Project - Big Data Analysis with PySpark

## Acknowledgments

- Data provided by Yahoo Finance
- Apache Spark community for PySpark framework
- Python data science community
