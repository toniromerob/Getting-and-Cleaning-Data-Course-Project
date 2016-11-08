
## Step-by-step Solution
### Introduction

After downloading the .zip file, and unzipping it into my folder "Course Project", we have the following:

- activity_labels.txt contains the type of activity for 1,2,...6
- features_info.txt contains information on how the features were obtained
- features.txt contains the list of each feature / variables that will be used as column names
- test and train folders
  - Each folder contains a X_...txt and y_...txt files (i.e. X_test.txt and X_train.txt)
    - Each Y file contains the activity_labels list (1,2,...6)
    - Each X file contains the data observed
  - Each folder contains a subject_...txt file (i.e. subject_test.txt and subject_train.txt)
    - Each of these files contains the subjects list:
    - 1,...30 for subject_train.txt
    - 1,...24 for subject_test.txt


### Part 1. Merges the training and the test sets to create one data set.

```
## Manually download the .zip file, unzip, and place it into my folder "Course Project"

## Set working directory
wd_location <- "/Users/toniromero/Google Drive/Coursera/1_Data Science - Johns Hopkins/3_Getting and Cleaning Data/Course Project/UCI HAR Dataset"
setwd(wd_location)

## Read .txt files into data frames
# Activity
data_activity_train <- read.table("train/y_train.txt")
data_activity_test <- read.table("test/y_test.txt")

# Data
data_observered_train <- read.table("train/x_train.txt")
data_observed_test <- read.table("test/x_test.txt")

# Subjects
data_subject_train <- read.table("train/subject_train.txt")
data_subject_test <- read.table("test/subject_test.txt")

## Merge tables
# Combine Test and Train Subjects
data_subject_all <- rbind(data_subject_test, data_subject_train)
names(data_subject_all) <- "subject"

# Combine Test and Train Activities
data_activity_all <- rbind(data_activity_test, data_activity_train)
# Change name of column from "V1" to "activity_code"
names(data_activity_all) <- "activity_code"

# Combine Test and Train Data
data_observed_all <- rbind(data_observed_test, data_observered_train)
# Change names of the 561 columns to the names in file features.txt
features_names <- read.table("features.txt")
names(data_observed_all) <- features_names$V2

# Combine all of them (Subject, Activity, and Data observed)
data_all <- cbind(data_subject_all, data_activity_all, data_observed_all)
```
### Part 2. Extracts only the measurements on the mean and standard deviation for each measurement.

```
## Extract mean and std for each measurement
# Read .txt into data frame
data_features <- read.table("features.txt")

# Change names of columns
names(data_features) <- c("code", "name")

# Get data of features of mean and std
data_mean <- grep("mean()", data_features$name, value = FALSE)
data_std <- grep("std()", data_features$name, value = FALSE)

# Combine mean and std indexes in a vector & sort them
data_mean_std <- c(data_mean, data_std)
data_mean_std <- sort(data_mean_std, decreasing = FALSE)

# Extract columns 1 (subject) and 2 (activity_code) and all columns containing mean or std
data_table <- data_all[,c(1, 2, data_mean_std+2]
```

### Part 3. Uses descriptive activity names to name the activities in the data set.

```
## Read activity info from activity_labels.txt
activity_labels <- read.table("activity_labels.txt")

## Uses descriptive activity names to name the activites in the data set
data_table$activity_code[data_table$activity_code == activity_labels$V1[1]] <- as.character(activity_labels$V2[1])
data_table$activity_code[data_table$activity_code == activity_labels$V1[2]] <- as.character(activity_labels$V2[2])
data_table$activity_code[data_table$activity_code == activity_labels$V1[3]] <- as.character(activity_labels$V2[3])
data_table$activity_code[data_table$activity_code == activity_labels$V1[4]] <- as.character(activity_labels$V2[4])
data_table$activity_code[data_table$activity_code == activity_labels$V1[5]] <- as.character(activity_labels$V2[5])
data_table$activity_code[data_table$activity_code == activity_labels$V1[6]] <- as.character(activity_labels$V2[6])

## Change the name of the column
colnames(data_table)[2] <- "activity"
```

### Part 4. Appropriately labels the data set with descriptive variable names.

```
## Take a look at the names of data_table corresponding to features.txt
names(data_table)

##There are a few things to denote:
        ## "t" = time
        ## "f" = frequency
        ## "Acc" = Accelerometer
        ## "Mag" = Magnitude
        ## "Gyro" = Gyroscopic
        ## "Freq" = Frequency
        ## "stimed" = estimated

data_table1 <- subset(data_table, select = 3:81)

names(data_table1) <- gsub("t", "time", names(data_table1))
names(data_table1) <- gsub("f", "frequency", names(data_table1))
names(data_table1) <- gsub("Acc", "Acceletomoter", names(data_table1))
names(data_table1) <- gsub("Mag", "Magnitude", names(data_table1))
names(data_table1) <- gsub("Gyro", "Gyroscopic", names(data_table1))
names(data_table1) <- gsub("Freq", "Frequency", names(data_table1))
names(data_table1) <- gsub("stimed", "estimated", names(data_table1))

data_table <- cbind(data_table[1:2], data_table1)
rm(data_table1)

# Create a file with the FIRST tidy data set
write.table(data_table, file = "tidy_data_table_1.txt", row.names = FALSE)
```

### Part 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

```
tidy_data_table <- aggregate(. ~ subject + activity, data_table, mean)

# Create a file with the SECOND tidy data set
write.table(tidy_data_table, file = "tidy_data_table_2.txt", row.names = FALSE)
```
