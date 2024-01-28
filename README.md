# Cyclistic-bike-share-Case-Study
![Alt text](https://github.com/tusharbartakke/Cyclistic-bike-share-Case-Study/blob/main/Insights%20images/logo.png?raw=true)

Crafting strategic marketing initiatives aimed at converting occasional cyclists into dedicated members for Cyclistic, leveraging advanced data analytics techniques.

I have recently completed the Google Data Analytics Professional Certificate on Coursera. My final capstone project involves analyzing a public dataset for a fictional company, utilizing the R programming language for data analysis and Tableau for visualizations. Excited to share the insights gained from this endeavor!

## About the Company

In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geo tracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime. Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

### Data Analysis Steps

The following data analysis steps will be followed:
## 1. **Ask**: 
The questions that need to be answered are:
    - How do annual members and casual riders use Cyclistic bikes differently?
    - Why would casual riders buy Cyclistic annual memberships?
    - How can Cyclistic use digital media to influence casual riders to become members?

## 2. **Prepare**: 
The dataset follows the ROCCC Analysis as described below:
    - Reliable - yes, not biased
    - Original - yes, can locate the original public data
    - Comprehensive - yes, not missing important information
    - Current - yes, updated monthly
    - Cited - yes
The data is located on AWS file server where the zip files can be downloaded and named correctly. The data dated from Jan 2022 to Dec 2022 was downloaded and stored locally on desktop for the data cleaning and further analytical processes. The data is current that the usefulness of data is secured. The data is indexed by month (2020-2023) or by fiscal quarters (2015-2020).
The public data has been made available by Motivate International Inc. under license ![Alt textr](https://ride.divvybikes.com/data-license-agreement). The downloaded original dataset from Cyclistic is reliable - accurate and comprehensive records with complete and unbiased information and it proven fit for use. 
Note: Data-privacy issues prohibit data-user from using riders’ personally identifiable information. All rider personal information is hidden or kept private to Cyclistic only. Data users won’t be able connect the pass purchases to credit card numbers to o determine if casual riders live in the Cyclistic service area or if they have purchased multiple single passes.

3. **Installing and loading required packages**

```R
install.packages("tidyverse")
install.packages("lubridate")
install.packages("janitor")
library(tidyverse)
library(lubridate)
library(janitor)
```
## Importing data to data-frame in R

```R
jan22 <- read.csv("202201-divvy-tripdata.csv")
feb22 <- read.csv("202202-divvy-tripdata.csv")
mar22 <- read.csv("202203-divvy-tripdata.csv")
apr22 <- read.csv("202204-divvy-tripdata.csv")
may22 <- read.csv("202205-divvy-tripdata.csv")
jun22 <- read.csv("202206-divvy-tripdata.csv")
jul22 <- read.csv("202207-divvy-tripdata.csv")
aug22 <- read.csv("202208-divvy-tripdata.csv")
sep22 <- read.csv("202209-divvy-publictripdata.csv")
oct22 <- read.csv("202210-divvy-tripdata.csv")
nov22 <- read.csv("202211-divvy-tripdata.csv")
dec22 <- read.csv("202212-divvy-tripdata.csv")
```
## Merging data into a single data frame

```R
bikerides <- rbind(jan22,feb22,mar22,apr22,may22,jun22,jul22,aug22,sep22,oct22,nov22,dec22)
dim(bikerides)
[1] 5667717      13 ## contains 5723532 rows and 13 columns
```

## 3. **Process** :
Cleaning and Preparing Data
Checking and removing empty col and rows using janitor

```R
bikerides <- janitor::remove_empty(dat = bikerides, which = c("cols"))
bikerides <- janitor::remove_empty(dat = bikerides, which = c("rows"))
dim(bikerides)
5667717      13
```
Converting started at and ended at columns to date data type
```R
bikerides$started_at <- lubridate::as_datetime(bikerides$started_at)
bikerides$ended_at <- lubridate::as_datetime(bikerides$ended_at)
```
Getting the start time and end time components from date-time
```R
bikerides$start_hr <- lubridate::hour(bikerides$started_at)
bikerides$end_hr <- lubridate::hour(bikerides$ended_at) 
```
Getting the start date and end date
```R
bikerides$start_date <- lubridate::date(bikerides$started_at)
bikerides$end_date <- lubridate::date(bikerides$ended_at)
```
Getting the ride length and changing diff date to numeric format, the duration is in seconds.
```R
bikerides$ride_length_hr <- difftime(bikerides$ended_at,bikerides$started_at,unit = c("hours"))
bikerides$ride_length_hr<-as.numeric(bikerides$ride_length_hr) ##changing diff date to numeric)
```
Getting the trip duration in mins
```R
bikerides$ride_length_min <- difftime(bikerides$ended_at,bikerides$started_at,units = c("mins"))
bikerides$ride_length_min <- as.numeric(bikerides$ride_length_min)
```
Removing row which have a trip duration equal to or less than 0
```R
bikerides_v2 <- na.omit(bikerides[!(bikerides$start_station_name == "HQ QR" | bikerides$length<=0),])
dim(bikerides_v2)
5661328      21
colSums(is.na(bikerides_v2))
```
Assigning days of the week to start and end date
```R
bikerides_v2$start_day <- wday(bikerides_v2$start_date, label = TRUE, abbr = FALSE)
bikerides_v2$end_day <- wday(bikerides_v2$end_date, label = TRUE, abbr = FALSE)
dim(bikerides_v2)
5661328      23
```
Exporting the end result as CSV to work on Tableau
```R
write.csv(bikerides_v2, "cyclistproject.csv", row.names = FALSE)
```
## 4. **Analyze**: 
Key tasks:
    - Aggregate your data so it’s useful and accessible.
    - Organize and format your data.
    - Perform calculations.
    - Identify trends and relationships.
avg trip durations as well as the maximun trip duration
```R
cat('average duration of rides is : ', mean(bikerides_v2$ride_length_min), "mins")
average duration of rides is :  16.3314 mins

cat('maximum duration of rides is : ', round(max(bikerides_v2$ride_length_min)/60/24), "days")
maximum duration of rides is :  24 days
```
## Comparision between Casual and members

![Alt text](https://github.com/tusharbartakke/Cyclistic-bike-share-Case-Study/blob/main/Insights%20images/Sheet%201.png?raw=true)

**We can see that we have majority in the member type.**

## Trip duration distribution

![Alt text](https://github.com/tusharbartakke/Cyclistic-bike-share-Case-Study/blob/main/Insights%20images/Sheet%202.png?raw=true)

**For casual clients we can see that the maximum duration of trip is during the weekends
For member clients we can see classic bike rides and electric bike rides both happening on Tuesday, Thursday and Wednesday.**

##  Average ride length of member type

![Alt text](https://github.com/tusharbartakke/Cyclistic-bike-share-Case-Study/blob/main/Insights%20images/Sheet%203(updated).png?raw=true)

**The Avg ride time of casual clients is more compared to the member clients.**

## Bike types used by Different clients 

![Alt text](https://github.com/tusharbartakke/Cyclistic-bike-share-Case-Study/blob/main/Insights%20images/Sheet%204.png?raw=true)

**Classic bikes are preferred by both casual and member clients of the company followed by electric bikes.
The member does not use the docked bikes and casual users also use them very less compared to classic and electric bikes.**

## Daywise trip Distribution for casual and members

![Alt text](https://github.com/tusharbartakke/Cyclistic-bike-share-Case-Study/blob/main/Insights%20images/Sheet%205.png?raw=true)

**From the above we can conclude that casual clients use the service majorly over the weekends and the members use the service throughout the week but with slight increase during the weekdays.**

## Number of rides and average ride length(monthly)

![Alt text](https://github.com/tusharbartakke/Cyclistic-bike-share-Case-Study/blob/main/Insights%20images/Sheet%207.png?raw=true)

**The above shows us the number of rides taken by both client types monthly, we can see that the number of rides and avg ride duration dips during the colder months. This can be due to the cold weather and christmas festival during which people prefer to stay indoors or travel using a car or other public transportation.**

## 5. **Share** :
The graphs used can be viewed from my [Tableu](https://public.tableau.com/app/profile/tushar.bartakke/viz/CyclisticBike-sharecasestudy_17060183363860/Dashboard1#1)
![Alt text](https://github.com/tusharbartakke/Cyclistic-bike-share-Case-Study/blob/main/Insights%20images/Dashboard%201.png?raw=true)

## 6. **Act** :

   - Numbers of Casual riders are more in comparision to the members.
   - To convert casual riders into annual members we target casual riders and provide more discounts and offers for membership.
   - We can increase rental prices for weekends for casual rides so that they have certain incentive to convert to members.
   - we can Push Cyclistic's advertisement online during the weekends to target casual users.
   - WE ca encourage casual users to have membership by lowering the renting price of bikes (member's deal) for weekend.

