Coursera: Reproducible Research - Peer Assignment 1 (May 2014)
==============================================================

```r
# Sys.getlocale(category = 'LC_ALL')
Sys.setlocale("LC_TIME", "English_US")
```

```
## [1] "English_United States.1252"
```


## Loading and preprocessing the data

### Load the data

```r
activity_original <- read.csv("./activity.csv", sep = ",", header = TRUE, quote = "", 
    strip.white = TRUE, stringsAsFactors = FALSE, col.names = c("steps", "date", 
        "interval"))
## activity$date <- as.Date(activity$date,format='%Y-%m-%d')
```

### Process/transform the data (if necessary) into a format suitable for your analysis

```r
activity <- activity_original[!is.na(activity_original$steps), ]
## head(activity) activity$date <- as.Date(activity$date,format='%Y-%m-%d')
```



## What is mean total number of steps taken per day?
### Make a histogram of the total number of steps taken each day


```r
library(data.table)
ActTable <- data.table(activity)
## names(ActTable)
ActTotalPerDayTable <- ActTable[, sum(steps), by = date]
setNames(ActTotalPerDayTable, c("date", "total_steps"))
```

```
##             date total_steps
##  1: "2012-10-02"         126
##  2: "2012-10-03"       11352
##  3: "2012-10-04"       12116
##  4: "2012-10-05"       13294
##  5: "2012-10-06"       15420
##  6: "2012-10-07"       11015
##  7: "2012-10-09"       12811
##  8: "2012-10-10"        9900
##  9: "2012-10-11"       10304
## 10: "2012-10-12"       17382
## 11: "2012-10-13"       12426
## 12: "2012-10-14"       15098
## 13: "2012-10-15"       10139
## 14: "2012-10-16"       15084
## 15: "2012-10-17"       13452
## 16: "2012-10-18"       10056
## 17: "2012-10-19"       11829
## 18: "2012-10-20"       10395
## 19: "2012-10-21"        8821
## 20: "2012-10-22"       13460
## 21: "2012-10-23"        8918
## 22: "2012-10-24"        8355
## 23: "2012-10-25"        2492
## 24: "2012-10-26"        6778
## 25: "2012-10-27"       10119
## 26: "2012-10-28"       11458
## 27: "2012-10-29"        5018
## 28: "2012-10-30"        9819
## 29: "2012-10-31"       15414
## 30: "2012-11-02"       10600
## 31: "2012-11-03"       10571
## 32: "2012-11-05"       10439
## 33: "2012-11-06"        8334
## 34: "2012-11-07"       12883
## 35: "2012-11-08"        3219
## 36: "2012-11-11"       12608
## 37: "2012-11-12"       10765
## 38: "2012-11-13"        7336
## 39: "2012-11-15"          41
## 40: "2012-11-16"        5441
## 41: "2012-11-17"       14339
## 42: "2012-11-18"       15110
## 43: "2012-11-19"        8841
## 44: "2012-11-20"        4472
## 45: "2012-11-21"       12787
## 46: "2012-11-22"       20427
## 47: "2012-11-23"       21194
## 48: "2012-11-24"       14478
## 49: "2012-11-25"       11834
## 50: "2012-11-26"       11162
## 51: "2012-11-27"       13646
## 52: "2012-11-28"       10183
## 53: "2012-11-29"        7047
##             date total_steps
```

```r
png(filename = "./figures/TotalStepsPerDayHist.png", width = 480, height = 480)
hist(ActTotalPerDayTable$total_steps, main = "Total Number of steps per day", 
    xlab = "Total Number of steps", )
# dev.copy(png, file = './figures/TotalStepsPerDayHist.png')
dev.off()
```

```
## pdf 
##   2
```

![Total Steps Per Day](figures/TotalStepsPerDayHist.png) 


### Calculate the *mean* and *median* total number of steps taken per day


```r
summary(ActTotalPerDayTable)
```

```
##      date            total_steps   
##  Length:53          Min.   :   41  
##  Class :character   1st Qu.: 8841  
##  Mode  :character   Median :10765  
##                     Mean   :10766  
##                     3rd Qu.:13294  
##                     Max.   :21194
```


## What is the average daily activity pattern?

### Make a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r
ActTMeanPer5minTable <- ActTable[, mean(steps), by = interval]
setNames(ActTMeanPer5minTable, c("interval", "average_steps"))
```

```
##      interval average_steps
##   1:        0       1.71698
##   2:        5       0.33962
##   3:       10       0.13208
##   4:       15       0.15094
##   5:       20       0.07547
##  ---                       
## 284:     2335       4.69811
## 285:     2340       3.30189
## 286:     2345       0.64151
## 287:     2350       0.22642
## 288:     2355       1.07547
```

```r
png(filename = "./figures/AvgStepsPerInterval.png", width = 480, height = 480)
plot(ActTMeanPer5minTable, type = "l", main = "Time series plot of the average number of steps taken")
# dev.copy(png, file = './figures/AvgStepsPerInterval.png')
dev.off()
```

```
## pdf 
##   2
```


![Average Steps Per Interval](figures/AvgStepsPerInterval.png) 

### Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
summary(ActTMeanPer5minTable)
```

```
##     interval    average_steps   
##  Min.   :   0   Min.   :  0.00  
##  1st Qu.: 589   1st Qu.:  2.49  
##  Median :1178   Median : 34.11  
##  Mean   :1178   Mean   : 37.38  
##  3rd Qu.:1766   3rd Qu.: 52.83  
##  Max.   :2355   Max.   :206.17
```

```r
indexMax <- ActTMeanPer5minTable[, which.max(average_steps)]
ActTMeanPer5minTable[indexMax, ]
```

```
##    interval average_steps
## 1:      835         206.2
```


## Imputing missing values

Note that there are a number of days/intervals. The presence of missing days may introduce bias into some calculations or summaries of the data.

### Calculate and report the total number of missing values in the dataset


```r
table(is.na(activity_original$steps))
```

```
## 
## FALSE  TRUE 
## 15264  2304
```


### Devise a strategy for filling in all of the missing values in the dataset. 
The missing data will be replaced by the mean of the same 5 minute interval.

### Create a new dataset that is equal to the original dataset but with the missing data filled in.


```r
meanPerInterval <- as.data.frame(ActTMeanPer5minTable[, ])
names(meanPerInterval)
```

```
## [1] "interval"      "average_steps"
```

```r
activity_filled <- merge(x = activity_original, y = meanPerInterval, by.x = "interval", 
    all = TRUE)
names(activity_filled)
```

```
## [1] "interval"      "steps"         "date"          "average_steps"
```

```r
activity_filled$steps[is.na(activity_filled$steps)] <- activity_filled$average_steps[is.na(activity_filled$steps)]
head(activity_filled)
```

```
##   interval steps         date average_steps
## 1        0 1.717 "2012-10-01"         1.717
## 2        0 0.000 "2012-11-23"         1.717
## 3        0 0.000 "2012-10-28"         1.717
## 4        0 0.000 "2012-11-06"         1.717
## 5        0 0.000 "2012-11-24"         1.717
## 6        0 0.000 "2012-11-15"         1.717
```


### Make a histogram of the total number of steps taken each day 

```r
FilledTable <- data.table(activity_filled)
ActTotalPerDayFilledTable <- FilledTable[, sum(steps), by = date]
setNames(ActTotalPerDayFilledTable, c("date", "total_steps"))
```

```
##             date total_steps
##  1: "2012-10-01"       10766
##  2: "2012-11-23"       21194
##  3: "2012-10-28"       11458
##  4: "2012-11-06"        8334
##  5: "2012-11-24"       14478
##  6: "2012-11-15"          41
##  7: "2012-10-20"       10395
##  8: "2012-11-16"        5441
##  9: "2012-11-07"       12883
## 10: "2012-11-25"       11834
## 11: "2012-11-04"       10766
## 12: "2012-11-08"        3219
## 13: "2012-10-12"       17382
## 14: "2012-10-30"        9819
## 15: "2012-11-26"       11162
## 16: "2012-10-04"       12116
## 17: "2012-11-27"       13646
## 18: "2012-10-31"       15414
## 19: "2012-11-18"       15110
## 20: "2012-10-05"       13294
## 21: "2012-10-14"       15098
## 22: "2012-10-23"        8918
## 23: "2012-11-19"        8841
## 24: "2012-10-11"       10304
## 25: "2012-10-15"       10139
## 26: "2012-10-06"       15420
## 27: "2012-11-11"       12608
## 28: "2012-11-29"        7047
## 29: "2012-11-02"       10600
## 30: "2012-10-07"       11015
## 31: "2012-11-03"       10571
## 32: "2012-11-30"       10766
## 33: "2012-11-21"       12787
## 34: "2012-10-02"         126
## 35: "2012-10-26"        6778
## 36: "2012-11-22"       20427
## 37: "2012-11-28"       10183
## 38: "2012-11-13"        7336
## 39: "2012-10-18"       10056
## 40: "2012-10-27"       10119
## 41: "2012-11-14"       10766
## 42: "2012-10-22"       13460
## 43: "2012-10-10"        9900
## 44: "2012-10-19"       11829
## 45: "2012-11-09"       10766
## 46: "2012-10-17"       13452
## 47: "2012-10-16"       15084
## 48: "2012-10-29"        5018
## 49: "2012-11-01"       10766
## 50: "2012-10-21"        8821
## 51: "2012-11-10"       10766
## 52: "2012-10-03"       11352
## 53: "2012-11-17"       14339
## 54: "2012-11-12"       10765
## 55: "2012-10-25"        2492
## 56: "2012-10-13"       12426
## 57: "2012-10-24"        8355
## 58: "2012-10-08"       10766
## 59: "2012-11-20"        4472
## 60: "2012-11-05"       10439
## 61: "2012-10-09"       12811
##             date total_steps
```

```r
png(filename = "./figures/TotalStepsPerDaySimulatedHist.png", width = 480, height = 480)
hist(ActTotalPerDayFilledTable$total_steps, main = "Total Number of steps per day (simulated)", 
    xlab = "Total Number of steps", )
# dev.copy(png, file = './figures/TotalStepsPerDaySimulatedHist.png')
dev.off()
```

```
## pdf 
##   2
```


![Total Steps Per Day (Simulated)](figures/TotalStepsPerDaySimulatedHist.png) 

### Calculate and report the mean and median total number of steps taken per day. 

```r
summary(ActTotalPerDayFilledTable)
```

```
##      date            total_steps   
##  Length:61          Min.   :   41  
##  Class :character   1st Qu.: 9819  
##  Mode  :character   Median :10766  
##                     Mean   :10766  
##                     3rd Qu.:12811  
##                     Max.   :21194
```



### Do these values differ from the estimates from the first part of the assignment? 
Without missing values:
- Median :10765  
- Mean   :10766  


By Imputing missing values with the mean of the 5 minute interval:
- Median :10766  
- Mean   :10766  

### What is the impact of imputing missing data on the estimates of the total daily number of steps?
By Imputing missing values with the mean of the 5 minute interval **the value of median gets equal to the value of mean**.

## Are there differences in activity patterns between weekdays and weekends?

Use the dataset with the filled-in missing values for this part.

### Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

```r
# activity_filled$DayType <-
# strftime((strptime(as.character(activity_filled$date),format='\'%Y-%m-%d\'')),format='%A')
activity_filled$DayType <- weekdays(strptime(as.character(activity_filled$date), 
    format = "\"%Y-%m-%d\""))

head(activity_filled)
```

```
##   interval steps         date average_steps  DayType
## 1        0 1.717 "2012-10-01"         1.717   Monday
## 2        0 0.000 "2012-11-23"         1.717   Friday
## 3        0 0.000 "2012-10-28"         1.717   Sunday
## 4        0 0.000 "2012-11-06"         1.717  Tuesday
## 5        0 0.000 "2012-11-24"         1.717 Saturday
## 6        0 0.000 "2012-11-15"         1.717 Thursday
```

```r
activity_filled$DayType[activity_filled$DayType %in% c("Saturday", "Sunday")] <- "weekend"
activity_filled$DayType[!(activity_filled$DayType %in% c("weekend"))] <- "weekday"
head(activity_filled)
```

```
##   interval steps         date average_steps DayType
## 1        0 1.717 "2012-10-01"         1.717 weekday
## 2        0 0.000 "2012-11-23"         1.717 weekday
## 3        0 0.000 "2012-10-28"         1.717 weekend
## 4        0 0.000 "2012-11-06"         1.717 weekday
## 5        0 0.000 "2012-11-24"         1.717 weekend
## 6        0 0.000 "2012-11-15"         1.717 weekday
```

```r
activity_filled$DayType <- as.factor(activity_filled$DayType)
summary(activity_filled)
```

```
##     interval        steps           date           average_steps   
##  Min.   :   0   Min.   :  0.0   Length:17568       Min.   :  0.00  
##  1st Qu.: 589   1st Qu.:  0.0   Class :character   1st Qu.:  2.49  
##  Median :1178   Median :  0.0   Mode  :character   Median : 34.11  
##  Mean   :1178   Mean   : 37.4                      Mean   : 37.38  
##  3rd Qu.:1766   3rd Qu.: 27.0                      3rd Qu.: 52.83  
##  Max.   :2355   Max.   :806.0                      Max.   :206.17  
##     DayType     
##  weekday:12960  
##  weekend: 4608  
##                 
##                 
##                 
## 
```


### Make a panel plot containing a time series plot of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis).

```r
library(lattice)
png(filename = "./figures/AvgStepsWeekendsVsWeekdays.png", width = 480, height = 480)
xyplot(average_steps ~ interval | DayType, data = activity_filled, type = "l", 
    layout = c(1, 2))
# dev.copy(png, file = './figures/AvgStepsWeekendsVsWeekdays.png')
dev.off()
```

```
## pdf 
##   2
```


![Average Steps Per Interval (Weekends vs Weekdays)](figures/AvgStepsWeekendsVsWeekdays.png) 
