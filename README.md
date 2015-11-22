# Getting_and_Cleaning_Data_Project
'#read in features and activity labels
'features<-read.table("UCI HAR Dataset/features.txt",colClasses = "character")
'column_names<-features[,2]
'activity_labels<-read.table("UCI HAR Dataset/activity_labels.txt", colClasses = 
'                              "character", col.names = c("label","activity"))

'#read in test data and assign descriptive variable names as column names
'y_test<-read.table("UCI HAR Dataset/test/y_test.txt",colClasses = "character",
'          col.names = "activity")
'X_test<-read.table("UCI HAR Dataset/test/X_test.txt",col.names = column_names)
'subject_test<-read.table("UCI HAR Dataset/test/subject_test.txt",colClasses = 
'          "character",col.names = "subject")

'#merge X_test, y_test, and subject_test
'test_data<-cbind(subject_test,y_test,X_test)

'#read in train data and assign descriptive variable names as column names
'y_train<-read.table("UCI HAR Dataset/train/y_train.txt",colClasses = "character",
'                   col.names = "activity")
'X_train<-read.table("UCI HAR Dataset/train/X_train.txt",col.names = column_names)
'subject_train<-read.table("UCI HAR Dataset/train/subject_train.txt",colClasses = 
'                           "character",col.names = "subject")

'#merge X_train, y_train, and subject_train
'train_data<-cbind(subject_train,y_train,X_train)

'#merge train and test data
'all_data<-rbind(test_data,train_data)

'#extract mean and standard deviation for each measurement
'means<-all_data[,grep("mean",colnames(all_data))]
'stds<-all_data[,grep("std",colnames(all_data))]
'extracted<-cbind(all_data[,1:2],means,stds)

'#name activities in the data set with descriptive activity names
'newlabels<activity_labels[extracted[,2],2]
'extracted$activity<-newlabels

'#calculate averages of each variable for each subject and activity
'library(dplyr)
'extracted_group<-group_by(extracted,subject,activity)
'final_data<-summarize_each(extracted_group,funs(mean))

'#write the data to a text file called final_data.txt in the current working directory
'write.table(final_data,file="final_data.txt",row.names = FALSE)
