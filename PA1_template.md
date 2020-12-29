---
title: "Project 1"
author: "Macarena Fernández"
date: "December 28, 2020"
output:
  html_document: 
    keep_md: yes
  md_document:
    variant: markdown_github
  pdf_document: default
---

#Project 1
by Macarena Fernández


Load the data:

```r
library(knitr)
```

```
## Warning: package 'knitr' was built under R version 3.6.3
```

```r
library(readr)
```

```
## Warning: package 'readr' was built under R version 3.6.3
```

```r
activity <- read_csv("C:/Users/pc/Desktop/Python/activity.csv")
```

```
## 
## -- Column specification ------------------------------------------------------------------------
## cols(
##   steps = col_double(),
##   date = col_date(format = ""),
##   interval = col_double()
## )
```


Histograms of steps per day:


```r
s_by_d=tapply(activity$steps,as.factor(activity$date),sum,na.rm=TRUE)
hist(s_by_d,main="Histograms of steps by day",xlab="Steps by day",col="green")
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
summary(s_by_d)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##       0    6778   10395    9354   12811   21194
```

Time series plot of the average of steps taken by day:

```r
m_by_i=tapply(activity$steps,as.factor(activity$interval),mean,na.rm=TRUE)
intervals=unique(activity$interval)
plot(intervals,m_by_i,type="l",main="Time series average steps",xlab="Intervals",ylab="Mean Steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
The 5-minute interval that, on average, contains the maximum number of steps:

```r
i=which.max(m_by_i)
names(m_by_i)[i]
```

```
## [1] "835"
```
Number of missings Values:

```r
sum(is.na(activity$steps))
```

```
## [1] 2304
```
Straregy for imputing missing values: mean of the interval

```r
for (i in 1:nrow(activity)){
  if (is.na(activity[i,1])){
    interval=as.numeric(activity[i,3])
    ind=which(intervals==interval)
    activity[i,1]=m_by_i[[ind]]
    
  }
}
```

Histogram:

```r
s_by_d=tapply(activity$steps,as.factor(activity$date),sum)
hist(s_by_d,main="Histograms of steps by day",xlab="Steps by day",col="blue")
```

![](PA1_template_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

```r
summary(s_by_d)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##      41    9819   10766   10766   12811   21194
```
Factor Variable 0 is the date is a weekday 1 if is a weekend:

```r
activity$weekdays=0
weekend=c("sábado", "domingo")
activity$weekdays[weekdays(activity$date) %in% weekend]=1
activity$weekdays=factor(activity$weekdays,labels=c("weekday","weekend"))
```

Plots of average steps by interval, one for weekdays and weekend:

```r
weekend=subset(activity,weekdays=="weekend")
weekday=subset(activity,weekdays=="weekday")
m_by_i_we=tapply(weekend$steps,as.factor(weekend$interval),mean)
m_by_i_wd=tapply(weekday$steps,as.factor(weekday$interval),mean)
i_we=unique(weekend$interval)
i_wd=unique(weekday$interval)
r=range(m_by_i_wd,m_by_i_we)
```


Graph:

```r
par(mfrow=c(2,1),mar = c(4,2,4,2))
plot(i_we,m_by_i_we,type="l",col="blue",main="Weekend",xlab="interval",ylab="step",ylim=r)
plot(i_wd,m_by_i_wd,type="l",col="blue",main="Weekend",xlab="interval",ylab="step",ylim=r)
```

![](PA1_template_files/figure-html/unnamed-chunk-10-1.png)<!-- -->



