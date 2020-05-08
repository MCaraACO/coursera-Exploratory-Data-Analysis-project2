# coursera-Exploratory-Data-Analysis-project2
### Project 2, subset and plots 

Fine particulate matter (PM2.5) is an ambient air pollutant for which there is strong evidence that it is harmful to human health. In the United States, the Environmental Protection Agency (EPA) is tasked with setting national ambient air quality standards for fine PM and for tracking the emissions of this pollutant into the atmosphere. Approximatly every 3 years, the EPA releases its database on emissions of PM2.5. This database is known as the National Emissions Inventory (NEI). You can read more information about the NEI at the EPA National Emissions Inventory web site.

For each year and for each type of PM source, the NEI records how many tons of PM2.5 were emitted from that source over the course of the entire year. The data that you will use for this assignment are for 1999, 2002, 2005, and 2008.

The data for this assignment are available from the course web site as a single zip file:

https://d396qusza40orc.cloudfront.net/exdata%2Fdata%2FNEI_data.zip

The zip file contains two files:

PM2.5 Emissions Data (\color{red}{\verb|summarySCC_PM25.rds|}summarySCC_PM25.rds): This file contains a data frame with all of the PM2.5 emissions data for 1999, 2002, 2005, and 2008. For each year, the table contains number of tons of PM2.5 emitted from a specific type of source for the entire year. Here are the first few rows.

as long as each of those files is in your current working directory (check by calling \color{red}{\verb|dir()|}dir() and see if those files are in the listing).

# Assignment : 
The overall goal of this assignment is to explore the National Emissions Inventory database and see what it say about fine particulate matter pollution in the United states over the 10-year period 1999–2008. You may use any R package you want to support your analysis.

# Script : 
```
setwd("./Desktop/datasciencecoursera/Coursera Exploratory/")
```

## Plot 1
```
zip = "exdata_data_NEI_data.zip"
if(file.exists(zip)) {
unzip(zip)
}

NEI <- readRDS("summarySCC_PM25.rds")
SCC <- readRDS("Source_Classification_Code.rds")
unique(NEI$year)
#[1] 1999 2002 2005 2008

png(filename = "plot1.png", width = 640, height = 480, units = "px")

with(NEI, plot(unique(year), tapply(Emissions, year, sum),
type = "l", lwd = 3, col = rgb(0,0.255,0,0.5),
xlab = "Year", ylab = expression("Total PM"[2.5]*" Emissions"),
main = "Total emissions from PM2.5 in the United States from 1999 to 2008?"))

dev.off()
```


## Plot 2
```
zip = "exdata_data_NEI_data.zip"
if(file.exists(zip)) {
unzip(zip)
}

NEI <- readRDS("summarySCC_PM25.rds")
SCC <- readRDS("Source_Classification_Code.rds")
unique(NEI$year)
#[1] 1999 2002 2005 2008
png(filename = "plot2.png", width = 600, height = 600, units = "px")
```

#### Subset baltimore city
```
baltimore <- subset(NEI, fips == "24510")
#Plot
with(baltimore, plot(unique(year), tapply(Emissions, year, sum),
type = "l", lwd = 3, col = rgb(0.255,0,0,0.5),
xlab = "Year", ylab = expression("Total PM"[2.5]*" Emissions"),
main = "Total emissions from PM2.5 in Baltimore city from 1999 to 2008?"))

dev.off()
```


## Plot 3
```
zip = "exdata_data_NEI_data.zip"
if(file.exists(zip)) {
unzip(zip)
}

NEI <- readRDS("summarySCC_PM25.rds")
SCC <- readRDS("Source_Classification_Code.rds")
unique(NEI$year)
#[1] 1999 2002 2005 2008
```

#### Subset baltimore
```
baltimore <- subset(NEI, fips == "24510")
```

#### Aggregation of Emissions by type and year
```
library(dplyr)
baltimore%>%
group_by(year,type)%>%
summarise(total = sum(Emissions)) -> baltimoreAgg

png(filename = "plot3.png", width = 600, height = 600, units = "px")

library(ggplot2)
g <- ggplot(baltimoreAgg, aes(year, total, color = type))
g <- g + geom_line(lwd = 1) +
xlab("Years") +
ylab(expression("Total PM"[2.5]*" Emissions")) +
ggtitle(expression("Total PM"[2.5]*" Emissions Baltimore City, Maryland from 1999 to 2008"))
print(g)

dev.off()
```


## Plot 4
```
zip = "exdata_data_NEI_data.zip"
if(file.exists(zip)) {
unzip(zip)
}

NEI <- readRDS("summarySCC_PM25.rds")
SCC <- readRDS("Source_Classification_Code.rds")
unique(NEI$year)
#[1] 1999 2002 2005 2008
```

#### Merging NEI and
```
colnames(SCC)
colnames(NEI)
newdata = merge(SCC, NEI, by = "SCC")
```

#### Subset and aggregate coal combustion-related
```
coal <- newdata[grep("coal", newdata$Short.Name), ]
head(coal)

coal%>%
group_by(year)%>%
summarise(total = sum(Emissions)) -> coalAgg

png(filename = "plot4.png", width = 600, height = 600, units = "px")

library(ggplot2)
g <- ggplot(coalAgg, aes(year, total))
g <- g + geom_line(lwd = 1) +
xlab("Years") +
ylab(expression("Total PM"[2.5]*" Emissions")) +
ggtitle(expression("Total PM"[2.5]*" Emissions in US from coal"))
print(g)

dev.off()
```

## PLOT 5 
```

zip = "exdata_data_NEI_data.zip"
if(file.exists(zip)) {
unzip(zip)
}

NEI <- readRDS("summarySCC_PM25.rds")
SCC <- readRDS("Source_Classification_Code.rds")
unique(NEI$year)
#[1] 1999 2002 2005 2008
```

#### Merging NEI and
```
colnames(SCC)
colnames(NEI)
newdata = merge(SCC, NEI, by = "SCC")
```

#### Subset and aggregate coal combustion-related
```
vehicI <- grepl("vehicle", newdata$SCC.Level.Two, ignore.case=TRUE)
vehic <- newdata[vehicI,]

vehicSub = subset(vehic, fips == "24510")
head(vehicSub)

vehicSub%>%
group_by(year)%>%
summarise(total = sum(Emissions)) -> vehicAgg

png(filename = "plot5.png", width = 600, height = 600, units = "px")

library(ggplot2)
g <- ggplot(vehicAgg, aes(year, total))
g <- g + geom_line(lwd = 1) +
xlab("Years") +
ylab(expression("Total PM"[2.5]*" Emissions")) +
ggtitle(expression("Total PM"[2.5]*" Emissions in Baltimore City from vehicles"))
print(g)

dev.off()
```


## PLOT 6
```
zip = "exdata_data_NEI_data.zip"
if(file.exists(zip)) {
  unzip(zip)
}

NEI <- readRDS("summarySCC_PM25.rds")
SCC <- readRDS("Source_Classification_Code.rds")
unique(NEI$year)
#[1] 1999 2002 2005 2008
```

#### Merging NEI and 
```
colnames(SCC)
colnames(NEI)
newdata = merge(SCC, NEI, by = "SCC")
```

#### Subset and aggregate coal combustion-related
```
vehicI <- grepl("vehicle", newdata$SCC.Level.Two, ignore.case=TRUE)
vehic <- newdata[vehicI,]

vehicBalt = subset(vehic, fips == "24510")
vehicLA = subset(vehic,fips=="06037")

vehicBalt%>%
  group_by(year)%>%
  summarise(total = sum(Emissions)) -> vehicBaltAgg
vehicBaltAgg$loc = "Balt"
vehicLA%>%
  group_by(year)%>%
  summarise(total = sum(Emissions)) -> vehicLAAgg
vehicLAAgg$loc = "L.A"

test = rbind(vehicLAAgg,vehicBaltAgg)

png(filename = "plot6.png", width = 600, height = 600, units = "px")

library(ggplot2)
g <- ggplot(vehicLAAgg, aes(year, total, color = loc))
g <- g + geom_line(lwd = 1) +
  xlab("Years") +
  ylab(expression("Total PM"[2.5]*" Emissions")) +
  ggtitle(expression("Total PM"[2.5]*" Emissions in L.A from vehicles"))
g2 <- ggplot(vehicBaltAgg, aes(year, total, color = "blue"))
g2 <- g2 + geom_line(lwd = 1) +
  xlab("Years") +
  ylab(expression("Total PM"[2.5]*" Emissions")) +
  ggtitle(expression("Total PM"[2.5]*" Emissions in Baltimore City  from vehicles"))
require(gridExtra)
gridExtra::grid.arrange(g,g2)

dev.off()
```
