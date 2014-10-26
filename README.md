## Steps taken to clean the data

My R script (run_analysis.R) perform the following steps to clean the original data set and create an independent tidy data set.

* Load "training data set" and "test data set" to the memory and merge them
<ol>
<li>train <- read.table("X_train.txt",header=FALSE)</li>
<li>test <- read.table("X_test.txt",header=FALSE)</li>
<li>train <- rbind(train,test)</li>
</ol>
* Load and merge "training subject list" and "test subject list"
<ol>
<li>subject <- read.table("subject_train.txt",header = FALSE)</li>
<li>subjecttest <- read.table("subject_test.txt",header=FALSE)</li>
<li>subject <- rbind(subject,subjecttest)</li>
</ol>
* Load "features.txt", "y_train.txt" and "y_test.txt" to get column headings and activities for training and test sets. Combine training and test activities.
<ol>
<li>headerc <- read.table("features.txt",header = FALSE)</li>
<li>activity <- read.table("y_train.txt",header = FALSE)</li>
<li>activitytest <- read.table("y_test.txt",header = FALSE)</li>
<li>activity <- rbind(activity,activitytest)</li>
</ol>
* Load labels for activities and create a descriptive activity names
<ol>
<li>actlabel <- read.table("activity_labels.txt",header = FALSE)</li>
<li>activities <- as.character(actlabel[activity[[1]],2])</li>
</ol>
* Appnd Subject and Activity columns to the data set
<ol>
<li>train <- cbind(subject,train,activities)</li>
<li>colnames(train) <- c("Subject",as.character(headerc[,2]),"Activity")</li>
</ol>
* Extracts mean and standard deviation for each measurement and assign to another variable
<ol>
<li>x <- grep("mean()", colnames(train),fixed=TRUE,value=FALSE)</li>
<li>y <- grep("std()", colnames(train),fixed=TRUE,value=FALSE)</li>
<li>x <- sort(c(x,y))</li>
<li>tidy <- train[,c(1,x,563)]</li>
</ol>
* Remove the unnecessary data frames from the memory and load "dplyr" to do grouping and summarizing. 
<ol>
<li>rm(train)</li>
<li>library(dplyr)</li>
<li>stidy <- tbl_df(tidy)</li>
<li>rm(tidy)</li>
<li>gtidy <- group_by(stidy, Subject, Activity)</li>
<li>ftidy <- summarise_each(gtidy,funs(mean))</li>
</ol>
* Finally, write the data frame to a text file for uploading.
<ol><li>write.table(ftidy, file="final_tidy.txt",row.names=FALSE)</li></ol>




## About Original Data Set

This project uses data from
the <a href="http://archive.ics.uci.edu/ml/">UC Irvine Machine
Learning Repository</a>, a popular repository for machine learning
datasets. In particular, I will be using the "Human Activity Recognition Using Smartphones Data Set " 


* <b>Dataset</b>: <a href="https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip">Human Activity Recognition Using Smartphones Data Set </a>

* <b>Description</b>: The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ. These time domain signals (prefix 't' to denote time) were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 

Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals). 

These signals were used to estimate variables of the feature vector for each pattern:  
'-XYZ' is used to denote 3-axial signals in the X, Y and Z directions.

* tBodyAcc-XYZ
* tGravityAcc-XYZ
* tBodyAccJerk-XYZ
* tBodyGyro-XYZ
* tBodyGyroJerk-XYZ
* tBodyAccMag
* tGravityAccMag
* tBodyAccJerkMag
* tBodyGyroMag
* tBodyGyroJerkMag
* fBodyAcc-XYZ
* fBodyAccJerk-XYZ
* fBodyGyro-XYZ
* fBodyAccMag
* fBodyAccJerkMag
* fBodyGyroMag
* fBodyGyroJerkMag

The set of variables that were estimated from these signals are: 

* mean(): Mean value
* std(): Standard deviation
* mad(): Median absolute deviation 
* max(): Largest value in array
* min(): Smallest value in array
* sma(): Signal magnitude area
* energy(): Energy measure. Sum of the squares divided by the number of values. 
* iqr(): Interquartile range 
* entropy(): Signal entropy
* arCoeff(): Autorregresion coefficients with Burg order equal to 4
* correlation(): correlation coefficient between two signals
* maxInds(): index of the frequency component with largest magnitude
* meanFreq(): Weighted average of the frequency components to obtain a mean frequency
* skewness(): skewness of the frequency domain signal 
* kurtosis(): kurtosis of the frequency domain signal 
* bandsEnergy(): Energy of a frequency interval within the 64 bins of the FFT of each window.
* angle(): Angle between to vectors.

Additional vectors obtained by averaging the signals in a signal window sample. These are used on the angle() variable:

* gravityMean
* tBodyAccMean
* tBodyAccJerkMean
* tBodyGyroMean
* tBodyGyroJerkMean


