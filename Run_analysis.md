---
title: "Run_analysis"
author: "Oscar Alvarado"
date: "22/7/2020"
output: html_document
---
## Downloading the files
```{r}
library(dplyr)
urlzip<- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(urlzip, "./zipquiz.zip")
unzip("./zipquiz.zip")
```

## Calling each element to the database
```{r}
traindata<- read.delim("./UCI HAR Dataset/train/x_train.txt", sep = "", header = F )
subjectname<-as.vector(read.csv("./UCI HAR Dataset/train/subject_train.txt", sep = "", header = F))
activity<-as.vector(read.csv("./UCI HAR Dataset/train/y_train.txt", sep = "", header = F))
traindata<-cbind(subjectname, activity, traindata)
head(traindata)

testdata<- read.delim("./UCI HAR Dataset/test/x_test.txt", sep = "", header = F )
subjectnamet<-as.vector(read.csv("./UCI HAR Dataset/test/subject_test.txt", sep = "", header = F))
activityt<-as.vector(read.csv("./UCI HAR Dataset/test/y_test.txt", sep = "", header = F))
testdata<-cbind(subjectnamet, activityt,testdata)
head(testdata)
```

## Joinning all
```{r}
traintestdata<-rbind(traindata, testdata)
head(traintestdata)
str(traintestdata)
```

## Columns Names
```{r}
featuresnames<-read.csv("./UCI HAR Dataset/features.txt", sep = "", header = F)
featurenames2<-as.vector(featuresnames[[2]])
nam<-c("subject", "activity", featurenames2)
names(traintestdata)<-nam
head(traintestdata)
names(traintestdata)
```

## Selecting columns with mean and standard desviation
```{r}

traintestdata2<-traintestdata%>%select("subject", "activity",grep("*mean", names(traintestdata)), grep("*std", names(traintestdata)))
names(traintestdata2)
head(traintestdata2)
```

## Labels for activity
```{r}

activitylabels<-scan("./UCI HAR Dataset/activity_labels.txt", what = "character")
activitylabels
activitylabels<-activitylabels[c(2,4,6,8,10,12)]
class(activitylabels)
activitylabels

for ( i in 1:6) {
    traintestdata2$activity<-replace(traintestdata2$activity,traintestdata2$activity==i,activitylabels[i])
}

traintestdata2$activity
```

## Appropriately labels the data set with descriptive variable names.
```{r}
newnames<-gsub("[()]","", names(traintestdata2))
newnames<-gsub("-","", newnames)
newnames<-gsub("(Body){2,2}", "", newnames)
newnames<-gsub("Acc", ".Accelerometer.", newnames)
newnames<-gsub("Gyro", ".Gyroscope.", newnames)
newnames<-gsub("mean", "Mean.", newnames)
newnames<-gsub("^f", "Frequency.", newnames)
newnames<-gsub("^t", "Time.", newnames)
newnames
newnames<-gsub("[.]{2,2}", ".", newnames)
newnames
names(traintestdata2)<-newnames
head(traintestdata2, 3)
```

## Creating a second, independent tidy data set with the average of each variable for each activity and each subject.
```{r}
Averages<-traintestdata2%>%group_by(activity, subject)%>%summarise_if(is.numeric, mean)
Averages
class(Averages)
write.csv(Averages, file = "./averages.csv")

```


