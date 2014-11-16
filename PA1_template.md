# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
data0 <- read.csv("activity.csv")
data <- na.omit(data0)
```


## What is mean total number of steps taken per day?

Sum the number of steps taken in each day using the aggregate function:

```r
dailysteps <- aggregate(steps ~ date, data = data, sum)
```

Create a histogram of the daily activity:


```r
hist(dailysteps$steps, main="Histogram of Steps per Day",xla="")
```

![](./PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

Calculate the mean and median steps per day (as well as other values):

```r
summary(dailysteps$steps)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##      41    8841   10760   10770   13290   21190
```

## What is the average daily activity pattern?

Calculate the mean number of steps taken in each 5-minute interval on the average day:

```r
dailypattern <- aggregate(steps ~ interval, data = data, mean)
```

Plot a time series graph of the daily activity:


```r
plot(dailypattern$interval,dailypattern$steps,type="l",xla="",yla="Mean number of steps in interval")
```

![](./PA1_template_files/figure-html/unnamed-chunk-6-1.png) 

The time of day when the number of steps was maximum (on average):


```r
dailypattern$interval[dailypattern$steps==max(dailypattern$steps)]
```

```
## [1] 835
```



## Imputing missing values

The original dataset has a number of missing values.  The number of rows affected is:


```r
sum(!complete.cases(data0))
```

```
## [1] 2304
```

We will fill these missing values with the mean for the corresponding interval:


```r
dataNAintervals <- data0$interval[is.na(data0$steps)]
data0$steps[is.na(data0$steps)] <- dailypattern$steps[dailypattern$interval == dataNAintervals]
```

Now we can redo the analysis:


```r
data <- data0
dailysteps <- aggregate(steps ~ date, data = data, sum)
hist(dailysteps$steps, main="Histogram of Steps per Day",xla="")
```

![](./PA1_template_files/figure-html/unnamed-chunk-10-1.png) 

```r
summary(dailysteps$steps)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##      41    8860   10770   10770   13190   21190
```


## Are there differences in activity patterns between weekdays and weekends?

I can't get the weekdays function to work...investigating now...
