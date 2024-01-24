# Cyclistic-bike-share-Case-Study
Crafting strategic marketing initiatives aimed at converting occasional cyclists into dedicated members for Cyclistic, leveraging advanced data analytics techniques.

I have recently completed the Google Data Analytics Professional Certificate on Coursera. My final capstone project involves analyzing a public dataset for a fictional company, utilizing the R programming language for data analysis and Tableau for visualizations. Excited to share the insights gained from this endeavor!

## About the Company

In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geo tracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime. Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

### Data Analysis Steps

The following data analysis steps will be followed:
1. **Ask**: The questions that need to be answered are:
    - How do annual members and casual riders use Cyclistic bikes differently?
    - Why would casual riders buy Cyclistic annual memberships?
    - How can Cyclistic use digital media to influence casual riders to become members?

2. **Prepare**: The dataset follows the ROCCC Analysis as described below:
    - Reliable - yes, not biased
    - Original - yes, can locate the original public data
    - Comprehensive - yes, not missing important information
    - Current - yes, updated monthly
    - Cited - yes

    I will be using the public dataset located [here](link_to_dataset). The data has been made available by Motivate International Inc. under [this license](link_to_license).

    Key Tasks Followed:
    - Downloaded data and copies have been stored on the computer.
    - Downloaded the data for April 21-March 22 Period.
    - The data is in CSV (comma-separated values) format, and there are a total of 13 columns.

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

## Process
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
3. **Analyze**: Key tasks:
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
![image](https://github.com/tusharbartakke/Cyclistic-bike-share-Case-Study/assets/151425512/1a323a03-2cab-4bcf-963d-ab95257760d6)
