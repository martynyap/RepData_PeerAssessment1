# Reproducible Research Assignment 1
Martyn Yap  
14 December 2014  
### Title: Reproducible Research Peer Assessment 1 

#### Qn 1) Loading and preprocessing the data

```r
ds <- read.csv("activity.csv", colClasses = c("numeric", "character", "numeric"))
ds$date <- as.Date(ds$date, "%Y-%m-%d")
```

#### Qn 2) What is mean total number of steps taken per day?

```r
TotalSteps <- aggregate(steps ~ date, data = ds, sum, na.rm = TRUE)
hist(TotalSteps$steps, main = " ", xlab = "day", col = "green")
```

![](./PA1_template_files/figure-html/unnamed-chunk-2-1.png) 
the mean is...

```r
mean(TotalSteps$steps)
```

```
## [1] 10766.19
```
the median is...

```r
median(TotalSteps$steps)
```

```
## [1] 10765
```

#### Qn 3) What is the average daily activity pattern?

```r
ts <- tapply(ds$steps, ds$interval, mean, na.rm = TRUE)
plot(row.names(ts), ts, type = "l", xlab = "5-min interval", ylab = "Average across all Days", main = " ", col = "green")
```

![](./PA1_template_files/figure-html/unnamed-chunk-5-1.png) 
the 5-minute interval is...

```r
max_interval <- which.max(ts)
names(max_interval)
```

```
## [1] "835"
```

#### Qn 4) Inputing missing values
the total number of missing values is...

```r
ds_NA <- sum(is.na(ds))
ds_NA
```

```
## [1] 2304
```
filling in all of the missing values...

```r
AverageSteps <- aggregate(steps ~ interval, data = ds, FUN = mean)
filling_in <- numeric()
for (i in 1:nrow(ds)) {
    obs <- ds[i, ]
    if (is.na(obs$steps)) {
        steps <- subset(AverageSteps, interval == obs$interval)$steps
    } else {
        steps <- obs$steps
    }
    filling_in <- c(filling_in, steps)
}
```
the new dataset...

```r
ds2 <- ds
ds2$steps <- filling_in
```

```r
TotalSteps2 <- aggregate(steps ~ date, data = ds2, sum, na.rm = TRUE)
hist(TotalSteps2$steps, main = " ", xlab = "day", col = "green")
```

![](./PA1_template_files/figure-html/unnamed-chunk-10-1.png) 
the mean is...

```r
mean(TotalSteps2$steps)
```

```
## [1] 10766.19
```
the median is...

```r
median(TotalSteps2$steps)
```

```
## [1] 10766.19
```
My Observation: Mean is the same but the median differs slightly.

#### Qn 5)Are there differences in activity patterns between weekdays and weekends?

```r
library(lattice)
day <- weekdays(ds$date)
daylevel <- vector()
for (i in 1:nrow(ds)) {
    if (day[i] == "Saturday") {
        daylevel[i] <- "Weekend"
    } else if (day[i] == "Sunday") {
        daylevel[i] <- "Weekend"
    } else {
        daylevel[i] <- "Weekday"
    }
}
ds$daylevel <- daylevel
ds$daylevel <- factor(ds$daylevel)

StepsByDay <- aggregate(steps ~ interval + daylevel, data = ds, mean)
names(StepsByDay) <- c("interval", "daylevel", "steps")

xyplot(steps ~ interval | daylevel, StepsByDay, type = "l", layout = c(1, 2), xlab = "Interval", ylab = "Number of steps")
```

![](./PA1_template_files/figure-html/unnamed-chunk-13-1.png) 
