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

● What tools are you choosing and why? 
● Have you ensured your data’s integrity? 
● What steps have you taken to ensure that your data is clean? 
● How can you verify that your data is clean and ready to analyze? 
● Have you documented your cleaning processes so you can review and share those results?

**Data Transformation:**
Normalize or standardize variables if necessary.
Create derived variables if they provide meaningful insights.
**Data Integration:**
Merge different datasets, if applicable.
Ensure consistency in variables across datasets.


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

The total number of observations is 5133286. It's 24,22%. Have to investigate if there are any patterns.

Questions are:
* Which columns have *nuul* values?
* Do we need all that data for the current business task, or can we drop it?
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
  COUNT(*) - COUNT(start_lng) AS start_lng,
  COUNT(*) - COUNT(end_lat) AS end_lat,
  COUNT(*) - COUNT(end_lng) AS end_lng,
  COUNT(*) - COUNT(member_casual) AS member_casual
FROM
  `cyclistic_tripdata.tripdata_all`
```
![image](https://github.com/artempohribnyi/case_study_cyclistic/assets/113499718/04621631-7344-45da-8355-e59dc5cb1a8a)

After discussions with the team and Lily, we decided that for the purpose of the current business task.
