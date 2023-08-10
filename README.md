# Cyclistic-Case-Study
Google Data Analytics Professional Certificate CAPSTONE Project

In this project, I analyzed 12 months of historical data for a bike-share company to determine how the two types of customers, casual and members, use the service differently. Throughout the project, I used Excel, RStudio, and SQL for preprocessing. Then, I used Tableau to analyze the data through meaningful visualizations. 

A detailed explanation of the project is provided below. 

## Table of Content
* Background
* Ask
* Prepare
* Process
* Analyze
* Share
* Act
  
## Background
Cyclistic is a bike-share company based in Chicago. The company currently has 3 pricing plans: single-ride passes, full-day passes, and annual memberships. Customers that buy passes are considered **casual** riders while the customers that have annual memberships are considered **members**. 

Because the financial analysts of Cyclistic have concluded that members are more profitable than casual riders, the director of marketing believes maximizing the number of annual members is the way for the company to succeed. 

The following data analysis phases were used:
* Ask
* Prepare
* Process
* Analyze
* Share
* Act

## Ask 
**Business task**: Determine how annual members and casual riders use Cyclistic bikes differently. 

Primary Stakeholder: Cyclistic Executive Team

Secondary Stakeholder: The director of marketing and Cyclistic marketing analytics team are the secondary stakeholders. The director of marketing and the analytics team will use the insights gathered from the analysis to develop campaigns promoting Cyclistic in a way that targets the conversion of casual riders to members. 

## Prepare
The data for this project was obtained [here](file:///C:/Mithuna/Data%20Analytics/Case%20Study%201/Case-Study-1.pdf), which is provided by Motivate International Inc. under this [license](https://ride.divvybikes.com/data-license-agreement). Data from July 2022 to June 2023 were used as the historical data for Cyclistic. 

Each data file is organized as follows:
* ride_id
    * This is the unique ID assigned to each ride. It is not associated with a specific customer as each ride has a separate ID regardless of customer identity. 
* rideable_type
    * This column provides the type of bike used for the ride. The options were: classic, docked, and electric. 
* started_at
    * This column provides the date and time of when the ride started. 
* ended_at
    * This column provides the date and time of when the ride ended. 
* start_station_name
    * This column provides the start station name.
* start_station_id
    * This column provides the start station ID.
* end_station_name
    * This column provides the end station name.
* end_station_id
    * This column provides the end station ID.
* start_lat
    * This column provides the latitude value of the start location.
* start_lng
    * This column provides the longitude value of the start location.
* end_lat
    * This column provides the latitude value of the end location.
* end_lng
    * This column provides the longitude value of the end location.
* member_casual
    * This column provides the type of customer that the data belongs to.

Because the data is supposed to be collected by Cyclistic according to the scenario, it is reliable and original. The data is taken from the past 12 months, which makes it comprehensive and current. As it's a primary data, it doesn't need any citation. This means, the data ROCCCs!

The data's integrity was checked using Excel by checking for missing values, ensuring no duplicates existed, and the values in each column are accurate. 

## Process
During this phase, the data was processed for analysis. This was done using multiple tools so the steps will be outlined accoring to each tool used. 

Excel processing steps:
1) Added ride_length column by subtracting started_at from ended_at to determine the total ride length.
2) Added season column according to the following months:
   * Spring: March-May
   * Summer: June-August
   * Fall: September-November
   * Winter: December-February
3) Removed duplicates with the excel function of the same name.
4) Removed data outside of the ride_length of 0:01:00 and 2:59:59
   * The data outside this range was deemed irrelevant as data under the range did not change locations in the 59 seconds that was recorded as the ride length. This data was most likely a case of customers changing their mind after starting the ride. The data over 3 hours was very few and contained extreme outliers. Data outside this range comprised of less than 4% of the total data so there was no issue in excluding them.
5) Removed data without end latitude and longitude or data with 0 for end latitude and longitude.
   * These data were incorrect.
7) Added a column for start_date and start_time for hourly trend.

RStudio processing steps:
1) After installing and loading the appropriate packages, the Excel processed data files were merged into one dataframe with the following code: 

_cyclistic_df <- list.files(path="C:/Mithuna/Data Analytics/Case Study 1/load_to_R", pattern = "*.csv", full.names = TRUE) %>% 
    lapply(read.csv) %>% 
    bind_rows_

2) After viewing the data, missing values under start_station_name/id and end_station_name/id columns were replaced with the following code:

_cyclistic_df$start_station_name[cyclistic_df$start_station_name == ''] <-'random_start_location'_

_cyclistic_df$start_station_id[cyclistic_df$start_station_id == ''] <- '11111'_

_cyclistic_df$end_station_name[cyclistic_df$end_station_name == ''] <- 'random_end_location'_

_cyclistic_df$end_station_id[cyclistic_df$end_station_id == ''] <- '22222'_

3) The resulting dataframe was exported as a csv file for further processing in SQL with the following code:

_write.csv(cyclistic_df, "C:/Mithuna/Data Analytics/Case Study 1/output_r/cyclistic_df.csv", row.names = FALSE )_

SQL processing steps:
1) The csv file was imported into SQL using custom schema
