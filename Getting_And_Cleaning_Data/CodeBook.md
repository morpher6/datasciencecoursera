-- Let's check if working directory is present otherwise exit the script and mind the underscores in the directory's name. Read all files and dirs in specified directory
dir = "./UCI_HAR_Dataset/"
if (dir.exists(dir)){
  file_list <- list.files(path = dir,pattern = "*")
} else {
  stop(paste("Directory \"",dir,"\" doesn't exist!"))
}

-- 1. Merges the training and the test sets to create one data set.

x_train <- read.table("./UCI_HAR_Dataset/train/X_train.txt")
x_test <- read.table("./UCI_HAR_Dataset/test/X_test.txt")
-- merge X data set
x <- rbind(x_train, x_test)

y_train <- read.table("./UCI_HAR_Dataset/train/y_train.txt")
y_test <- read.table("./UCI_HAR_Dataset/test/y_test.txt")
-- merge y data set
y <- rbind(y_train, y_test)

subj_train <- read.table("./UCI_HAR_Dataset/train/subject_train.txt")
subj_test <- read.table("./UCI_HAR_Dataset/test/subject_test.txt")
-- merge subjects
subj <- rbind(subj_train, subj_test) 

-- 2. Extracts only the measurements on the mean and standard deviation for each measurement. 

features <- read.table("./UCI_HAR_Dataset/features.txt")

-- find values with "-mean" and "-std" in name and create a vector
features_selected <- grep("-mean\\(\\)|-std\\(\\)", features[, 2])
x <- x[, features_selected]
names(x) <- features[features_selected, 2]

-- substitute all "needle" in "haystack" 
names(x) <- gsub("\\(|\\)", "", names(x))

-- change all names (like "fBodyGyro-std()-X") to lower case 
names(x) <- tolower(names(x))

-- 3. Uses descriptive activity names to name the activities in the data set.

activities <- read.table("./UCI_HAR_Dataset/activity_labels.txt")
activities[, 2] = gsub("_", "", tolower(as.character(activities[, 2])))
y[,1] = activities[y[,1], 2]
names(y) <- "activity"

-- 4. Appropriately labels the data set with descriptive variable names.

names(subj) <- "subject"

-- column bind of all three to create one data set
-- subject, activity, data
tidy_data <- cbind(subj, y, x)

-- print(head(tidy_data))

-- 5. From the data set in step 4, creates a second, independent tidy data set 
-- with the average of each variable for each activity and each subject.

-- number of tidy_data columns
tidy_data_cols = 68

-- set all the required variables to gather data for independent_tidy_data

subj_unique = unique(subj)[,1]
subj_cnt = length(subj_unique)
activities_unique = unique(activities)[,1]
activities_cnt = length(activities_unique)

independent_tidy_data = tidy_data[1:(subj_cnt*activities_cnt), ]

row = 1
for (a in 1:subj_cnt) {
  for (b in 1:activities_cnt) {
    independent_tidy_data[row, 1] = subj_unique[a]
    independent_tidy_data[row, 2] = activities[b, 2]
    one <- tidy_data[tidy_data$subject==a & tidy_data$activity==activities[b, 2], ]
    
    -- get average
    independent_tidy_data[row, 3:tidy_data_cols] <- colMeans(one[, 3:tidy_data_cols])
    row = row + 1
  }
}

-- write the data set to the file independent_tidy_data.txt
write.table(independent_tidy_data, "independent_tidy_data.txt", row.name=FALSE)