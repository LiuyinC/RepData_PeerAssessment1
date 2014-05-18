# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
ActivityData <- read.csv("./activity.csv", head = T)
head(ActivityData)
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
Since the date data is stored as factor, we can calculate the sum of steps per day by the function rowsum.

```r
sumsteps <- rowsum(ActivityData$steps, ActivityData$date, na.rm = T)
hist(sumsteps, xlab = "Total number of steps per day")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 

And the mean is

```r
meansteps <- mean(sumsteps)
meansteps
```

```
## [1] 9354
```

and median is 

```r
mediansteps <- median(sumsteps)
mediansteps
```

```
## [1] 10395
```

## What is the average daily activity pattern?
Now considering the intervals as factors, we can get the average steps during 5-minute interval through these 2 months.

```r
intervals <- unique(ActivityData$interval)
intervalsteps <- tapply(ActivityData$steps, ActivityData$interval, mean, na.rm = T)
plot(intervals, intervalsteps, type = "l", ylab = "Number of steps")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 


The maximum of steps is contained in the interval:

```r
intervals[which.max(intervalsteps)]
```

```
## [1] 835
```


## Imputing missing values




## Are there differences in activity patterns between weekdays and weekends?
