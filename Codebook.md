title | author | date | output
------------ | -------------
Getting and Cleaning Data Course Project | Antoni Romero Blanch | 11/08/2016 | tidy_data_table_1.txt; tidy_data_table_2.txt


## Project Description
This project is part of the *Getting and Cleaning Data* course from Johns Hopkins University on Coursera.org.

The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set. The goal is to prepare tidy data that can be used for later analysis.

## Study design and data processing
### Collection of the raw data
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data.

### Notes on the original (raw) data

For each record in the dataset it is provided:
- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope.
- A 561-feature vector with time and frequency domain variables.
- Its activity label.
- An identifier of the subject who carried out the experiment.

## Creating the tidy datafile
### Guide to create the tidy data files

The tidy data file was created by doing the following:

1. Merged training and test sets to create on data set:
  - Downloaded data: https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip
  - Unzipped file: creates ```UCI HAR Dataset``` folder with all untidy data and files
  - Read and merged train and test for activity, data, and subject:
    - Read and merged **activity** files: ```y_train.txt``` and ```y_test.txt``` into ```data_activity_all```
    - Read and merged **data** files: ```x_train.txt``` and ```x_test.txt``` into ```data_observed_all```
    - Read and merged **subject** files: ```subject_train.txt``` and ```subject_test.txt``` into ```data_subject_all```
  - Read data from ```features.txt``` into ```features_names```
  - Replaced the variable (column) names of ```data_observed_all``` using data 2nd column of ```features_names```
  - Merged ```data_subject_all```, ```data_activity_all```, and ```data_observed_all``` into ```data_all```
2. Extracted measurements on the mean and standard deviation for each measurement:
  - Extracted rows from ```features_names``` that contained word **mean** or **std**
  - Combined them into ```data_mean_std```
  - Extracted columns from ```data_all``` corresponding to **subject** (col1) and **activity** (col2) as well as columns containing **mean** or **std** (```data_mean_std```)
  - Stored new table into ```data_table```
3. Used descriptive activity names to name the activities in the data set:
  - Read ```activity_labels.txt``` to extract activity descriptions (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, and LAYING) into ```activity_labels```
  - Replaced each activity code (1,2,...,6) from ```data_table``` with the corresponding **activity label** from ```activity_labels```
  - Changed the variable name **activity_code** (2nd column) to **activity**
4. Labeled the data set with descriptive variable names:
  - Subset all columns corresponding to observed variables, excluding subject and activity, into ```data_table1```
  - Substituted all the observed abbreviations in the variables of ```data_table1``` for its corresponding full word:
    - "t" for "time"
    - "f" for "frequency"
    - "Acc" for "accelerometer"
    - etc.
  - Merged again the modified ```data_table1``` and the first 2 columns of ```data_table```
  - Removed ```data_table1``` as it was temporary
  - Saved these data into a new file ```tidy_data_table_1.txt```
5. Created a second tidy data set with the average of each variable for each **activity** and each **subject**:
  - Aggregated **subject + activity** and applied **mean**
  - Saved these data into a new file ```tidy_data_table_2.txt```

### Cleaning of the data

The data cleaning process has created 2 data sets:
- ```tidy_data_table_1.txt``` contains all data observations on all the variables corresponding to a **mean** or **standard deviation** with **subject** and **activity** details in the first 2 columns
- ```tidy_data_table_2.txt``` contains the previous data but grouped by subject (30 subjects) and activity (6 activities).
  - Subject 1 WALKING
  - Subject 1 WALKING_UPSTAIRS
  - ...
  - Subject 2 WALKING
  - ...
  - Subject 30 LAYING

## Description of the variables in the tidy_data_table_1.txt file

### Output files

File ```tidy_data_table_1.txt```:

- Dimensions 10299 x 81
- Variables: **subject**, **activity**, and all variables containing a **mean** or **standard deviation**

File ```tidy_data_table_2.txt```:

- Dimensions 180 x 81
- 180 is the result of 30 subjects and 6 activities per subject (grouped from all observations)
- Variables: **subject**, **activity**, and all variables containing a **mean** or **standard deviation**


### Variables

activity_labels:
  - 6 x 2 data.frame containing **index** and **activity description**
  - No unit of measurement, just descriptive word

data_activity_all:
  - 10299 x 1 data.frame containing all activity codes for each one of the 10299 observations
  - Integer 1 to 6 for each type of activity

data_activity_test:
  - 2947 x 1 data.frame containing all activity codes for each one of the 2947 observations of **test subjects**
  - Integer 1 to 6 for each type of activity

data_activity_train:
  - 7352 x 1 data.frame containing all activity codes for each one of the 7352 observations of **train subjects**
  - Integer 1 to 6 for each type of activity

data_all:
  - 10299 x 563 data.frame containing all data observed (all 561 variables) for **all subjects** and all activities, **including** corresponding subject and activity information

data_observed_all
  - 10299 x 561 data.frame containing all data observed (all 561 variables) for **all subjects** and all activities, **excluding** corresponding subject and activity information

data_observed_test
  - 2947 x 561 data.frame containing all data observed (all 561 variables) for all **test subjects** and all activities, **excluding** corresponding subject and activity information

data_observed_train
  - 7352 x 561 data.frame containing all data observed (all 561 variables) for all **train subjects** and all activities, **excluding** corresponding subject and activity information

data_subject_all
  - 10299 x 1 data.frame containing subject numbers for **all subjects** of the observations
  - Integers 1 to 30

data_subject_test
  - 2947 x 1 data.frame containing subject numbers for all **test subjects** observations
  - Integers 1 to 30

data_subject_train
  - 7352 x 1 data.frame containing subject numbers for all **train subjects** observations
  - Integers 1 to 30

data_table
  - 10299 x 81 data.frame containing all observations of and variables **subject**, **activity**, and all 79 variables that contain **mean** or **standard deviation**
  - **activity** variable contains the description of each activity instead of the code

features_names
  - 561 x 2 data.frame containing the **code** and **name** of each 561 different features such as *tBodyAcc-mean()-X*, *tBodyAcc-mean()-Y*, *tBodyAcc-mean()-Z*, *tBodyAcc-std()-X*, etc.

tidy_data_table
  - 180 x 81 data.frame containing the data of **subject**, **activity**, all variables containing **mean** and **standard deviation** grouped by **subject** and **activity** and with the mean applied to each group

data_mean
  - Vector with index of variables in features_names that contain the characters **mean()**
  - 46 int values

data_std
  - Vector with index of variables in features_names that contain the characters **std()**
  - 33 int values

data_mean_std
  - Vector with index of variables in features_names that contain the characters **mean()** or **std()**
  - 79 int values
