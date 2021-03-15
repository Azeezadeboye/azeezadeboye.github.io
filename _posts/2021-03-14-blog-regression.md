---
title: 'Regression Analysis'
date: 2021-03-14
permalink: /posts/2021/03/blog-regression/
tags:
  - cool posts
  - category1
  - category2
---

#Linear Regression Demo in R
#Demo by Azeez Adeboye

#discover the data available within the data package
data()
data(cars)
View(cars)

#visualize the data 
plot(dist~speed, data=cars)
abline(model, col="blue")

hist(cars$speed)
hist(cars$dist)

counts <- table(cars$speed)
barplot(counts, main="Speed of cars Distribution", xlab="Stopping distances of cars")

#Correlation to show a relationship
cor(cars, use="complete.obs", method="pearson")
cor(cars, use="complete.obs", method="spearman")

#train a model of the form Y = β1 + β2X + ε, use the lm() command
model <- lm(dist~speed, data=cars)
summary(model) #To view additional details of the model
anova(model) #evaluating the model

#graphical of the statistical properties of our model by plot
plot(model)

#To demonstrate the capabilities of R by SSE, SST, coefficient of determination R^2, adjusted R^2
prediction <- predict(model, cars)
SSE <- sum((prediction - cars$dist)^2)
variance <- SSE/(n-2)
variance <- SSE/(50-2)
meanDist <- mean(cars$dist)
SST <- sum((cars$dist - meanDist) ^2)
r2 <- 1 - (SSE/SST)
r2adj <- ((n-1)*r2 - k) / (n-1-k)

#adding an additional independent variable (speed^2) 
cars2 <- cars
cars2["speed^2"] <- cars$speed^2
View(cars2)

#train the model using different training and test sets
set.seed(100)
trainingIndex <- sample(1:nrow(cars), 0.8*nrow(cars))
cars2_train <- cars2[trainingIndex, ]
cars2_test <- cars2[-trainingIndex, ]

#Run the 2nd model
model2 <- lm(dist ~ speed + `speed^2`, data=cars2_train)
summary(model2)

#to visualize the data in 3D plot
install.packages("scatterplot3d")
library("scatterplot3d")
cars2plot <- scatterplot3d(cars2_train$speed, cars2_train$`speed^2`, cars2_train$dist, angle=45)
cars2plot$plane3d(model2) # to draw the regression plane
# We can view further information about the new model (summary(), anove() and predict() command).

#Step-wise regression and Logistic Regression
state=as.data.frame(state.x77)
View(state)
cor(state)

step(lm(`Life Exp`~Murder, data=state), direction="both", scope=~Population+Income+Murder+Illiteracy+Area+Frost+`HS Grad`)
Start:  AIC=-14.61
`Life Exp` ~ Murder

#import dataset for Logistic regression
View(admissions)
admissions$rank <- factor(admissions$rank)
admissionsModel <- glm(admit ~ gre + gpa + rank, data = admissions, family = "binomial")
summary(admissionsModel)


plot(admissionsModel)


Headings are cool
======

You can have many headings
======

Aren't headings cool?
------
