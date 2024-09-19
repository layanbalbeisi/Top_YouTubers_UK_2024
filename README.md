# Top YouTubers in UK for 2024

- [Introduction](#introduction)
- [Data Source](#data-source)
- [Tools & Technologies](#tools--technologies)
- [Methodology](#methodology)
- [SQL Query Process](#sql-query-process)
- [DAX Measures](#dax-measures)

## Introduction
The dataset provides insights into the top YouTubers in the UK for 2024, focusing on three key metrics: Total Videos, Total Views, and Total Subscribers.

## Data Source
The dataset is sourced from Kaggle, but it required significant cleaning and preprocessing.

## Tools & Technologies
- **Excel**: Initial data exploration and cleaning.
- **SQL**: For data storage, querying, and manipulation.
- **Power BI**: For creating interactive visualizations and dashboards.

## Methodology
1. **Design**: Defined the project objectives, identified relevant metrics, and planned the data workflow.
2. **Development**: Processed and cleaned the raw data, then loaded it into SQL for analysis.
3. **Testing**: Verified data accuracy and ensured that the key metrics aligned with the project goals.
4. **Analysis**: Performed detailed analysis and created visualizations to present insights into the top UK YouTubers of 2024.

## SQL Query Process

In this project, I used SQL to clean and query the data effectively. Here’s a breakdown of the steps:

### Step 1: Loading the Data
First, I uploaded the raw dataset into an SQL database to make querying easier. This dataset included information about YouTubers such as total videos, total views, total subscribers, and more.

### Step 2: Clean the Data

```sql
/*
  Select the required columns: channel name, total subscribers, total views, and total videos.
  Extract the channel name from the 'NOMBRE' column by removing the part after the '@' symbol.
*/

-- 1. Select the necessary columns and clean the channel name
SELECT
    SUBSTRING(NOMBRE, 1, CHARINDEX('@', NOMBRE) - 1) AS channel_name,  -- Extract channel name before '@'
    total_subscribers,
    total_views,
    total_videos
FROM
    top_uk_youtubers_2024;


## Step 2: Clean the Data

```sql
/*
  Select the required columns: channel name, total subscribers, total views, and total videos.
  Extract the channel name from the 'NOMBRE' column by removing the part after the '@' symbol.
*/

-- 1. Select the necessary columns and clean the channel name
SELECT
    SUBSTRING(NOMBRE, 1, CHARINDEX('@', NOMBRE) - 1) AS channel_name,  -- Extract channel name before '@'
    total_subscribers,
    total_views,
    total_videos
FROM
    top_uk_youtubers_2024;

```
## step 3: Create The SQL View
```sql
  
CREATE VIEW view_Top_youtubers_2024 As
select CAST (SUBSTRING (NOMBRE, 1, CHARINDEX('@', NOMBRE) -1 ) AS varchar(100)) as channel_name,
    total_subscribers,
		total_views,
		total_videos
from youtube_data_from_python
```
## Stpe 4: Data Quality Test 
  /* 
    
 Data quality tests
 1. The data needs to be 100 records of Youtube channels (row count test)
 2.The data needs 4 fields (column count test)
 3. The channek name column must be string format, and the other columns must be numerical data types (data type check)
 4. Each record must be unique in the dataset (duplicate count check)

 Row count - 100 (pass)
 Column count - 4

 Data types 
 Channel_name = VARCHER
 total_subscribers = INTGER
 total_views = INTGER
 total_video = INTGER

 Duplicate count = 0

 */
 ```sql
1. 
/*
# Count the total number of records (or rows) are in the SQL view
*/

SELECT
    COUNT(*) AS no_of_rows
FROM
    view_uk_youtubers_2024;
```
 ```sql

2. /*
# Count the total number of columns (or fields) are in the SQL view
*/


SELECT
    COUNT(*) AS column_count
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'view_uk_youtubers_2024'
```
 ```sql
3. /*
# Check the data types of each column from the view by checking the INFORMATION SCHEMA view
*/
SELECT
    COLUMN_NAME,
    DATA_TYPE
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'view_uk_youtubers_2024';
```

```sql
4. /*
# 1. Check for duplicate rows in the view
# 2. Group by the channel name
# 3. Filter for groups with more than one row
*/

-- 1.
SELECT
    channel_name,
    COUNT(*) AS duplicate_count
FROM
    view_uk_youtubers_2024

-- 2.
GROUP BY
    channel_name

-- 3.
HAVING
    COUNT(*) > 1;
```
## Result 
![Description of Image](https://github.com/layanbalbeisi/Top_YouTubers_UK_2024/blob/main/assests/images/%D9%84%D9%82%D8%B7%D8%A9%20%D8%B4%D8%A7%D8%B4%D8%A9%202024-09-19%20125902.png)

## DAX Measures

```dax
// Total Subscribers (M)
Total Subscribers (M) = 
VAR million = 1000000
VAR sumOfSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR totalSubscribers = DIVIDE(sumOfSubscribers, million)
RETURN totalSubscribers
```
```dax

// Total Views (B)
Total Views (B) = 
VAR billion = 1000000000
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR totalViews = ROUND(sumOfTotalViews / billion, 2)
RETURN totalViews
```
```dax
// Total Videos
Total Videos = 
VAR totalVideos = SUM(view_uk_youtubers_2024[total_videos])
RETURN totalVideos
```
```dax

// Average Views Per Video (M)
Average Views per Video (M) = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR avgViewsPerVideo = DIVIDE(sumOfTotalViews, sumOfTotalVideos, BLANK())
VAR finalAvgViewsPerVideo = DIVIDE(avgViewsPerVideo, 1000000, BLANK())
RETURN finalAvgViewsPerVideo
```
```dax
// Subscriber Engagement Rate
Subscriber Engagement Rate = 
VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR subscriberEngRate = DIVIDE(sumOfTotalSubscribers, sumOfTotalVideos, BLANK())
RETURN subscriberEngRate 
```
```dax
// Views Per Subscriber
Views Per Subscriber = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR viewsPerSubscriber = DIVIDE(sumOfTotalViews, sumOfTotalSubscribers, BLANK())
RETURN viewsPerSubscriber
```
