
## Electric Power Consumption
Data get from the http://archive.ics.uci.edu/ml/index.php a popular repository for machine learning datasets. 

Data set: [Electric power consumption](https://d396qusza40orc.cloudfront.net/exdata%2Fdata%2Fhousehold_power_consumption.zip) [20Mb]
</br>Description: Measurements of electric power consumption in one household with a one-minute sampling rate over a period of almost 4 years. Different electrical quantities and some sub-metering values are available.

```R
library(data.table)
data <- fread(input = "household_power_consumption.txt", na.strings="?"
                             )
```

```R
# Prevents histogram from printing in scientific notation
data <- data[, Global_active_power := lapply(.SD, as.numeric), .SDcols = c("Global_active_power")]
```

```R
# Change Date Column to Date Type
data <- data[, Date := lapply(.SD, as.Date, "%d/%m/%Y"), .SDcols = c("Date")]
```

```R
# Filter Dates for 2007-02-01 and 2007-02-02
data <- data[(Date >= "2007-02-01") & (Date <= "2007-02-02")]
```

```R
png("plot1.png", width=480, height=480)
## Plot 1
hist(data[, Global_active_power], main="Global Active Power", 
     xlab="Global Active Power (kilowatts)", ylab="Frequency", col="Red")

dev.off()
```
[](https://github.com/CamilaOspinaPatino/datasciencecoursera/blob/master/Project_1/plot1.png)

```R
# Prevents Scientific Notation
data <- data[, Global_active_power := lapply(.SD, as.numeric), .SDcols = c("Global_active_power")]
```

```R
# Making a POSIXct date capable of being filtered and graphed by time of day
data <- data[, dateTime := as.POSIXct(paste(Date, Time), format = "%d/%m/%Y %H:%M:%S")]
```


```R
png("plot2.png", width=480, height=480)

## Plot 2
plot(x = data[, dateTime] , y = data[, Global_active_power]
     , type="l", xlab=" ", ylab="Global Active Power (kilowatts)")

dev.off()
```

![](https://github.com/CamilaOspinaPatino/datasciencecoursera/blob/master/Project_1/plot2.png)

```R
png("plot3.png", width=480, height=480)
```

```R
# Plot 3
plot(x = data[, dateTime], y = data[, Sub_metering_1], type="l", xlab="Date/Time", ylab="Energy sub metering")
lines(data[, dateTime], data[, Sub_metering_2],col="red")
lines(data[, dateTime], data[, Sub_metering_3],col="blue")
legend("topright", col=c("black","red","blue"), c("Sub_metering_1  ","Sub_metering_2  ", "Sub_metering_3  ")
       ,lty=c(1,1), lwd=c(1,1))

dev.off()
```
![](https://github.com/CamilaOspinaPatino/datasciencecoursera/blob/master/Project_1/plot3.png)

```R
png("plot4.png", width=480, height=480)
```

```R
par(mfrow=c(2,2))

# Plot 1
plot(data[, dateTime], data[, Global_active_power], type="l", xlab="", ylab="Global Active Power")

# Plot 2
plot(data[, dateTime],data[, Voltage], type="l", xlab="datetime", ylab="Voltage")

# Plot 3
plot(data[, dateTime], data[, Sub_metering_1], type="l", xlab="", ylab="Energy sub metering")
lines(data[, dateTime], data[, Sub_metering_2], col="red")
lines(data[, dateTime], data[, Sub_metering_3],col="blue")
legend("topright", col=c("black","red","blue")
       , c("Sub_metering_1  ","Sub_metering_2  ", "Sub_metering_3  ")
       , lty=c(1,1)
       , bty="n"
       , cex=.5) 

# Plot 4
plot(data[, dateTime], data[,Global_reactive_power], type="l", xlab="datetime", ylab="Global_reactive_power")

dev.off()
```
![](https://github.com/CamilaOspinaPatino/datasciencecoursera/blob/master/Project_1/plot4.png)



















