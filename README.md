# Getting and Cleaning Data Assignment

##------1. Browsing dataset
	Get understanding the relationship among dataset.

##------2. Merge training data


	##merge y_train and activity_labels AND Uses descriptive activity names to name the activities in the data set
		## get labels
		actLables <- read.table("activity_labels.txt")  
		## extract y_train
		yTrain <- read.table("./train/y_train.txt")     
		activity <- merge(yTrain, actLables,by = "V1", all = FALSE)$V2


	## combine activity and x_train
		xTrain <- read.table("./train/X_train.txt")
		features <- read.table("features.txt")
		newNames <- features[[2]]  ##change column name of xTrain
		colnames(xTrain) <- newNames
		xyTrain <- cbind(activity, xTrain)

	## merge xyTrain and subject
	 ## subjective list
		subject <- read.table("./train/subject_train.txt") 
	 ##change the column name    
		subject <- rename(subject, subject = V1)               

	##final dataset for training dataset
	xyTrain <- cbind(subject, xyTrain)

	nrow(xyTrain)

##------3. Merge test data AND Uses descriptive activity names to name the activities in the data set
	## get labels
	actLables <- read.table("activity_labels.txt")  
	## extract y_test
	yTest <- read.table("./test/y_test.txt")     
	activity <- merge(yTest, actLables,by = "V1", all = FALSE)$V2


	## combine activity and x_test
	xTest <- read.table("./test/X_test.txt")
	features <- read.table("features.txt")
	newNames <- features[[2]]  
	colnames(xTest) <- newNames      ##change column name of xTest
	xyTest <- cbind(activity, xTest)

	## merge xyTest and subject
	 ## subjective list
	subjectTest<- read.table("./test/subject_test.txt")
	 ##change the column name      
	subjectTest<- rename(subjectTest, subject = V1)                

	##final dataset for test dataset
	xyTest <- cbind(subjectTest, xyTest)


##------3. Merges the training and the test sets to create one data set.
	data <-rbind(xyTrain, xyTest)

##------4. Extracts only the measurements on the mean and standard deviation for each measurement.

x <- grep("[Mm]ean|[Ss]td|activity|subject", colnames(data))
data <- data[,x]



##------5. create independent tidy data set with the average of each variable for each activity and each subject.
	library(reshape2)
	##melt data in to id variables and measure variables
	dataMelt <- melt(data, id = c("subject","activity"))    
	dataDcast <- dcast(dataMelt, subject + activity ~ variable, mean)

##------6. output the datasets
	 ## Save the clean data.
	path <- file.path("cleanData.csv")
	write.csv(data, path, row.names = FALSE)
	 ##Save the aggregated datasets
	path <- file.path("dataDcast.csv")
	write.csv(dataDcast, path, row.names = FALSE)

##-----7 convert the process above into a function called run.analysis(): please see the original code (run_analysis.R)

