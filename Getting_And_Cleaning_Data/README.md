# Peer Assessments /Getting and Cleaning Data Course Project

The purpose of this project is to demonstrate your ability to collect, work with, and clean a data set.

http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 
Here are the data for the project: 
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

You should create one R script called run_analysis.R that does the following:

1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement. 
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names. 
5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

HOW the script works:

I downloaded the data for the project, unzipped the file and renamed the folder to UCI_HAR_Dataset (mind the underscores!).

First I merged training and the test sets. That is how I got three data sets

Using the function "grep" on features.txt data and with the help of functions "gsub" and "tolower" I extracted the names for my data set.

I used function "cbind" to merge data sets into one data set. This is how I got my tidy data.

After extracting unique subjects and activities I was able to calculate the mean (average) and write the data into independent_tidy_data.txt which I also uploaded onto assignment page.