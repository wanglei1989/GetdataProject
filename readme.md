Getting and Cleaning Data Course Project
========================================================

## Loading the data(.txt files)

```r
x_test <- read.table("./UCI HAR Dataset/test/X_test.txt", header=F)
y_test <- read.table("./UCI HAR Dataset/test/y_test.txt", header=F)
subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt", header=F)
x_train <- read.table("./UCI HAR Dataset/train/X_train.txt", header=F)
y_train <- read.table("./UCI HAR Dataset/train/y_train.txt", header=F)
subject_train <- read.table("./UCI HAR Dataset/train/subject_train.txt", header=F)
features <- read.table("./UCI HAR Dataset/features.txt", header=F)
activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt", header=F)
```

## Combining the tables

```r
A <- cbind(y_test, subject_test)
colnames(A)<- c("Activity", "Subject")
B <- cbind(y_train, subject_train)
colnames(B)<- c("Activity", "Subject")
C<- rbind(B,A)
D <- rbind(x_train, x_test)
line <- ncol(D)
colnames(D) <- features[,2]
```

## Subsetting the table by mean and std

```r
mean <- grep("mean()",fixed=TRUE, colnames(D))
std <- grep("std()", fixed=TRUE, colnames(D))
all <- c(mean, std)
E <- D[,all]
table <- cbind(C,E)
write.table(table,"tidydata1.txt", row.names=TRUE, col.names=TRUE)
```

## Getting the tidy data

```r
tidydata <- data.frame(Activity=rep(activity_labels[,2], 30),Subject=rep(1:30, 6))
len <- ncol(table)
nm <- colnames(table)
for (i in 3:len){
        temp <- tapply(table[,i],list(table$Activity, table$Subject),mean)
        test <- c(temp)
        temp2<-data.frame(test)
        colnames(temp2)<-nm[i]
        tidydata <- cbind(tidydata, temp2)
}
```

## Output the second tidy datas
### Output the second tidy data: tidydata2.txt

```r
write.table(tidydata,"tidydata2.txt", row.names=TRUE, col.names=TRUE)
```
