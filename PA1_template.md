
# Peer Assessment One


Download, unzip and then read the given data.

```r
download.file("http://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip", 
        destfile = "fitnessdata.zip")
unzip("fitnessdata.zip")
fitdata <- read.csv("activity.csv", na.strings = "NA")
```
Load the dplyr library for use in data manipulation.  Group the data by date and summarise
in order to obtain the sum total steps for each date.  Convert the resulting totalsteps
variable to a numeric class for plotting purposes.
Load the ggplot2 package for plotting and run a quick plot on the data to produce a 
histogram of the sum total steps taken per day. 

```r
suppressPackageStartupMessages(library(dplyr))
fitdate <- group_by(fitdata, date)
fittotal <- summarise(fitdate, totalsteps = sum(steps, na.rm = TRUE))
fittotal <- subset(fittotal, totalsteps > 0)
fittotal$totalsteps <- as.numeric(fittotal$totalsteps)
library(ggplot2)
qplot(totalsteps, data = fittotal, binwidth = 2500, main = "Histogram of total number of steps taken each day", xlab = "Total Steps", ylab = "Count")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 

Use the mean function to obtain the average total daily steps.

```r
fitmean <- mean(fittotal$totalsteps)
print(fitmean)
```

```
## [1] 10766.19
```
Use the median function to obtain the median total daily steps.

```r
fitmedian <- median(fittotal$totalsteps)
print(fitmedian)
```

```
## [1] 10765
```
Load the lubridate package for time and date manipulation. Group the data by intervals
and summarise to obtain the average number of steps on a given interval across all dates.
Convert the intervals from their raw format to a POSIXct format to eliminate
inconsistencies in the data when plotting.  Use the base plotting system to ploy the 
average number of steps taken during each interval across all dates. 

```r
library(lubridate)
fitdaily <- group_by(fitdata, interval)
fitinterval1 <- summarise(fitdaily, meansteps = mean(steps, na.rm = TRUE))
fitinterval1$interval <- parse_date_time(fitinterval1$interval, order = "hm", quiet = TRUE)
with(fitinterval1, plot(interval, meansteps, type = "l", ylab = "Average number of steps taken", xlab = "Interval", main = "Average number of steps taken on given intervals"))
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png) 

Use the data frame previously sorted by intervals to summarise the data and obtain the 
average number of steps on each interval.  Further sort this data frame to obtain the 
interval with the highest number of average steps across all dates. 

```r
fitinterval <- summarise(fitdaily, meansteps = mean(steps, na.rm = TRUE))
x <- which.max(fitinterval$meansteps)
y <- fitinterval[x, 1]
print(y)
```

```
## Source: local data frame [1 x 1]
## 
##   interval
## 1      835
```
Sum the number of observations with missing data. 

```r
nadata <- sum(is.na(fitdata))
print(nadata)
```

```
## [1] 2304
```

