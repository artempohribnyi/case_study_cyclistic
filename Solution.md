# How Does a Bike-Share Navigate Speedy Success?

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

So, **the business task** is to understand how annual members and casual riders use Cyclistic bikes differently. And if we can use those insights in designing marketing strategies aimed at converting casual riders into annual members, this involves analyzing the historical bike trip data to uncover patterns and differences in usage between these two user groups.

## Consider Key Stakeholders:

**Lily Moreno (Director of Marketing and Manager):** She is interested in this data to design effective marketing strategies. Her goal is to maximize the number of annual memberships for the company's future success.

**Cyclistic Marketing Analytics Team:** Responsible for collecting, analyzing, and reporting data to guide the marketing strategy. They will be directly involved in the analysis and interpretation of the bike trip data.

**Cyclistic Executive Team:** They are the decision-makers who will approve or reject the recommended marketing program. They need comprehensive and compelling insights to support decision-making.

**Cyclistic Finance Analysts:** Have concluded that annual members are more profitable than casual riders. The analysis will provide data-backed insights into the profitability of different customer segments.

**Casual Riders and Annual Members:** Understanding their usage patterns is crucial to tailor marketing strategies that encourage casual riders to become annual members.

**Digital Media Team (potentially):** Depending on the findings, insights may guide the digital media team on how to target and influence casual riders effectively.


## Prepare
 ● Where is your data located? 
 ● How is the data organized? 
 ● Are there issues with bias or credibility in this data? Does your data ROCCC? 
 ● How are you addressing licensing, privacy, security, and accessibility? 
 ● How did you verify the data’s integrity? 
 ● How does it help you answer your question? 
 ● Are there any problems with the data?

**Data Collection:**
Identify relevant data sources.
Gather raw data from diverse sources.
**Data Understanding:**
Familiarize yourself with the data structure.
Document data sources, formats, and any preliminary observations.
**Data Cleaning:**
Identify and handle missing or incomplete data.
Remove duplicates and outliers.
Standardize formats and units.


Following the task, I downloaded trip historical data from the [public repository](https://divvy-tripdata.s3.amazonaws.com/index.html) where it is located.

## Official documentation:

Here you'll find Cyclistic's trip data for public use. So whether you're a policymaker, transportation professional, web developer, designer, or just plain curious, feel free to download it, map it, animate it, or bring it to life!

Note: This data is provided according to the [Divvy Data License Agreement](https://www.divvybikes.com/data-license-agreement) and released on a monthly schedule.

## The Data

Each trip is anonymized and includes:

* Trip start date and time
* Trip end day and time
* Trip start station
* Trip end station
* Rider type (Member, Single Ride, and Day Pass)
  
The data has been processed to remove trips that are taken by staff as they service and inspect the system and any trips that were below 60 seconds in length (potentially false starts or users trying to re-dock a bike to ensure it was secure).


I think the last 10 months, from January to October 2023, will be enough to complete the business task. Since the data is released on a monthly schedule, each month is in a separate zip file.

To ensure data integrity, I downloaded all files to a separate folder, *cyclistic_tripdata*, and, keeping the original files and using appropriate file-naming conventions, unziped them to a subfolder, *2023_tripdata*.

Datasets can show a lot of interesting information. But to be sure, I chose the right data that can actually help me solve a problem I decided to take a glance, using Excel to open the smallest-sized file. The text-to-column option helped to create a table since it's in CSV format. Sorted by start_at. So, there is time series data to analyze trends over time or to analyze different patterns of use between casuals and members, like average ride time. Count the number of rides and try to understand the preferred bike type, weekdays, and start stations.

I mentioned that there are some blanks in a data set, and they'll need some cleaning for further analysis. And since just this data set, the smallest one, contains 190302 rows, it would be great to manipulate all data sets in one place, so I decided that BigQuery is better for this purpose.



I created a new project in Big Quarry. I created a new data set, cyclistic_tripdata. I unpacked and loaded each data set into Big Quarry.
