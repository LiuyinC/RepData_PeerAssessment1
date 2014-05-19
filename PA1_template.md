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
There are lots of missing data in the dataset. The number of missing value in our dataset is the row difference between the original data and the data without missing values.

```r
nrow(ActivityData) - nrow(ActivityData[complete.cases(ActivityData), ])
```

```
## [1] 2304
```

In order to estimate the effect of missing values on daily activity, we can introduce a strategy that replaces NA with the average steps for that 5 minutes interval and saved in a new dataset named NewActData.

```r
NewActData <- ActivityData
ActivityData <- cbind(ActivityData, estimated = NA)
for (i in 1:nrow(ActivityData)) {
    if (is.na(ActivityData[i, 1])) {
        NewActData[i, 1] = intervalsteps[which(intervals == ActivityData[i, 
            3])]
    } else {
        NewActData[i, 1] = ActivityData[i, 1]
    }
}
head(NewActData)
```

```
##     steps       date interval
## 1 1.71698 2012-10-01        0
## 2 0.33962 2012-10-01        5
## 3 0.13208 2012-10-01       10
## 4 0.15094 2012-10-01       15
## 5 0.07547 2012-10-01       20
## 6 2.09434 2012-10-01       25
```

Now make a new histogram of the total number of steps in new dataset.

```r
Newsumsteps <- rowsum(NewActData$steps, NewActData$date, na.rm = T)
hist(Newsumsteps, xlab = "Total number of steps per day")
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 

And the mean is

```r
Newmeansteps <- mean(Newsumsteps)
Newmeansteps
```

```
## [1] 10766
```

and median is 

```r
Newmediansteps <- median(Newsumsteps)
Newmediansteps
```

```
## [1] 10766
```

However, by this strategy for replacing missing value increases the average or median of the total daily number of steps. 
## Are there differences in activity patterns between weekdays and weekends?
