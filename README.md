# Data_engineering_capstone_project-

# Real-time and Historical Cryptocurrency Market Analysis and Visualization Platform for BTC/USDT using Binance Data


## Project Overview

This project focuses on developing a platform for real-time and historical analysis and visualization of the BTC/USDT trading pair using Binance Kline data. The platform aims to provide users with the ability to explore, analyze, and derive insights into market trends and patterns for BTC/USDT.


### Kline Data Description

Kline data, also known as candlestick data, offers a summarized view of trading activity for a specific trading pair within a specified time frame. Each candlestick data point includes information about the opening price, closing price, highest price, lowest price, and trading volume for the period.

#### Contents of Kline Data

Kline data includes the following fields:
- **open_time**: Timestamp indicating the start of the candlestick period.
- **open**: Opening price of the asset.
- **high**: Highest price of the asset during the period.
- **low**: Lowest price of the asset during the period.
- **close**: Closing price of the asset.
- **volume**: Total trading volume of the asset during the period.
- **close_time**: Timestamp indicating the end of the candlestick period.
- **quote_asset_volume**: Total volume of the quote asset traded during the period.
- **num_trades**: Number of trades executed during the period.
- **taker_base_vol**: Total volume of trades taken by the taker during the period.
- **taker_quote_vol**: Total volume of the quote asset traded by takers during the period.
- **ignore**: Placeholder field, not used in analysis.
- **order_book**: Data related to the order book at the time of closing the candlestick.

### Additional Project Requirements

- **AWS Account**: Set up an AWS account to utilize services such as Amazon S3, AWS Glue, and Amazon EC2.
- **Snowflake Account**: Set up a Snowflake account for data storage and analysis.
- **Binance API Key**: Register for a Binance account and obtain an API key to access BTC/USDT Kline data.
- **Programming Languages and Libraries**: Utilize Python, SQL, and/or R for data processing, analysis, and model building. Relevant libraries include Pandas, NumPy, SciPy, Scikit-learn, and TensorFlow.
- **Version Control and Collaboration**: Use Git and GitHub for version control and collaboration to ensure all project-related code and documentation are well-organized and accessible.

#### Purpose of Kline Data

Kline data provides valuable insights for traders, analysts, and researchers by enabling them to:
- Analyze price movements: Identify trends, support, and resistance levels.
- Gauge market sentiment: Determine buying and selling pressure.
- Understand trading volume: Assess liquidity and market activity.
- Develop trading strategies: Use historical data to backtest and optimize strategies.

#### Example Usage

The functions `get_klines()` and `save_klines_to_csv()` allow users to retrieve Kline data for a specific trading pair and time frame from the Binance API and save it as a CSV file. For example, `save_klines_to_csv("BTCUSDT", "1h")` retrieves hourly Kline data for BTCUSDT and saves it to "BTCUSDT_1h.csv".

Using this data, users can perform various analyses, create visualizations, and develop trading strategies based on historical price action and trading volume.

## Overview

The crypto_data_pipeline (Kline Data) streamlines the extraction, transformation, and loading of BTCUSDT data from Binance into Snowflake for analysis. The pipeline comprises the following steps:

1. **Data Extraction**
   - Data is extracted from Binance using the Binance API.
   - Extracted data is stored in an S3 bucket created for raw data.

2. **Data Transformation**
   - Involves data cleaning, column removal, and derivation.
   - Transformed data is stored in a separate S3 bucket created for transformed data.

3. **Loading Data into Warehouse or Snowpipe Integration**
   - Snowpipe is configured to monitor the transformed data file in the S3 bucket.
   - A Snowpipe is created to automatically ingest data into Snowflake tables upon the arrival of the transformed data.

### Architecture Diagram

```plaintext
+---------------------+
|                              |
|   Binance API       |
|  (Kline data)          |
|                               |
+----------+----------+
           |
           v
+----------+----------+
|                     |
|  Data Extraction    |
|                     |
+----------+----------+
           |
           v
+----------+----------+
|                     |
| Data Transformation |
|                     |
+----------+----------+
           |
           v
+----------+----------+
|                     |
|    S3 Buckets       |
| (Raw & Transformed) |
+----------+----------+
           |
           v
+----------+----------+
|                     |
| Loading Data into   |
|    Snowflake        |
|                     |
+---------------------+
```

In this diagram:
- **Binance API**: Source of trade data.
- **Data Extraction**: Fetching data from the Binance API.
- **Data Transformation**: Cleaning and processing the extracted data.
- **Loading Data into Snowflake**: Storing the transformed data in Snowflake for analysis.

## Components

- **Apache Airflow**: Manages and schedules the ETL tasks.
- **Snowflake Snowpipe**: Loads data into Snowflake from S3.
- **Python**: Scripts for data extraction, transformation, and loading.
- **S3 Buckets**: AWS S3 buckets for storing raw and transformed data.

### Pros and Cons of the Binance Data Pipeline

#### Pros
1. **Scalability**: Handles large volumes of data efficiently.
2. **Flexibility**: Customizable according to user requirements.
3. **Reliability**: Ensures data integrity with S3 storage.
4. **Ease of Deployment**: Clear setup instructions and minimal dependencies.
5. **Comprehensive Data Handling**: Covers a wide range of data storage and processing needs.

#### Cons
1. **Complexity**: Requires configuring AWS services and managing dependencies.
2. **Cost**: Associated AWS and Snowflake costs.
3. **Maintenance Overhead**: Regular maintenance needed for smooth operation.
4. **Limited Error Handling**: Basic error handling, requiring additional implementation for robust management.
5. **Potential Security Risks**: Importance of proper configuration and security measures.

By weighing these pros and cons, users can determine whether this pipeline meets their specific data processing needs.

## Setup

### Prerequisites

- Docker to run Airflow
- Access to an Apache Airflow environment or Docker container
- AWS account with S3 credentials
- Snowflake account with necessary privileges
- Python environment with required packages (`boto3`, `pandas`, etc.)

### Configuration

1. **Airflow Container Setup**
   - Ensure an Airflow environment or container is running.
   - Mount the project directory containing the DAG definition file into the Airflow container.

2. **AWS Configuration**
   - Configure AWS with your IAM access keys.

3. **Snowflake Configuration**
   - Update the Snowflake SQL script (`snowflakedb_def.sql`) with your AWS credentials.
   - Execute the script in Snowflake to set up the database, schema, tables, and pipes.

## Usage

1. Clone the repository and install requirements:
   ```bash
   git clone "https://github.com/princekwusu/Crypto_Data_Pipeline.git"
   cd Crypto_Data_Pipeline
   pip install -r requirements.txt 
   ```
2. Start the Airflow container:
   ```bash
   docker-compose up -d 
   ```
3. Access the web server at https://localhost:8080.
4. Log in using default credentials (username: airflow, password: airflow).
5. Ensure the DAG file (`crypto_data_pipeline.py`) is in the DAGs directory of your Airflow environment.
6. Trigger the DAG manually or schedule automatic execution.
7. Monitor the Airflow UI for task execution and check logs for errors.
8. Verify data ingestion in Snowflake using SQL queries:
   ```sql
   SELECT * FROM cryptodb.cryptoschema.btcusdt;
   ```

## Customization

- Adjust the Binance API endpoint, S3 bucket name, and parameters as needed.
- Modify the main script (`crypto_data_pipeline.py`) for custom data processing logic.

## Contributing

Contributions and suggestions are welcome. For major changes, please open an issue first to discuss potential modifications.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

