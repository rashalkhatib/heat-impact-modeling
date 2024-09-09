## Overview:

This AI-driven solution is designed to help health insurance companies dynamically adjust premiums in response to rising heat and Wet Bulb Globe Temperature (WBGT) trends. By leveraging real-time data, historical climate information, and predictive analytics, the software ensures premiums accurately reflect the growing health risks from heat-related illnesses, optimizing insurance risk management.

## Objectives:
- Risk-Based Premium Adjustments: Automatically adjust health insurance premiums based on the projected increase in heat-related health risks.
- Data-Driven Decision-Making: Leverage real-time climate data and predictive analytics to ensure premiums are fair and accurate.
- Customer-Centric Approach: Enhance transparency and provide customers with clear explanations for premium changes based on environmental factors.

## Core Features:
1. Environmental Data Analysis:
- Collect and process historical and real-time heat and WBGT data from global sources.
- Analyze the correlation between extreme temperatures and health insurance claims.
2. Machine Learning for Predictive Risk Modeling:
- Use advanced AI models to predict health risks related to climate changes, specifically rising temperatures.
- Generate recommendations for adjusting premiums based on forecasted heat risks.
3. Automated Premium Adjustments:
- Dynamically adjust premiums using AI-driven risk assessments.
- Provide automated notifications to customers about premium changes, ensuring transparency.

## Benefits:
- Enhanced Risk Management: Helps insurers anticipate and respond to increased heat-related health risks, reducing unexpected financial impacts.
- Revenue Protection: Ensures that premium models are aligned with emerging risks, optimizing revenue while preventing financial strain on customers.
- Improved Customer Relationships: Transparency and fair pricing strengthen trust and loyalty among policyholders.
- Proactive Health Alerts: Provides customers with insights on upcoming high-risk periods, enabling them to take preventive health actions.

## Implementation:


### Dataset: 

- Daily, County-Level Wet-Bulb Globe Temperature, Universal Thermal Climate Index, and Other Heat Metrics for the Contiguous United States, 2000-2020
  - License: [CC BY 4.0]
  - Citations: K.R. Spangler, S. Liang, and G.A. Wellenius. "Wet-Bulb Globe Temperature, Universal Thermal Climate Index, and Other Heat Metrics for US Counties, 2000-2020." Scientific Data (2022). doi: 10.1038/s41597-022-01405-3
  - Disclaimers: This data set contains modified Copernicus Climate Change Service information (2022), as described and cited in the manuscript referenced above. Neither the European Commission nor ECMWF is responsible for any use that may be made of the Copernicus information or data it contains. 
This data set is provided “as is” with no warranty of any kind.

- Dataset source: https://figshare.com/articles/dataset/_UNWEIGHTED_Daily_County-Level_Wet-Bulb_Globe_Temperature_Universal_Thermal_Climate_Index_and_Other_Heat_Metrics_for_the_Contiguous_United_States_2000-2020/19419881/2?file=35070640
  
- Description: The original data set includes daily, unweighted values of various heat metrics for every county in the contiguous United States from 2000-2020:
  - Features: Minimum, maximum, and mean values for ambient temperature, dew-point temperature, humidex, heat index, net effective temperature, wet-bulb globe temperature (WBGT), and Universal Thermal Climate Index (UTCI) are included
  - Original file format for this dataset is .rds
  - 23835252 rows × 30 columns
    
  
### Preprocessing:
- Compatibility
  - used rdata function to read .rds file into pandas
- Normalizing values
  - 2-step date conversion:
    - 'posixct_date' to str
    - str to datetime 
  - Convert Celsuis to Fahrenheit for all temperature values
  - Convert 'StCoFIPS' (Unique identifier for U.S. states and counties, a.k.a. FIPS Code) from category to integer
  - Columns renamed for usability
  - Columns with null values dropped
- Scaling dataset
  - filtered datasets by FIPS range for all counties in Texas, and Michigan
  - retained daily frequency of data for model training
  - created new csv files for Texas and Michigan 
- Calculating additional feature columns with the data
  - Diurnal Temperature Range (DTR)
  - Relative Humidity
  - Binary indicators to flag whether wet-bulb globe temperature (WBGT) safety threshhold was exceeded for a particular day
 
### Modeling:  
1. Time-seriesising Prophet: likelihood of WBGT safety threshhold exceeding threshhold over time
   - Identify 'y' values and prepare data with train_test_split
   - Fit model, make predictions
   - Visualize predictions
   - Evaluate
2. Classification using K-Means: are there common patterns by geography that can be clustered at a county level?
    - Scale data,
    - Find inertia values for optimal number of clusters,
    - Apply clustering
    - Evaluate performance with Davies-Bouldin Index
    - Visualize clusters at county level using geopandas  
     
