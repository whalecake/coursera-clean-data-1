The experiment from which the Samsung data was collected involved 30 volunteers and 187 3-axial measurements from an accelerometer and gyroscope, across 6 different activities.

# Lists out the measurements 
        trainx <- read.table(file="./UCI HAR Dataset/train/X_train.txt")
        testx <- read.table(file="./UCI HAR Dataset/test/X_test.txt")

# Lists out the activities that correspond with the measurements
        testy <- read.table(file="./UCI HAR Dataset/test/Y_test.txt")
        trainy <- read.table(file="./UCI HAR Dataset/train/Y_train.txt")

# 9 subjects from the test & 288-381 observations of each
        subject_test <- read.table(file="./UCI HAR Dataset/test/subject_test.txt")
# 21 subjects from the training set & 281-409 observations of each
        subject_train <- read.table(file="./UCI HAR Dataset/train/subject_train.txt")

# lists out features
        features <- read.table(file="./UCI HAR Dataset/features.txt")
# indexes V1-V561 so that features can be used to name columns in subject_test
        features$V1 <- sub("^", "V", features$V1)

# use features data frame to name columns in both trainx and testx
        names(testx) <- features$V2
        names(trainx) <- features$V2

# extracts only the measurements on the mean and stdev for each measurement
        sub_testx <- testx[,grepl("mean",names(testx)) | grepl("std",names(testx))]
        sub_trainx <- trainx[,grepl("mean",names(trainx)) | grepl("std",names(trainx))]

# bind train, test, activity, and subject rows & columns together
        fulltest <- cbind(subject_test,testy, sub_testx)
        fulltrain <- cbind(subject_train, trainy, sub_trainx)
        full <- rbind(fulltest, fulltrain)

# rename columns
        names(full)[1] <- "subject"
        names(full)[2] <- "activity"

# full data set with descriptive variable names & values
        full <- full %>%
                mutate(activity = ifelse(activity=="1", "walking",
                                         ifelse(activity=="2", "walking upstairs",
                                                ifelse(activity=="3", "walking downstairs",
                                                       ifelse(activity=="4", "sitting",
                                                              ifelse(activity=="5", "standing",
                                                                     ifelse(activity=="6", "laying",
                                                                            NA)))))))

# Cleans variable names of features, removing hyphens and (), using camelcase
        names(full) <- gsub("-","",names(full))
        names(full) <- gsub("mean","Mean",names(full))
        names(full) <- gsub("std","Std",names(full))
        names(full) <- gsub("\\()","",names(full))

# Create a second, independent tidy, data set with the avg for each activity and each subject
        grouped_means <- aggregate (.~subject + activity, full, mean)
# Renamed column names in grouped_means data to have 'Mean of' prefixed
        colnames(grouped_means)[3:81] <- paste("Mean of", colnames(grouped_means[,c(3:81)]))

# Wrote table to .txt file
        write.table(grouped_means, file="tidydata_means.txt", row.name=FALSE)
