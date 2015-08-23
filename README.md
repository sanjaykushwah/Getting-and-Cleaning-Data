# Introduction
This repository was created for the course project for the Coursera class Getting and Cleaning Data.
The purpose of this project is to demonstrate the ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis.
The repository contains the following:
  1. the script `run_analysis.R`
  2. the file `README.md`
  3. the file `CodeBook.md`
  4. the file tidy_dataset.txt
	
# Data
The data linked to from the course website represent data collected from the accelerometers from the Samsung Galaxy S smartphone. A full description is available at the site where the data was obtained: 

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

Here are the data for the project: 

https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

`a more detailed description of the data is found in the CodeBook.md`

# run_analysis.R:
### The script `run_analysis.R` will perform the following operations:
#### Creates the data folder
```
# if the data folder does not exist, create it
if(!file.exists("./data")){dir.create("./data")
}
```

#### Download the zip file into the data file and unzip it.
```
# if the zip file does not exist, download it and unzip it.
if(!file.exists("./data/Dataset.zip")){
  fileUrl <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
  download.file(fileUrl,destfile="./data/Dataset.zip")
  unzip(zipfile="./data/Dataset.zip",exdir="./data")
}
```

#### Load the dplyr library
```
library(dplyr)
```

#### Loads both subject files,  merges them into the subject table and names the column.
```
# read subject_train.txt and subject_test.txt files into data tables then bind
# them into a data table called subject with the column named 'subject'.
subject_train <- read.table('data/UCI HAR Dataset/train/subject_train.txt')
subject_test <- read.table('data/UCI HAR Dataset/test/subject_test.txt')
subject <- rbind(subject_train, subject_test)
colnames(subject) <- 'subject'
```
#### Loads both activity files, merges them into the activity table and names the column.
```
# read y_train.txt and y_test.txt files into data tables then bind them into
# a data table called activity with the column named 'activity'.
y_train <- read.table('data/UCI HAR Dataset/train/y_train.txt')
y_test <- read.table('data/UCI HAR Dataset/test/y_test.txt')
activity <- rbind(y_train, y_test)
colnames(activity) <- 'activity'
```

#### Loads the activity_labels file and replaces the numeric values of the activity table with verbal descriptions.
```
# read activity_labels.txt into a data table called activity_labels. 
activity_labels <- read.table('data/UCI HAR Dataset/activity_labels.txt')

# replace numeric values with descriptions of activities
activities <- factor(c(activity$"activity"))
activity_list <- plyr::mapvalues(activities, from = c(activity_labels$V1),
                           to = c(as.character(activity_labels$V2)))
```

#### Loads the training and testing files and merges them into the phone_data table.
```
# read X_train.txt and X_test.txt files into data tables then bind them into
# a data table called phone_data.
X_train <- read.table('data/UCI HAR Dataset/train/X_train.txt')
X_test <- read.table('data/UCI HAR Dataset/test/X_test.txt')
phone_data <- rbind(X_train, X_test)
```

#### Loads the features table and use the data to rename the columns of the phone_data table.
```
# Read the features.txt into a data table called features.
features <- read.table('data/UCI HAR Dataset/features.txt')

# Renamed the columns of phone_data in by using the the descriptions of
# the phone measurements contained in features.
colnames(phone_data) <- features$V2
```

#### Removes all columns that do not include 'std' or 'mean' and creates the reduced_data table.
```
# creates a data table that only contains column names that includes 'std' or 'mean',
# not case sensitive.
reduced_data <- phone_data[,grep("std|mean", ignore.case = TRUE, colnames(phone_data))]
```

#### Merges the subject, activity_list, and reduced_data tables into the combined_data table.
```
# binds the data tables subject, activity_list, and reduced_data
# into the data table called combined_data
combined_data <- cbind(subject, activity_list, reduced_data)
```

#### renames the column name 'activity_list' to 'activity'
```
# renames the column name 'activity_list' to 'activity'
combined_data <- rename(combined_data, activity = activity_list)
```

#### creates a data table called tidy_dataset
```
# creates a data table called tidy_data that is the result of
# grouping the combined_data table by subject and activity and finding the mean of each activity.
tidy_data <- combined_data %>%
  group_by(subject,activity) %>%
  summarise_each(funs(mean))
```

#### writes the table tidy_data to file tidy_dataset.txt into the current directory
```
# writes the table tidy_data to file tidy_dataset.txt 
write.table(tidy_data, file = 'tidy_dataset.txt', row.name = FALSE)
```