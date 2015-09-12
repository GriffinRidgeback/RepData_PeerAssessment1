# Reproducible Research: Peer Assessment 1
September 12, 2015  



This assignment makes use of data from a personal activity monitoring
device. This device collects data at 5 minute intervals throughout the
day. The data consists of two months of data from an anonymous
individual collected during the months of October and November, 2012
and include the number of steps taken in 5 minute intervals each day.

## Data

The data for this assignment can be downloaded from the course web
site:

* Dataset: [Activity monitoring data](https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip) [52K]

The variables included in this dataset are:

* **steps**: Number of steps taking in a 5-minute interval (missing values are coded as `NA`)

* **date**: The date on which the measurement was taken in YYYY-MM-DD format

* **interval**: Identifier for the 5-minute interval in which measurement was taken

The dataset is stored in a comma-separated-value (CSV) file and there are a total of 17,568 observations in this dataset (61 days * 288 time intervals per day).

## Loading and preprocessing the data

The first step in the project, after downloading the data and unpacking it to the local file system, is to read it into R.  Since the data has been provided as a CSV file, the logical method to use would be read.csv; stringsAsFactors is set to FALSE so that dates are not factors but actual date strings.


```r
activity  <- read.csv("./data/activity.csv", stringsAsFactors = F)
```

## What is mean total number of steps taken per day?

The first question to be asked of this dataset is how many total steps were taken in a 24-hour period?  The dplyr package will be useful for this and other questions, so loading that library will be the first step.


```r
suppressMessages(library(dplyr))
library("dplyr")
```

Next, since the data contains NA values, create a second dataset with those values removed.  We want to consider only those days for which step counts were recorded.


```r
complete_days_only <- activity[complete.cases(activity), ]
```

To perform the analysis required for this part of the assignment, use the `group_by` and `summarise` functions from `dplyr`.  The *sum*, *mean*, and *median* can be calculated using `summarise` once the data has been organized by **date** using `group_by`.


```r
step_summary  <- complete_days_only %>% 
  group_by(date) %>% 
  summarise(daily_step_count = sum(steps), mean_step = mean(steps), median_step = median(steps))
```

Now plot the results for total number of steps taken per day.  The histogram will show the range of totals across the x-axis (from 0 to the maximum number of steps recorded for a single day) while along the y-axis will be shown the number of days that fell within each range (i.e., between 0 and 5000, there were 5 days which had totals in that range).


```r
hist(step_summary$daily_step_count, 
    main = "Histogram of total steps per day",
    xlab = "Range of step totals",
    ylab = "Number of totals in range",
    border = "green",
    col = heat.colors(5),
    las = 1,
    ylim = c(0, 30))
```

![](PA1_template_files/figure-html/plot_count-1.png) 

Since the mean and median were calculated at the same time as the sum, they simply can be printed at this point.


```r
summary(step_summary$mean_step)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.1424 30.7000 37.3800 37.3800 46.1600 73.5900
```

```r
summary(step_summary$median_step)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       0       0       0       0       0       0
```


## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
