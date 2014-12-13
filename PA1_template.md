# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data


```r
activity <- read.csv("activity.csv", colClasses = c("numeric","character","numeric"))

activity$date <- as.Date(activity$date, "%Y-%m-%d")

head(activity)
```

```
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
```

## What is mean total number of steps taken per day?


```r
StepsTotal <- aggregate(steps ~ date, data = activity, sum, na.rm = TRUE)

hist(StepsTotal$steps, main = "Total steps by day", xlab = "day", col = "red")
```

![](./PA1_template_files/figure-html/unnamed-chunk-2-1.png) 


## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
