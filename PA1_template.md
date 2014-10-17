# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
Show any code that is needed to
Load the data (i.e. read.csv())
Process/transform the data (if necessary) into a format suitable for your analysis


```r
d <- read.csv(unz('activity.zip','activity.csv'));
d$date=as.Date(d$date);
```


## What is mean total number of steps taken per day?
For this part of the assignment, you can ignore the missing values in the dataset.
Make a histogram of the total number of steps taken each day
Calculate and report the mean and median total number of steps taken per day


```r
ds<-aggregate(steps~date,d,sum);
hist(ds$steps);
```

![plot of chunk unnamed-chunk-2](./PA1_template_files/figure-html/unnamed-chunk-2.png) 


```r
mean(ds$steps);
```

```
## [1] 10766
```



```r
median(ds$steps);
```

```
## [1] 10765
```

## What is the average daily activity pattern?
Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r
dt<-aggregate(steps~interval,d,sum);
plot(dt,type="l");
```

![plot of chunk unnamed-chunk-5](./PA1_template_files/figure-html/unnamed-chunk-5.png) 
Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
dt[which.max(dt$steps),'interval'];
```

```
## [1] 835
```



## Imputing missing values
Note that there are a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

```r
sum(is.na(d$steps));
```

```
## [1] 2304
```


Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
da<-aggregate(steps~interval,d,mean);
names(da)[2] <- 'avgsteps'


d2 <- d;
d2<-merge(d2,da)
inull=which(is.na(d$steps));
d2$steps[inull]=d2$avgsteps[inull];
d2<-d2[,c('interval','steps','date')]
```


Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

```r
ds2<-aggregate(steps~date,d2,sum);
hist(ds2$steps);
```

![plot of chunk unnamed-chunk-9](./PA1_template_files/figure-html/unnamed-chunk-9.png) 


```r
mean(ds2$steps);
```

```
## [1] 9564
```



```r
median(ds2$steps);
```

```
## [1] 11216
```




## Are there differences in activity patterns between weekdays and weekends?
