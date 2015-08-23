# The A full description is available at the site where the data was obtained: 
http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 
Contact: activityrecognition@smartlab.ws

# The data for the project is located at: 
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

# Description of data:
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. See 'features_info.txt' for more details. 

####For each record it is provided:
- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.

###The downloaded dataset includes the following files:
- 'README.txt'
- 'features.txt': List of all features.
- 'activity_labels.txt': Links the class labels with their activity name.
- 'train/X_train.txt': Training set.
- 'train/y_train.txt': Training labels.
- 'test/X_test.txt': Test set.
- 'test/y_test.txt': Test labels.

####The following files are available for the train and test data. Their descriptions are equivalent. 
- 'train/subject_train.txt': Each row identifies the subject who performed the activity for each window sample. Its range is from 1 to 30.
 
- 'train/Inertial Signals/total_acc_x_train.txt': The acceleration signal from the smartphone accelerometer X axis in standard gravity units 'g'. Every row shows a 128 element vector. The same description applies for the 'total_acc_x_train.txt' and 'total_acc_z_train.txt' files for the Y and Z axis.
 
- 'train/Inertial Signals/body_acc_x_train.txt': The body acceleration signal obtained by subtracting the gravity from the total acceleration. 

- 'train/Inertial Signals/body_gyro_x_train.txt': The angular velocity vector measured by the gyroscope for each window sample. The units are radians/second. 

####Notes: 
- Features are normalized and bounded within [-1,1].
- Each feature vector is a row on the text file.

# Description of Data Manipulation
### README.md provides a detailed explanation of how the script was implemented for each of the following steps.
- A file named `data` was created in the current directory
- The zip file was downloaded and unzipped

## Description of Data Location
### The downloaded dataset in `data` includes the following files:
- `Dataset.zip`
- `UCI HAR Dataset`

#### The files located in UCI HAR Dataset are:
- `README.txt`
- `features.txt`: List of all features.
- `activity_labels.txt`: Links the class labels with their activity name:
  - `WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING`
- `features_info.txt`: Detailed information about the features

##### The folder `test` which contains:
- `subject_test.txt`: Subject testing set. dim = 2947 * 1 units = 1:30 # of subjects
- `test/X_test.txt`: Test set. dim = 72947 * 561 units = numeric # of measurements
- `test/y_test.txt`: Test labels. dim = 2947 * 1 units = 1:6 # of activities

##### The folder `training` which contains: 
- `subject_train.txt`: Subject training set. dim = 7352 * 1 units = 1:30 # of subjects
- `train/X_train.txt`: Training set. dim = 7352 * 561 units = numeric # of measurements
- `train/y_train.txt`: Training labels. 7352 * 1 units = 1:6 # of activities

## After the download the script does the following:
#### Loads the dplyr library
#### Loads both subject files,  merges them into the subject table and names the column
#### Loads both activity files, merges them into the activity table and names the column.
#### Loads the activity_labels file and replaces the numeric values of the activity table with verbal descriptions.
#### Loads the training and testing files and merges them into the phone_data table.
#### Renamed the columns of phone_data in by using the the descriptions of
#### the phone measurements contained in features.
#### Removes all columns that do not include 'std' or 'mean' and creates the reduced_data table.
#### Merges the subject, activity_list, and reduced_data tables into the combined_data table.
#### renames the column name 'activity_list' to 'activity'
#### creates a data table called tidy_data `dim = 180 * 88`
#### tidy_data columns:                           
```
[1] "subject"                              "activity"                            
 [3] "tBodyAcc-mean()-X"                    "tBodyAcc-mean()-Y"                   
 [5] "tBodyAcc-mean()-Z"                    "tBodyAcc-std()-X"                    
 [7] "tBodyAcc-std()-Y"                     "tBodyAcc-std()-Z"                    
 [9] "tGravityAcc-mean()-X"                 "tGravityAcc-mean()-Y"                
[11] "tGravityAcc-mean()-Z"                 "tGravityAcc-std()-X"                 
[13] "tGravityAcc-std()-Y"                  "tGravityAcc-std()-Z"                 
[15] "tBodyAccJerk-mean()-X"                "tBodyAccJerk-mean()-Y"               
[17] "tBodyAccJerk-mean()-Z"                "tBodyAccJerk-std()-X"                
[19] "tBodyAccJerk-std()-Y"                 "tBodyAccJerk-std()-Z"                
[21] "tBodyGyro-mean()-X"                   "tBodyGyro-mean()-Y"                  
[23] "tBodyGyro-mean()-Z"                   "tBodyGyro-std()-X"                   
[25] "tBodyGyro-std()-Y"                    "tBodyGyro-std()-Z"                   
[27] "tBodyGyroJerk-mean()-X"               "tBodyGyroJerk-mean()-Y"              
[29] "tBodyGyroJerk-mean()-Z"               "tBodyGyroJerk-std()-X"               
[31] "tBodyGyroJerk-std()-Y"                "tBodyGyroJerk-std()-Z"               
[33] "tBodyAccMag-mean()"                   "tBodyAccMag-std()"                   
[35] "tGravityAccMag-mean()"                "tGravityAccMag-std()"                
[37] "tBodyAccJerkMag-mean()"               "tBodyAccJerkMag-std()"               
[39] "tBodyGyroMag-mean()"                  "tBodyGyroMag-std()"                  
[41] "tBodyGyroJerkMag-mean()"              "tBodyGyroJerkMag-std()"              
[43] "fBodyAcc-mean()-X"                    "fBodyAcc-mean()-Y"                   
[45] "fBodyAcc-mean()-Z"                    "fBodyAcc-std()-X"                    
[47] "fBodyAcc-std()-Y"                     "fBodyAcc-std()-Z"                    
[49] "fBodyAcc-meanFreq()-X"                "fBodyAcc-meanFreq()-Y"               
[51] "fBodyAcc-meanFreq()-Z"                "fBodyAccJerk-mean()-X"               
[53] "fBodyAccJerk-mean()-Y"                "fBodyAccJerk-mean()-Z"               
[55] "fBodyAccJerk-std()-X"                 "fBodyAccJerk-std()-Y"                
[57] "fBodyAccJerk-std()-Z"                 "fBodyAccJerk-meanFreq()-X"           
[59] "fBodyAccJerk-meanFreq()-Y"            "fBodyAccJerk-meanFreq()-Z"           
[61] "fBodyGyro-mean()-X"                   "fBodyGyro-mean()-Y"                  
[63] "fBodyGyro-mean()-Z"                   "fBodyGyro-std()-X"                   
[65] "fBodyGyro-std()-Y"                    "fBodyGyro-std()-Z"                   
[67] "fBodyGyro-meanFreq()-X"               "fBodyGyro-meanFreq()-Y"              
[69] "fBodyGyro-meanFreq()-Z"               "fBodyAccMag-mean()"                  
[71] "fBodyAccMag-std()"                    "fBodyAccMag-meanFreq()"              
[73] "fBodyBodyAccJerkMag-mean()"           "fBodyBodyAccJerkMag-std()"           
[75] "fBodyBodyAccJerkMag-meanFreq()"       "fBodyBodyGyroMag-mean()"             
[77] "fBodyBodyGyroMag-std()"               "fBodyBodyGyroMag-meanFreq()"         
[79] "fBodyBodyGyroJerkMag-mean()"          "fBodyBodyGyroJerkMag-std()"          
[81] "fBodyBodyGyroJerkMag-meanFreq()"      "angle(tBodyAccMean,gravity)"         
[83] "angle(tBodyAccJerkMean),gravityMean)" "angle(tBodyGyroMean,gravityMean)"    
[85] "angle(tBodyGyroJerkMean,gravityMean)" "angle(X,gravityMean)"                
[87] "angle(Y,gravityMean)"                 "angle(Z,gravityMean)"
``` 
#### writes the table tidy_data to file tidy_data.txt into the current directory