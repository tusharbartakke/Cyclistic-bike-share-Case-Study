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
