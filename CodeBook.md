# Getting-and-Cleaning-Data-R

## Downloading the files
- I loaded the library dplyr
- Creating an element with the url of the zip file
- Downloaded the zip file in the current directory
- unzip the file in the current directory

## Calling each element to the database
- Reading the textfile with the train dataset
- Reading the textfile of the subject numbers (people) 
- Reading activity lables: 1 WALKING 2 WALKING_UPSTAIRS 3 WALKING_DOWNSTAIRS 4 SITTING 5 STANDING 6 LAYING
- Joining all the previous elements in an element called: "traindata"

Then, I did the same for test dataset which was called "testdata"

## Joining All
- joining traindata + test data in "traintestdata"

## Columns names
- Creating an element "featuresnames" wich contained names of features
- Converting featuresnames in a vector
- Creating an element "nam" with the future names of traintestdata columns
- Assigned nam to the names of traintestdata columns

## Selecting columns with mean and standard desviation
- Creating a new database called traintestdata2 with only the mean and standard deviation variables

## Labels for activity
- Reading the file "activity_labels"
- Selecting only characters of "activity labels"
- Replacing the numbers in the column activity by the new activity labels  in the traintestdata2 database

## Appropriately labels the data set with descriptive variable names.
- Deleting undesirable of the column names of traintestdata2

## Creating a second, independent tidy data set with the average of each variable for each activity and each subject
- Using pipeline and summarize I created a new database called Averages with the average of each variable by activity and subject


