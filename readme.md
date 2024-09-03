# Uber Data Engineering Project

## Table of Contents
- Project Overview
- Tech Stack
- Project Architecture
- Steps to Reproduce
  - 1. Prerequisites
  - 2. Data Pipeline Setup
  - 3. Data Modeling
  - 4. Data Transformation
  - 5. Deployment
  - 6. Visualization
- Insights and Applications
- Considerations
- Conclusion

## Project Overview
This project demonstrates an end-to-end data engineering workflow using an Uber dataset. The goal is to build a data model, transform the data, and deploy the pipeline on Google Cloud. The final output is a dashboard created using Looker Studio.

## Tech Stack
- **Python**: For data transformation scripts.
- **SQL**: For querying and managing data.
- **Jupyter Notebook**: For exploratory data analysis.
- **Mage**: An open-source data pipeline tool.
- **Google Cloud Platform (GCP)**: For cloud services.
  - **BigQuery**: For data storage and querying.
  - **Looker Studio**: For data visualization.

## 🏗️ Project Architecture
![Architecture Diagram](architecture.jpg)


Our project leverages a robust, cloud-based architecture built on Google Cloud Platform (GCP), designed for scalability, reliability, and high-performance data processing. Here's a detailed breakdown of our architecture:

### 1. 📥 Data Ingestion and Storage
- **Raw Data Source**: Our pipeline begins with raw Uber trip data.
- **🗄️ Google Cloud Storage**: Acts as our data lake, storing the raw data in its original format. This provides a scalable, durable, and cost-effective solution for large-scale data storage.

### 2. 🔄 ETL Process
At the heart of our architecture lies the ETL (Extract, Transform, Load) process, powered by Mage:

- **🧙 Mage**: An open-source, modern data pipeline tool that orchestrates our entire ETL workflow.
  - **📤 Data Extraction**: Mage pulls raw data from Google Cloud Storage.
  - **🛠️ Data Transformation**: Complex data transformations are performed here, including:
    - Cleaning and standardizing data
    - Applying business logic
    - Creating fact and dimension tables for our star schema
  - **📥 Data Loading**: Processed data is prepared for analytics

- **💻 Mage VM on Compute Engine**: 
  - Hosts the Mage platform
  - Provides computational resources for ETL processes
  - Ensures scalability and flexibility in processing power

### 3. 🏭 Data Warehousing
- **🔍 BigQuery**: Google's serverless, highly scalable data warehouse
  - Stores processed data in a structured format
  - Enables lightning-fast SQL queries on large datasets
  - Supports real-time analytics and machine learning integrations

### 4. 📊 Data Visualization
- **👁️ Looker**: Advanced business intelligence and data visualization tool
  - Connects directly to BigQuery for real-time data access
  - Provides interactive dashboards and reports
  - Enables data exploration and self-service analytics for business users

### 🌟 Key Architectural Benefits:
1. **📈 Scalability**: Each component, from storage to processing to analytics, can scale independently to meet demand.
2. **🔧 Flexibility**: The modular design allows for easy updates or replacements of individual components.
3. **💰 Cost-Effectiveness**: Leveraging cloud services optimizes costs by paying only for resources used.
4. **⚡ Real-Time Capabilities**: The architecture supports near real-time data processing and analytics.
5. **🔄 End-to-End Solution**: Covers the entire data lifecycle from ingestion to visualization.
6. **🔒 Security**: Utilizes GCP's robust security features to protect data at rest and in transit.
7. **☁️ Managed Services**: Reduces operational overhead by using managed services like BigQuery and Cloud Storage.

This architecture empowers our Uber data engineering project with the capability to process vast amounts of trip data, derive meaningful insights, and present them in an accessible format. It sets the foundation for advanced analytics, machine learning applications, and data-driven decision-making in the ride-sharing domain.

## Dataset Used
TLC Trip Record Data Yellow and green taxi trip records include fields capturing pick-up and drop-off dates/times, pick-up and drop-off locations, trip distances, itemized fares, rate types, payment types, and driver-reported passenger counts.

Here is the dataset used in the project: [Uber Data CSV](Data/uber_data.csv) 🚕

More info about the dataset can be found here:
- **Website**: [TLC Trip Record Data](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page)
- **Data Dictionary**: [TLC Data Dictionary](https://www.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf)

## Data Model

Our data model follows a star schema design, optimized for analytical queries and reporting. It consists of one central fact table surrounded by multiple dimension tables, allowing for efficient querying and analysis of Uber ride data.

![Data model diagram](data_model.jpeg)

### Fact Table

The `fact_table` is the core of our data model, containing the following columns:
- `trip_id`: A unique identifier for each trip
- `VendorID`: Identifies the provider associated with the trip record
- `datetime_id`: Links to the `datetime_dim` table for temporal analysis
- `passenger_count_id`: Links to the `passenger_count_dim` table
- `trip_distance_id`: Links to the `trip_distance_dim` table
- `rate_code_id`: Links to the `rate_code_dim` table
- `store_and_fwd_flag`: Indicates whether the trip record was held in vehicle memory before sending to the vendor
- `pickup_location_id` and `dropoff_location_id`: Link to the respective location dimension tables
- `payment_type_id`: Links to the `payment_type_dim` table
- Various amount fields: `fare_amount`, `extra`, `mta_tax`, `tip_amount`, `tolls_amount`, `improvement_surcharge`, `total_amount`

### Dimension Tables

1. **datetime_dim**: Stores temporal information about each trip
   - Includes fields for pickup and dropoff times, hours, days, months, years, and weekdays
   - Allows for detailed time-based analysis and aggregations

2. **passenger_count_dim**: Contains information about the number of passengers
   - Useful for analyzing ride occupancy and its impact on other metrics

3. **trip_distance_dim**: Stores distance information for each trip
   - Enables analysis based on trip length and its correlation with other factors

4. **rate_code_dim**: Contains details about different rate codes
   - Includes `RatecodeID` and `rate_code_name`, allowing for analysis of pricing structures

5. **pickup_location_dim** and **dropoff_location_dim**: Store geographical coordinates
   - Include latitude and longitude for pickup and dropoff locations
   - Essential for spatial analysis and visualization of ride patterns

6. **payment_type_dim**: Contains information about payment methods
   - Includes `payment_type_id`, `payment_type`, and `payment_type_name`
   - Allows for analysis of payment preferences and their impact on other metrics

### Key Features of the Data Model

1. **Granularity**: The fact table maintains a fine granularity at the individual trip level, allowing for detailed analysis.

2. **Dimensional Analysis**: The star schema design enables multi-dimensional analysis across various aspects of the rides.

3. **Flexibility**: The model supports a wide range of queries, from simple aggregations to complex analytical queries.

4. **Performance**: By separating dimensional data, the model reduces data redundancy and improves query performance.

5. **Scalability**: The structure allows for easy addition of new dimensions or measures as the business evolves.

This data model enables a wide range of analyses, including:
- Temporal trends in ride frequency, duration, and revenue
- Geographical analysis of popular pickup and dropoff locations
- Impact of factors like passenger count, distance, and payment type on fare amounts
- Performance analysis of different rate codes and vendor IDs

By leveraging this comprehensive data model, we can extract valuable insights to optimize operations, improve customer experience, and drive data-informed decision-making in the ride-sharing business.


## Steps to Reproduce

### 1. Prerequisites
- Install Python and Jupyter Notebook.
- Set up a Google Cloud account.
- Install Mage and other required Python libraries.

### 2. Data Pipeline Setup
- **Mage**: Set up Mage for managing the data pipeline.
  - Install Mage: `pip install mage`
  - Configure Mage for your project.

### 3. Data Modeling
- **Data Model**: Design the data model using tools like Lucidchart.
  - Create fact and dimension tables.
  - Define relationships between tables.

### 4. Data Transformation
- **Python Scripts**: Write Python scripts to transform the data.
  - Load the Uber dataset.
  - Clean and preprocess the data.
  - Transform the data into fact and dimension tables.

### 5. Deployment
- **Google Cloud**: Deploy the data pipeline on Google Cloud.
  - Set up BigQuery for data storage.
  - Load the transformed data into BigQuery.

### 6. Visualization
- **Looker Studio**: Create a dashboard to visualize the data.
  - Connect Looker Studio to BigQuery.
  - Design and build the dashboard.

## Insights and Applications
This project can provide several valuable insights, such as:
- **Ride Patterns**: Analyze peak hours, popular routes, and demand trends.
- **Driver Performance**: Evaluate driver efficiency and performance metrics.
- **Customer Behavior**: Understand customer preferences and behaviors.
- **Operational Efficiency**: Identify areas for operational improvements.

### Application Scenarios
- **Business Decision Making**: Use insights to make data-driven business decisions.
- **Marketing Strategies**: Tailor marketing strategies based on customer behavior analysis.
- **Resource Allocation**: Optimize resource allocation and improve operational efficiency.
- **Predictive Analytics**: Implement predictive models to forecast demand and improve service.

## Considerations
When dealing with this kind of data, we should keep the following in mind:
- **Data Privacy**: Ensure compliance with data privacy regulations and anonymize sensitive information.
- **Data Quality**: Maintain high data quality by handling missing values, duplicates, and outliers.
- **Scalability**: Design the data pipeline to handle large volumes of data efficiently.
- **Performance**: Optimize queries and transformations for better performance.

## Conclusion
This project showcases the complete workflow of a data engineering project, from data ingestion to visualization. 

Feel free to reach out if you have any questions or need further assistance!
