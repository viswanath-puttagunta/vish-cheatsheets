
## Data Wrangling in R
Get Current working directory
```
getwd()
```

Set Current working directory
```
setwd("C:/Users/vputtagunt/Documents/")
```


Read into dataframe from csv file
```
df <- as.data.frame(read.csv("sample.csv",header=TRUE,sep=",",fill=TRUE),stringsAsFactors=FALSE)
```

Number of rows and dimensions of dataframe
```
nrow(df)
dim(df)
```

Sum of specific column
```
sum(df$col1)
```

Find rows that have NA's 
```
df[is.na(df$col1) == TRUE, ]
```

Enter literal value into dataframe column
```
df$intcept <- 1
```

Remove if variables exist
```
rm(temp1,temp2)
```

##Group by sum/mean and building aggregated dataframe and join with parent dataframe
```
temp1 <- as.data.frame(aggregate(df$intcept,by=list(df$ID),na.rm=TRUE,sum),stringsAsFactors=FALSE)
temp2 <- as.data.frame(aggregate(df$paid,by=list(df$ID),na.rm=TRUE,mean),stringsAsFactors=FALSE)
temp2 <- temp2[,2]    #Remove the ID column
```
Concatenate Dataframes
```
aggregatedDf <- cbind(temp1,temp2)
```

Change column names
```
colnames(aggregatedDf) <- c("ID","NumPeriods", "paidSum")
names(aggregatedDf)[names(aggregatedDf) == "old.name"] <- 'new.name'
```

Left Join
```
df2 <- join(df,aggregatedDf,by="ID",type="left",match="all")
```

Generate quantileis in increments of 0.01. Useful for eliminating extreme values.
Note: Each quantile range has same number of rows.
But the value ranges differ for each quantile
```
q <- quantile(df$paid,probs = seq(0, 1, 0.01),na.rm=TRUE,type = 7)
```

Filter rows for which column is <= some value
Note that you are selecting all columns. That's why condition is on left of comma.
and no condition on right of comma
```
promo3 <- promo2[which(promo2$paidp <= 10.06), ]
```

Partition by cumulative sum
Ref: https://ugoproto.github.io/ugo_r_doc/Data_Analysis_in_R,_the_data.table_Way/
Ref: https://stackoverflow.com/questions/8559485/cumulative-sum-by-group-in-sqldf
```
library(data.table)
DT = data.table(place = 1:4, time = rep(1:3, each = 4), value = 1:3)
setkey(DT,place,time)   # order by place and time
DT
      place time value
 [1,]     1    1     1
 [2,]     1    2     2
 [3,]     1    3     3
 [4,]     2    1     2
 [5,]     2    2     3
 [6,]     2    3     1
 [7,]     3    1     3
 [8,]     3    2     1
 [9,]     3    3     2
[10,]     4    1     1
[11,]     4    2     2
[12,]     4    3     3

DT[,list(time,value,cumsum(value)),by=place]
      place time value V3
 [1,]     1    1     1  1
 [2,]     1    2     2  3
 [3,]     1    3     3  6
 [4,]     2    1     2  2
 [5,]     2    2     3  5
 [6,]     2    3     1  6
 [7,]     3    1     3  3
 [8,]     3    2     1  4
 [9,]     3    3     2  6
[10,]     4    1     1  1
[11,]     4    2     2  3
[12,]     4    3     3  6
```
