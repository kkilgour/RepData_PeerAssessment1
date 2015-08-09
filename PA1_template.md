# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
dataset <- read.csv('activity.csv')
head(dataset[is.na(dataset$steps)==FALSE & dataset$steps > 0,], n=10)
```

```
##     steps       date interval
## 555   117 2012-10-02     2210
## 556     9 2012-10-02     2215
## 627     4 2012-10-03      410
## 631    36 2012-10-03      430
## 644    25 2012-10-03      535
## 647    90 2012-10-03      550
## 648   411 2012-10-03      555
## 649   413 2012-10-03      600
## 650   415 2012-10-03      605
## 651   519 2012-10-03      610
```

```r
typeof(dataset)
```

```
## [1] "list"
```

```r
typeof(dataset$steps)
```

```
## [1] "integer"
```

```r
typeof(dataset$date)
```

```
## [1] "integer"
```

```r
typeof(dataset$interval)
```

```
## [1] "integer"
```

## What is mean total number of steps taken per day?
Calculate the total number of steps taken per day.

```r
# Include the plyr library to be able to use the ddply function
library(plyr)
# Summarize the dataset by date
daily <- ddply(dataset, "date", summarize, total.steps = sum(steps))
head(daily, n=10)
```

```
##          date total.steps
## 1  2012-10-01          NA
## 2  2012-10-02         126
## 3  2012-10-03       11352
## 4  2012-10-04       12116
## 5  2012-10-05       13294
## 6  2012-10-06       15420
## 7  2012-10-07       11015
## 8  2012-10-08          NA
## 9  2012-10-09       12811
## 10 2012-10-10        9900
```

Make a histogram of the total number of steps taken each day.

```r
hist(daily$total.steps, breaks=20)
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

Calculate and report the mean and median of the total number of steps taken per day.

```r
daily.mean <- mean(daily$total.steps, na.rm=TRUE)
daily.median <- median(daily$total.steps, na.rm=TRUE)
```
The mean daily steps taken is 10766.19 and the median daily steps taken is 10765.

## What is the average daily activity pattern?
Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r
# Import the ggplot library
library(ggplot2)
# Calculate a typical day by averaging the number of steps taken at each interval
typical.day <- ddply(dataset, "interval", summarize, avg.steps = mean(steps, na.rm=TRUE))
ggplot(data=typical.day, mapping=aes(x=interval, y=avg.steps)) + geom_line()
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png) 

Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
# Order the data frame by average steps descending to find the interval with the most steps
ordered <- arrange(typical.day, desc(avg.steps))
#strftime(ordered[1,1], format="%H%M")
```
The interval with the maximum number of steps is ``835``.

## Imputing missing values
Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

```r
na.dataset <- subset(dataset, subset = is.na(steps))
head(na.dataset)
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

```r
nrow(na.dataset)
```

```
## [1] 2304
```

```r
nrow(dataset[is.na(dataset$steps),])
```

```
## [1] 2304
```

Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

Create a new dataset that is equal to the original dataset but with the missing data filled in.

Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?


## Are there differences in activity patterns between weekdays and weekends?
