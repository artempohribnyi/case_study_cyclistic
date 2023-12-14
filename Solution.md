# How Does a Bike-Share Navigate Speedy Success?
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/4feddaf4-d93c-4c0f-8d9a-be4b94093129)


## Scenario

I’m a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, my team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, my team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve my recommendations, so they must be backed up with compelling data insights and professional data visualizations.

## Characters and teams

● **Cyclistic:** A bike-share program that features more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use them to commute to work each day.

● **Lily Moreno:** The director of marketing and my manager. Moreno is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social media, and other channels.

● **Cyclistic marketing analytics team:** A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy. I joined this team six months ago and have been busy learning about Cyclistic’s mission and business goals, as well as how I, as a junior data analyst, can help Cyclistic achieve them.

● **Cyclistic executive team:** The notoriously detail-oriented executive team will decide whether to approve the recommended marketing program.


## About the company

In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked at one station and returned to any other station in the system at any time.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic's members.

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.

Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends.


## Ask 

Three questions will guide the future marketing program:

1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

Moreno has assigned me the first question to answer: How do annual members and casual riders use Cyclistic bikes differently?

So, **the business task is to understand how annual members and casual riders use Cyclistic bikes differently** . And if we can use those insights in designing marketing strategies aimed at converting casual riders into annual members, this involves analyzing the historical bike trip data to uncover patterns and differences in usage between these two user groups.

### Consider Key Stakeholders:

**Lily Moreno (Director of Marketing and Manager):** She is interested in this data to design effective marketing strategies. Her goal is to maximize the number of annual memberships for the company's future success.

**Cyclistic Marketing Analytics Team:** Responsible for collecting, analyzing, and reporting data to guide the marketing strategy. They will be directly involved in the analysis and interpretation of the bike trip data.

**Cyclistic Executive Team:** They are the decision-makers who will approve or reject the recommended marketing program. They need comprehensive and compelling insights to support decision-making.

**Cyclistic Finance Analysts:** Have concluded that annual members are more profitable than casual riders. The analysis will provide data-backed insights into the profitability of different customer segments.

**Casual Riders and Annual Members:** Understanding their usage patterns is crucial to tailor marketing strategies that encourage casual riders to become annual members.

**Digital Media Team (potentially):** Depending on the findings, insights may guide the digital media team on how to target and influence casual riders effectively.


## Prepare

Following the task, I downloaded trip historical data from the [public repository](https://divvy-tripdata.s3.amazonaws.com/index.html) where it is located.

### Official documentation:

Here you'll find Cyclistic's trip data for public use. So whether you're a policymaker, transportation professional, web developer, designer, or just plain curious, feel free to download it, map it, animate it, or bring it to life!

Note: This data is provided according to the [Divvy Data License Agreement](https://www.divvybikes.com/data-license-agreement) and released on a monthly schedule.

### The Data

Each trip is anonymized and includes:

* Trip start date and time
* Trip end day and time
* Trip start station
* Trip end station
* Rider type (Member, Single Ride, and Day Pass)
  
The data has been processed to remove trips that are taken by staff as they service and inspect the system and any trips that were below 60 seconds in length (potentially false starts or users trying to re-dock a bike to ensure it was secure).


I think the last 10 months, from January to October 2023, will be enough to complete the business task. Since the data is released on a monthly schedule, each month is in a separate zip file.

To ensure data integrity, I downloaded all files to a separate folder, *cyclistic_tripdata*, and, keeping the original files and using appropriate file-naming conventions, unziped them to a subfolder, *2023_tripdata*.

Datasets can show a lot of interesting information. But to be sure, I chose the right data that can actually help me solve a problem I decided to take a glance, using Excel to open the smallest-sized file, *202301_tripdata*. The text-to-column option helped to create a table since it's in CSV format. Sorted by *start_at*. So, there is time series data to analyze trends over time or to analyze different patterns of use between casuals and members, like average ride time. Count the number of rides and try to understand the preferred bike type, weekdays, and start stations.

I mentioned that there are some blanks in a data set, and they'll need some cleaning for further analysis. And since just this data set, the smallest one, contains 190302 rows, it would be great to manipulate all data sets in one place, so I decided that BigQuery is better for this purpose.

The conclusion from this step is that the data from reliable data sources, originally collected by a first-party organisation, is comprehensive, current, and cited. **So it ROCCCs!** Let's move forward to the next step.

## Process

To upload data to BigQuarry, I created a new project and a new data set, *cyclistic_tripdata*. Then I loaded each data set into BigQuarry, but because of the size of some data sets, it was needed to use Google Cloud Storage.

The next step using the menu is to check the schema of each table, field names, and data type.

*CREATE TABLE* and *UNION ALL* merged tables to one single table, *tripdata_all*, for esy processing.
```
CREATE TABLE cyclistic_tripdata.tripdata_all AS 
SELECT * FROM cyclistic_tripdata.202301_tripdata UNION ALL
    SELECT * FROM cyclistic_tripdata.202302_tripdata UNION ALL
      SELECT * FROM cyclistic_tripdata.202303_tripdata UNION ALL
        SELECT * FROM cyclistic_tripdata.202304_tripdata UNION ALL
          SELECT * FROM cyclistic_tripdata.202305_tripdata UNION ALL
            SELECT * FROM cyclistic_tripdata.202306_tripdata UNION ALL
              SELECT * FROM cyclistic_tripdata.202307_tripdata UNION ALL
                SELECT * FROM cyclistic_tripdata.202308_tripdata UNION ALL
                  SELECT * FROM cyclistic_tripdata.202309_tripdata UNION ALL
                    SELECT * FROM cyclistic_tripdata.202310_tripdata
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/48a20296-fe22-4617-bf59-1d98ee7bb192)

Checking for duplicates.
```
SELECT
  COUNT(ride_id) AS num_of_observ,
  COUNT(DISTINCT ride_id) AS dist_num_of_observ
FROM
  `cyclistic_tripdata.tripdata_all`
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/43438405-c292-4105-a681-b91a9df1c41c)


I see that we have some rows with *null* values in such columns:

**start_station_name
start_station_id
end_station_name
end_station_id**

Let's count how many observations have *null* values at all.

```
SELECT
  COUNT(*) AS num_of_rows_with_null
FROM
  `cyclistic_tripdata.tripdata_all`
WHERE
  ride_id IS NULL
  OR rideable_type IS NULL
  OR started_at IS NULL
  OR ended_at IS NULL
  OR start_station_name IS NULL
  OR start_station_id IS NULL
  OR end_station_name IS NULL
  OR end_station_id IS NULL
  OR start_lat IS NULL
  OR start_lng IS NULL
  OR end_lat IS NULL
  OR end_lng IS NULL
  OR member_casual IS NULL
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/636baf13-cdd8-4b20-9bb0-37cab0e89e9f)

The total number of observations is 5133286. It's 24,22%. Have to investigate if there are any patterns there.

Questions are:
* Which columns have *nuul* values?
* Do we need all that data to answer business question, or can we drop it?
* What causes *null* in the data set?

```
/* 
I want to investigate which columns have NULLs. 
I'm going to compare the number of NULLs in each column to the number of rows in the whole data set (5133286).
*/
SELECT
  COUNT(*) - COUNT(ride_id) AS ride_id,
  COUNT(*) - COUNT(rideable_type) AS rideable_type,
  COUNT(*) - COUNT(started_at) AS started_at,
  COUNT(*) - COUNT(ended_at) AS ended_at,
  COUNT(*) - COUNT(start_station_name) AS start_station_name,
  COUNT(*) - COUNT(start_station_id) AS start_station_id,
  COUNT(*) - COUNT(end_station_name) AS end_station_name,
  COUNT(*) - COUNT(end_station_id) AS end_station_id,
  COUNT(*) - COUNT(start_lat) AS start_lat,
  COUNT(*) - COUNT(start_lng) AS start_lng,
  COUNT(*) - COUNT(end_lat) AS end_lat,
  COUNT(*) - COUNT(end_lng) AS end_lng,
  COUNT(*) - COUNT(member_casual) AS member_casual
FROM
  `cyclistic_tripdata.tripdata_all`
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/60372359-f3ea-406c-a0d1-b307ab258cc9)

Ok. I see now that there are nulls in this columns:

**start_station_name
start_station_id
end_station_name
end_station_id
end_lat
end_lng**

I decided to create two tables for further analysis. The first, with the dropped variables (columns), and the second, with the dropped observations (rows), contain nulls.

```
/* 
The table without dropped variables (columns) with null values.
*/

WITH
  dropped_var AS (
  SELECT
    ride_id,
    rideable_type,
    started_at,
    ended_at,
    member_casual
  FROM
    cyclistic_tripdata.tripdata_all
  ORDER BY
    started_at )

SELECT
  *
FROM
  dropped_var
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/10b9ffc4-1e83-422e-b7f6-da2253bfb252)

```
/* 
Dropped any observations with a null. 
*/

WITH
  dropped_observ AS (
  SELECT
    *
  FROM
    cyclistic_tripdata.tripdata_all
  WHERE
    start_station_name IS NOT NULL
    AND start_station_id IS NOT NULL
    AND end_station_name IS NOT NULL
    AND end_station_id IS NOT NULL
    AND end_lat IS NOT NULL
    AND end_lng IS NOT NULL
  ORDER BY
    started_at )

SELECT
  *
FROM
  dropped_observ
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/b1ebd096-07fa-4881-bd74-5ae32139036a)

Let's do some calculations next. I'm going to add two new variables: *ride_length* and *day_of_week*.

```
/*
Added two variables 'ride_length' and 'day_of_week'.
*/

SELECT
  *,
  DATE_DIFF(ended_at, started_at, minute) AS ride_length,
  FORMAT_DATE('%A', started_at) AS day_of_week
FROM
  dropped_var
ORDER BY 
  ride_length
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/b2055126-6dea-4d08-886f-69ba3ebc2404)

But what do we see here? There are some observations with a minus sign, which means that there are missing records in timestamps. Let's fix this.

```
WITH
  dropped_var AS (
  SELECT
    ride_id,
    rideable_type,
    started_at,
    ended_at,
    member_casual,
    -- Fixed missing values with changing their places
    IF(DATE_DIFF(ended_at, started_at, minute) >= 0, started_at, ended_at) AS started_at_fixed,
    IF(DATE_DIFF(ended_at, started_at, minute) >= 0, ended_at, started_at) AS ended_at_fixed
  FROM
    cyclistic_tripdata.tripdata_all )
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/9f0ec351-e6e8-4386-9ccc-e1c4ff518553)

Another issue that we know from a data document< that the data has been processed to remove trips that are taken by staff as they service and inspect the system and any trips that were below 60 seconds in length (potentially false starts or users trying to re-dock a bike to ensure it was secure). So let's filter zeros in the *ride_length*.

```
SELECT
  ride_id,
  rideable_type,
  started_at_fixed,
  ended_at_fixed,
  member_casual,
  DATE_DIFF(ended_at_fixed, started_at_fixed, MINUTE) AS ride_length,
  FORMAT_DATE('%A', started_at_fixed) AS day_of_week
FROM
  dropped_var
-- Filtered values are less than 60 seconds.
WHERE
  DATE_DIFF(ended_at_fixed, started_at_fixed, MINUTE) > 0
ORDER BY 
  ride_length
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/38d26679-f3ca-49c1-b677-3e350ad91f50)

Using a quarry setting, I saved the result in a new table, *tripdata_clean_var*. The same I did with the second table, *tripdata_clean_observ*.
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/43783d05-a15f-4463-a032-8f076b436791)


So, now that my data is stored appropriately and has been prepared for analysis, let's start putting it to work.


## Analyze

Let's calculate some summary statistics, investigate interesting trends, and do aggregations.

**Q: What is the mean, max, and min of ride_length?**

```
SELECT
  *
FROM (
  SELECT
    *
  FROM (
    SELECT
      DISTINCT col,
      PERCENTILE_CONT(value, 0.25) OVER win AS q1,
      PERCENTILE_CONT(value, 0.50) OVER win AS q2,
      PERCENTILE_CONT(value, 0.75) OVER win AS q3,
      ROUND(AVG(value) OVER win, 1) AS avg,
      ROUND(STDDEV(value) OVER win, 1) AS std,
      MAX(CAST(value AS FLOAT64)) OVER win AS max,
      MIN(CAST(value AS FLOAT64)) OVER win AS min
    FROM
      `cyclistic_tripdata.tripdata_clean_observ` 
    UNPIVOT (value FOR col IN (ride_length))
    WINDOW
      win AS (PARTITION BY col)) UNPIVOT (value FOR stats IN (q1, q2, q3, avg, std, max, min)) ) PIVOT (ANY_VALUE(value) FOR col IN ('ride_length'))

```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/6c520498-2980-43eb-a14d-1600d903d37f)

**Q: What is the mode of the *da_of_week*?**

```
SELECT
  day_of_week,
  COUNT(day_of_week) AS mode
FROM
  `cyclistic_tripdata.tripdata_clean_observ`
GROUP BY
  day_of_week
ORDER BY
  mode DESC
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/b2152345-76c3-4a8e-8685-3b3d3a860ba7)

**Q: What is the most popular day of the week by *member_casual*?**

```
SELECT
  member_casual,
  COUNTIF(day_of_week = 'Monday') AS Monday,
  COUNTIF(day_of_week = 'Tuesday') AS Tuesday,
  COUNTIF(day_of_week = 'Wednesday') AS Wednesday,
  COUNTIF(day_of_week = 'Thursday') AS Thursday,
  COUNTIF(day_of_week = 'Friday') AS Friday,
  COUNTIF(day_of_week = 'Saturday') AS Saturday,
  COUNTIF(day_of_week = 'Sunday') AS Sunday
FROM
  `cyclistic_tripdata.tripdata_clean_observ`
GROUP BY
  member_casual
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/abc7edeb-5290-4890-90b6-896ef523bfbf)

**Q: What is the average *ride_length* by *member_casual*?**

```
SELECT
  member_casual,
  ROUND(AVG(ride_length), 2) AS total_avg_ride_length
FROM
  `cyclistic_tripdata.tripdata_clean_observ`
GROUP BY
  member_casual
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/d47d1ccc-b9d3-4b7a-a6bb-fbb06d761e15)

**Q: What is the average *ride_length* by *member_casual* by day_of_week?**

```
SELECT
  member_casual,
  ROUND(AVG(CASE WHEN day_of_week = 'Monday' THEN ride_length END), 2) AS Monday,
  ROUND(AVG(CASE WHEN day_of_week = 'Tuesday' THEN ride_length END), 2) AS Tuesday,
  ROUND(AVG(CASE WHEN day_of_week = 'Wednesday' THEN ride_length END), 2) AS Wednesday,
  ROUND(AVG(CASE WHEN day_of_week = 'Thursday' THEN ride_length END), 2) AS Thursday,
  ROUND(AVG(CASE WHEN day_of_week = 'Friday' THEN ride_length END), 2) AS Friday,
  ROUND(AVG(CASE WHEN day_of_week = 'Saturday' THEN ride_length END), 2) AS Saturday,
  ROUND(AVG(CASE WHEN day_of_week = 'Sunday' THEN ride_length END), 2) AS Sunday
FROM
  `cyclistic_tripdata.tripdata_clean_observ`
GROUP BY
  member_casual;
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/ea33eee6-e0dd-4cb4-ab24-0556ac442b66)


## Share

During ten months from January 2023 to October 2023 annual members were using Cyclistic's bikes more frequently.  

![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/75b5c31b-a110-440a-9d78-6b97b8a95cf1)

Casual members spend more time overall than annual members.
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/2ccaf094-2852-4010-beb6-caacce708369)


The average length of a ride by the period of a casual member is 2.33 times longer than annual. 

![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/634a9bd3-c0ed-4556-87c2-6a9e531cb0d4)

Annual members much more actively ride bikes on workdays opposite to casual users. Even if the number of rides by casual users increases on weekends and decreases by annual users, the last one still uses bikes more often. 

![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/1b0809b4-5ea6-47c5-8758-234011d4b996)


The same picture, if to break down by a day. The average ride of casual users is lower in the middle of the week and increases on weekends. The average;ength of annual users during working days stays stable and also increases a bit on weekends.  

![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/14e38af2-62fa-4589-825b-d82427c1f5d5)

Casual riders prefer electric bikes to classic and some like docked bikes. Annual members use classic and electric bikes equaly. Also non of them prefers docked bikes at all.

![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/2e0dd15f-e2d8-4b43-a838-44acaacfd00b)


## Act

**Guidingquestions**
● What is your final conclusion based on your analysis? 
● How could your team and business apply your insights? 
● What next steps would you or your stakeholder stake based on your findings? 
● Is there additional data you could use to expand on your findings? 
**Keytasks**
1. Create your portfolio.
2. Add your case study.
3. Practice presenting your case study to afriend or family member.
**Deliverable**
Your top three recommendations based on your analysis

**Decision Making:**
Assist stakeholders in making informed decisions based on analysis.
Provide recommendations and potential courses of action.
**Feedback Loop:**
Gather feedback from stakeholders about the analysis.
Iterate on the analysis based on feedback if necessary.

