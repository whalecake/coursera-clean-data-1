tidydata_means.txt

The experiment from which the Samsung data was collected involved 30 volunteers and 187 3-axial measurements from an accelerometer and gyroscope, across 6 different activities.

Variables:

- Subject (int): an aggregate of 30 subjects, labeled 1-30, extracted from the subject_test.txt & subject_train.txt
- Activity (chr): extracted from the trainy.txt & testy.txt files
        - laying (previously, 6)
        - sitting (4)
        - standing (5)
        - walking (1)
        - walking downstairs (2)
        - walking upstairs (3)
- Features (num): 79 measurements extracted from testx.txt & trainx.txt, with variables renamed
        - Means & StDev, across the X, Y, and Z-axis collected for the following variables:
                - tBodyAcc (XYZ)
                - tGravityAcc (XYZ)
                - tBodyAccJerk (XYZ)
                - tBodyGyro (XYZ)
                - tBodyGyroJerk (XYZ)
                - tBodyAccMag (XYZ)
                - tGravityAccMag (XYZ)
                - tBodyAccJerkMag (XYZ)
                - tBodyGyroMag (XYZ)
                - tBodyGyroJerkMag (XYZ)
                - fBodyAcc (XYZ)
                - fBodyAccJerk (XYZ)
                - fBodyGyro (XYZ)
                - fBodyAccMag
                - fBodyAccJerkMag
                - fBodyGyroMag
                - fBodyGyroJerkMag
- Mean of Features (num): the average of the 79 measurements across activity per subject
        - 'Mean of tBodyAccMeanX': mean of all 'tBodyAccMeanX' values measured per subject and per activity
        
Cleaning Up Data:
1. Used read.table to load all .txt files into R
2. Used the features.txt file to rename feature measurement columns
3. Extracted only the mean and standard deviation measurements of features
4. Used cbind() to bind together subject, activity, and measurements data
5. Used rbind() to bind together training and testing datasets
6. Cleaned up variable names (ie: from 'tBodyAcc-mean()-x' to 'tBodyAccMeanX')
7. Used aggregate() to take the grouped means of measurements per subject and per activity