# Homecredit Capstone Project

This is a group project that I completed for my capstone with 3 other colleagues. The dataset that was utilized for this project can be found on Kaggle: https://www.kaggle.com/competitions/home-credit-default-risk/data

## Business Problem

Homecredit is a company that gives loans to the underserved population. In other words, these are clients who do not have conventional or nonexistent credit histories. Homecredit however, like any conventional financial institution faces financial loss if loans are given to clients who cannot pay it back. The median financial loss for approving a client who cannot pay back a loan is **$24,412 dollars**. Furthermore, Homecredit must also deal with the issue of forgone revenue if it rejects clients who could pay back the loan. The median potential revenue loss for incorrectly rejecting a client is **$23,800 dollars**. Both of these outcomes are bad and severely impact Homecredit's ability to operate.

### Project Objective

The objective of this project is to minimize both of these losses which will enable HomeCredit to operate more efficiently. It will also ensure that Homecredit can more accurately approve/reject applications.

## Problem Solution

Our group approached this problem by first conducting Exploratory Data Analysis individually. Afterwards, we reivewed our results and created a final clean dataset to use for training models. The next step was experimenting with various models like, Logistic Regression, Random Forest, XGboost, etc. After we were done doing hyperparameter tuning and experimenting with the various models, we selected the model with the best AUC Score. 

The final model we selected was the Resampled XGboost model which had a AUC of .99 on the train set and .98 on the test set when we cross validated the model. The model had an AUC of .72 on the holdout data provided by Kaggle. In terms of dollars, this model helped reduce the median loss of approving a client who could not pay a loan to $number. This represents a _% compared to the previous median amount $number. 

## My Contribution

I contributed to the project by training and evaluating the following models: 
* Decision Trees (Weighted and Unweighted)
* Logistic Regression
* Naive Bayes
* Random Forest

I also assisted with training the Random Forest and XGboost Models that we did as a group. I also additionally assisted with submitting the model predictions to Kaggle. Finally, I also performed Outlier Analysis, Feature Engineering, Data Preparation. For instance, I created a variable called `House_Attribute_Low_Variance` that combined all the housing related variables that had low variance into one column. I also helepd with one-hot encoding several numeric variables that should have been categorical like `FLAG_DOCUMENT_3` which is just a binary variable.

## Difficulties Encountered

During the course of this project, my team encountered several difficulties: 

* __Imbalanced Dataset__: The Target variable in the dataset which is named "TARGET" was highly imbalanced with 92% of the dataset consisting of clients who sucessfully paid of the loan. 8% of the clients failed to pay back the loan. This severely affected the performance of the models because the models would sucessfully predict the majority class (No Default) but fail to predict the minority class (Default)
  * __Solution:__ Undersample the majority class which helped improve model performance tremendously. For instance as previously mentioned, our best performing model XGboost had an ROC of .99 on the train set and .98 on the test set.

* __Missing Values__: Several of the columns had values that were missing. The issue was further excuberated by the issue that several of the colummns were interelated which meant that dropping columns with NA Values would be insufficient. For instance, there are two variables that in the main dataset that are closely related:
  * `FLAG_OWN_CAR`
  * `OWN_CAR_AGE`
  * `FLAG_OWN_CAR`indicates whether someone owns a car or not while while `OWN_CAR_AGE` is the age of the car. The issue however is that someone does not own a car, this is marked as `NA` in the `FLAG_OWN_CAR` variable. To solve this issue, I imputed any `NA` values in the `OWN_CAR_AGE` with zero.
* There however were several variables like this including highly correlated variables like `COMMONAREA_AVG`, `COMMONAREA_MODE` and `COMMONAREA_MEDI` that had missing values as well.
  * __Solution:__ Carefully analyze all the variables to see how they are interelated. Afterwards, imputation was used when deemed appropriate based on the variable relationships. We also utilized packages like MICE (Multi-Variate Imputation by Chained Equations) for some of the highly correlated variables.

## Project Insights

I gleaned several insights from this project, here are some of the key insights: 

* __Imbalanced Datasets__: I have learned how to more adequately address imbalanced datasets through various techniques like undersampling the majority class or oversampling the minority class. Other techniques include adding more weight to the minority class which I experimented with in Decision Trees.

* __Cleaning Data__: I got a better understanding of how clean the data by carefully examining the relationships between variables. For instance, one should not just impute a column with the median if the column is interrelated with another column. Additionally, outliers should be carefully considered because there may be several observations which are genuine. Thus, a conservative approach like utilizing the Local Outliers Algorithm with conservaters paramters is ideal. This helps preserve rows which is crucial for any model to learn patterns in the data. Variables should also be carefully considered before being dropped or retained because these variables may contain information that can help a model learn. For instance, `EXT_SOURCE_2` was a very important predictor for several models despite having missing values. If this variable had been dropped, then any model would have lost valuable insights into predicting default.

* __Cross Validation__: Cross Validation is crucial to ensuring that a model generalizes to any dataset. I learned more about effecitvely cross-validating my model by building several models and evaluating them on the train and test splits.

* __Hyper-Paramter Tuning__: Some of the models like XGboost and Random Forest were incredibly powerful, however they overfitted to the train dataset. Thus, to deal with these issues, I experimented with various parmaters like regularization parameters, tree_depth, etc. for Random Forest and XGboost. I additionally learned more about Grid Search and its ability to search through several hyperparameter combinations more efficiently, especially for Black Box Models like Nueral Networks and XGboost. I also learned more about threshold probablities for algorithms like Logistic Regression as well.

* __Business Insights__: It is very important to translate model results into business insights for Home Credit. For instance, this model helps save Home Credit $X dollars on bad rejections, etc.

