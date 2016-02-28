# CodeBook for run_analysis.R

## Overview

### Key Variables

* subject_test
* data_test
* activity_test
* subject_train
* data_train
* activity_train
* subject_merge
* data_merge
* activity_merge
* features
* activities
* filter_criteria
* tidy_data

### Code Breakdown
The script perform the following functions

#### 1. Read data from text files

>  # loading test data
>  print("Reading: subject_test.txt")
>  subject_test = read.table("getdata-projectfiles-UCI HAR Dataset/UCI HAR Dataset/test/subject_test.txt")
>  print("Reading: X_test.txt")
>  data_test = read.table("getdata-projectfiles-UCI HAR Dataset/UCI HAR Dataset/test/X_test.txt")
>  print("Reading: Y_test.txt")
>  activity_test = read.table("getdata-projectfiles-UCI HAR Dataset/UCI HAR Dataset/test/Y_test.txt")
  
>  # loading training data
>  print("Reading: subject_train.txt")
>  subject_train = read.table("getdata-projectfiles-UCI HAR Dataset/UCI HAR Dataset/train/subject_train.txt")
>  print("Reading: X_train.txt")
>  data_train = read.table("getdata-projectfiles-UCI HAR Dataset/UCI HAR Dataset/train/X_train.txt")
>  print("Reading: Y_train.txt")
>  activity_train = read.table("getdata-projectfiles-UCI HAR Dataset/UCI HAR Dataset/train/Y_train.txt")
  
>  # loading lookup information
>  print("Reading: features.txt")
>  features <- read.table("getdata-projectfiles-UCI HAR Dataset/UCI HAR Dataset/features.txt", col.names=c("featureId", "featureLabel"))
>  print("Reading: activity_labels.txt")
>  activities <- read.table("getdata-projectfiles-UCI HAR Dataset/UCI HAR Dataset/activity_labels.txt", col.names=c("activityId", "activityLabel"))
  
#### 2. Merge data from various text files

>  # merge test and train dataset
>  data_merge = rbind(data_test,data_train)
>  activity_merge = rbind(activity_test,activity_train)
>  subject_merge = rbind(subject_test,subject_train)
>  names(subject_merge) = "subjectId"

#### 3. Filter information based on the necessary criteria

>  # filter dataset based on criteria
>  names(data_merge) = features$featureLabel
>  filter_criteria <- grepl("-mean\\(\\)|-std\\(\\)",features$featureLabel)
>  data_merge <- data_merge[,filter_criteria]
>  names(activity_merge)="activityId"
>  activity <- merge(activity_merge,activities, by="activityId")$activityLabel
  
#### 4. Generate tidy data

>  # merge tidy data
>  tidy_data <- cbind(subject_merge,activity,data_merge)
>  write.table(tidy_data, "merged_tidy_data.txt")

#### 5. Calculate average of each variable
 
>  dataDT <- data.table(data)
>  calculatedData<- dataDT[, lapply(.SD, mean), by=c("subjectId", "activity")]
>  write.table(calculatedData, "calculated_tidy_data.txt")

