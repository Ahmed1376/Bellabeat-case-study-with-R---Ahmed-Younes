# Bellabeat-case-study-with-R---Ahmed-Younes
try1
Bellabeat case study
Ahmed,Younes
2022-06-13
Bellabeat company :
Bellabeat, a high-tech manufacturer of health-focused products for women. Bellabeat is a successful small company, but they have the potential to become a larger player in the global smart device market. Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart device fitness data could help unlock new growth opportunities for the company.

Ask for analysis
1- What are some trends in smart device usage? 2- How could these trends apply to Bellabeat customers? 3- How could these trends help influence Bellabeat marketing strategy?

Task
Identify potential opportunities for growth and recommendations for the Bellabeat marketing strategy improvement based on trends in smart device usage.

loading packages
install.packages("tidyverse")
WARNING: Rtools is required to build R packages but is not currently installed. Please download and install the appropriate version of Rtools before proceeding:

https://cran.rstudio.com/bin/windows/Rtools/
Installing package into ‘C:/Users/MBR/Documents/R/win-library/4.1’
(as ‘lib’ is unspecified)
trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.1/tidyverse_1.3.1.zip'
Content type 'application/zip' length 430232 bytes (420 KB)
downloaded 420 KB
Error in install.packages : cannot open file 'C:/Users/MBR/Documents/R/win-library/4.1/file380c245c537c/tidyverse/help/figures/logo.png': Permission denied
library(tidyverse) 
-- Attaching packages ------------------------------------------- tidyverse 1.3.1 --
âˆš ggplot2 3.3.5     âˆš purrr   0.3.4
âˆš tibble  3.1.6     âˆš dplyr   1.0.8
âˆš tidyr   1.2.0     âˆš stringr 1.4.0
âˆš readr   2.1.2     âˆš forcats 0.5.1
-- Conflicts ---------------------------------------------- tidyverse_conflicts() --
x dplyr::filter() masks stats::filter()
x dplyr::lag()    masks stats::lag()
library(dplyr)
library(skimr)
library(here)
here() starts at C:/Users/MBR/Desktop
library(lubridate)

Attaching package: ‘lubridate’

The following objects are masked from ‘package:base’:

    date, intersect, setdiff, union
library(janitor)

Attaching package: ‘janitor’

The following objects are masked from ‘package:stats’:

    chisq.test, fisher.test
library(ggplot2)
Import the data
activity <- read.csv("dailyActivity_merged.xlsx.csv", header = TRUE, sep = ",")
dailyCalory <- read.csv("dailyCalories_merged.xlsx.csv",header = TRUE, sep = ",")
dailyIntensities <- read.csv("dailyIntensities_merged.xlsx.csv",header = TRUE, sep = ",")
dailySteps <- read.csv("dailySteps_merged.xlsx.csv", header = TRUE, sep = ",")
heartrate <- read.csv("heartrate_seconds_merged.xlsx.csv", header = TRUE, sep = ",")
sleepDay <- read.csv("sleepDay_merged.xlsx.csv", header = TRUE, sep = ",") 
dailyIntensities <- read.csv("dailyIntensities_merged.xlsx.csv",header = TRUE, sep = ",")
Explore the data
n_distinct(activity$Id)
[1] 33
n_distinct(dailyCalory$Id)
[1] 33
n_distinct(dailySteps$Id)
[1] 33
n_distinct(heartrate$Id)
[1] 7
n_distinct(sleepDay$Id)
[1] 24
n_distinct(dailyIntensities$Id)
[1] 33
more exploring
activity %>% 
  summary()
       Id            ActivityDate         TotalSteps       Calories    TotalDistance  
 Min.   :1.504e+09   Length:940         Min.   :    0   Min.   :   0   Min.   : 0.00  
 1st Qu.:2.320e+09   Class :character   1st Qu.: 3790   1st Qu.:1828   1st Qu.: 2.60  
 Median :4.445e+09   Mode  :character   Median : 7406   Median :2134   Median : 5.25  
 Mean   :4.855e+09                      Mean   : 7638   Mean   :2304   Mean   : 5.49  
 3rd Qu.:6.962e+09                      3rd Qu.:10727   3rd Qu.:2793   3rd Qu.: 7.70  
 Max.   :8.878e+09                      Max.   :36019   Max.   :4900   Max.   :28.00  
 TrackerDistance  LoggedActivitiesDistance VeryActiveDistance ModeratelyActiveDistance
 Min.   : 0.000   Min.   :0.0000           Min.   : 0.000     Min.   :0.0000          
 1st Qu.: 2.620   1st Qu.:0.0000           1st Qu.: 0.000     1st Qu.:0.0000          
 Median : 5.245   Median :0.0000           Median : 0.210     Median :0.2400          
 Mean   : 5.475   Mean   :0.1082           Mean   : 1.503     Mean   :0.5675          
 3rd Qu.: 7.710   3rd Qu.:0.0000           3rd Qu.: 2.053     3rd Qu.:0.8000          
 Max.   :28.030   Max.   :4.9421           Max.   :21.920     Max.   :6.4800          
 LightActiveDistance SedentaryActiveDistance VeryActiveMinutes FairlyActiveMinutes
 Min.   : 0.000      Min.   :0.000000        Min.   :  0.00    Min.   :  0.00     
 1st Qu.: 1.945      1st Qu.:0.000000        1st Qu.:  0.00    1st Qu.:  0.00     
 Median : 3.365      Median :0.000000        Median :  4.00    Median :  6.00     
 Mean   : 3.341      Mean   :0.001606        Mean   : 21.16    Mean   : 13.56     
 3rd Qu.: 4.782      3rd Qu.:0.000000        3rd Qu.: 32.00    3rd Qu.: 19.00     
 Max.   :10.710      Max.   :0.110000        Max.   :210.00    Max.   :143.00     
 LightlyActiveMinutes SedentaryMinutes
 Min.   :  0.0        Min.   :   0.0  
 1st Qu.:127.0        1st Qu.: 729.8  
 Median :199.0        Median :1057.5  
 Mean   :192.8        Mean   : 991.2  
 3rd Qu.:264.0        3rd Qu.:1229.5  
 Max.   :518.0        Max.   :1440.0  
shorted activity (wanted to explore the 4 variables )
Activity_1 <- activity %>% 
  select(Id, TotalSteps, TotalDistance, Calories) 
more about activity

 Activity_1 %>%  
    summary() 
       Id              TotalSteps    TotalDistance      Calories   
 Min.   :1.504e+09   Min.   :    0   Min.   : 0.00   Min.   :   0  
 1st Qu.:2.320e+09   1st Qu.: 3790   1st Qu.: 2.60   1st Qu.:1828  
 Median :4.445e+09   Median : 7406   Median : 5.25   Median :2134  
 Mean   :4.855e+09   Mean   : 7638   Mean   : 5.49   Mean   :2304  
 3rd Qu.:6.962e+09   3rd Qu.:10727   3rd Qu.: 7.70   3rd Qu.:2793  
 Max.   :8.878e+09   Max.   :36019   Max.   :28.00   Max.   :4900  
Looking if there is any relationship :
  ggplot(data = Activity_1)+
    geom_point(mapping = aes(x= TotalSteps,y= TotalDistance, color=Calories))


NA
NA
The plot shows the correlation between Total steps, Total distance and calories. It shows a relationship :

The more steps ,the more distance,the more calories.
Sleep day
sleepDay %>% 
  summary()
       Id              SleepDay         TotalSleepRecords TotalMinutesAsleep
 Min.   :1.504e+09   Length:413         Min.   :1.000     Min.   : 58.0     
 1st Qu.:3.977e+09   Class :character   1st Qu.:1.000     1st Qu.:361.0     
 Median :4.703e+09   Mode  :character   Median :1.000     Median :433.0     
 Mean   :5.001e+09                      Mean   :1.119     Mean   :419.5     
 3rd Qu.:6.962e+09                      3rd Qu.:1.000     3rd Qu.:490.0     
 Max.   :8.792e+09                      Max.   :3.000     Max.   :796.0     
 TotalTimeInBed  loss_.time_beforeSleeping
 Min.   : 61.0   Min.   :  0.00           
 1st Qu.:403.0   1st Qu.: 17.00           
 Median :463.0   Median : 25.00           
 Mean   :458.6   Mean   : 39.17           
 3rd Qu.:526.0   3rd Qu.: 40.00           
 Max.   :961.0   Max.   :371.00           
want to know how long time spending on bed before sleeping, so I added a new column by make subtraction
sleepDay["time_onBed_before_sleeping"]<-sleepDay$TotalTimeInBed - sleepDay$TotalMinutesAsleep
Spot on time on bed before sleeping :
Stats_time_on_bed_before_sleeping <- sleepDay %>% 
 summarise(min(time_onBed_before_sleeping),
             mean(time_onBed_before_sleeping),
             max(time_onBed_before_sleeping)) 
r Sleep Day summary
 sleepDay %>% 
   select(TotalMinutesAsleep,
          TotalTimeInBed,
          time_onBed_before_sleeping) %>% 
   summary() 
 TotalMinutesAsleep TotalTimeInBed  time_onBed_before_sleeping
 Min.   : 58.0      Min.   : 61.0   Min.   :  0.00            
 1st Qu.:361.0      1st Qu.:403.0   1st Qu.: 17.00            
 Median :433.0      Median :463.0   Median : 25.00            
 Mean   :419.5      Mean   :458.6   Mean   : 39.17            
 3rd Qu.:490.0      3rd Qu.:526.0   3rd Qu.: 40.00            
 Max.   :796.0      Max.   :961.0   Max.   :371.00            
 
#### Plot Sleep Day

ggplot(data = sleepDay)+
   geom_point(mapping = aes(x= TotalTimeInBed, y= TotalMinutesAsleep,
                            color=time_onBed_before_sleeping)) 


Some discoveries :
the time before sleeping is high something, it looks like there is INSOMNIA with some participants .
I wonder if Bellabeat add instruction in Bellabeat`s App to guide the customers to avoid their INSOMNIA.
Explore the dailyIntensities :
dailyIntensities <- dailyIntensities %>% 
   select(Id, ActivityDay, SedentaryMinutes, LightlyActiveMinutes,
          FairlyActiveMinutes, VeryActiveMinutes)
more exploring :
dailyIntensities %>% 
   summary()
       Id            ActivityDay        SedentaryMinutes LightlyActiveMinutes
 Min.   :1.504e+09   Length:940         Min.   :   0.0   Min.   :  0.0       
 1st Qu.:2.320e+09   Class :character   1st Qu.: 729.8   1st Qu.:127.0       
 Median :4.445e+09   Mode  :character   Median :1057.5   Median :199.0       
 Mean   :4.855e+09                      Mean   : 991.2   Mean   :192.8       
 3rd Qu.:6.962e+09                      3rd Qu.:1229.5   3rd Qu.:264.0       
 Max.   :8.878e+09                      Max.   :1440.0   Max.   :518.0       
 FairlyActiveMinutes VeryActiveMinutes
 Min.   :  0.00      Min.   :  0.00   
 1st Qu.:  0.00      1st Qu.:  0.00   
 Median :  6.00      Median :  4.00   
 Mean   : 13.56      Mean   : 21.16   
 3rd Qu.: 19.00      3rd Qu.: 32.00   
 Max.   :143.00      Max.   :210.00   
dailyIntensities viz
ggplot(data = dailyIntensities)+
 geom_point(mapping = aes(x= SedentaryMinutes, y= VeryActiveMinutes )) +
   geom_smooth(mapping = aes(x= SedentaryMinutes, y= VeryActiveMinutes))
`geom_smooth()` using method = 'loess' and formula 'y ~ x'


Discovery showed :
The Sedentary time is the most common between the participants.
Merge and explore Intensity combined with Sleep Day.
intensity_sleepDay <- merge(dailyIntensities,sleepDay, by="Id", all = TRUE) 
Explore the active time minutes :
Stats_Very_Active_Minuts <- dailyIntensities %>% 
summarise(min(VeryActiveMinutes),
          mean(VeryActiveMinutes), 
          max(VeryActiveMinutes))
Some interesting discoveries :
the average and maximum of time on bed before sleeping are.
clearly greater than average and maximum of Very active minutes.
Finished
Thanks, and appreciated for interest on my first case study.
I am beginner and I know it is poor something case study.
