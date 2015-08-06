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



## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
