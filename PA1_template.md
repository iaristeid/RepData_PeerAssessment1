Coursera: Reproducible Research - Peer Assignment 1 (May 2014)
==============================================================

```r
Sys.getlocale(category = "LC_ALL")
```

```
## [1] "LC_COLLATE=Greek_Greece.1253;LC_CTYPE=Greek_Greece.1253;LC_MONETARY=Greek_Greece.1253;LC_NUMERIC=C;LC_TIME=Greek_Greece.1253"
```

```r
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





















