# Arvato-Financial-Services

## Project motivation

In this project, a clustering technique and a classification model are used to address some of Arvato Bertelsmann business challenges.

Arvato Bertelsmann  is a financial services provider which uses “predictive analytics, leading-edge platforms and big data” to guide their clients to optimized financial performance, specially focusing on credit management. They are present in 15 countries with particular focus on the European market. 

A mail-order sales company in Germany hired Arvato with the particular interest of identifying the parts of the population that best describe their core customer base in the country. 

Furthermore, they intended to gather information to define targets for a marketing campaign, by predicting which individuals are most likely to convert into becoming their customers. 
 
This project is part of Udacity’s Data Science Nanodegree capstone project. So all the data and files used are limited to what Udacity offered in the course material.

## Data

The data provided by Udacity are divided into four sets. In some cases, samples were taken to reduce processing time:
* *azdias_sample.pkl*: contains 366 features for the 133683 German citizens. It's a random sample representing 15% of the original data set provided by Udacity.
* *customers.pkl*: contains 369 features for the 153215 customers. This represents 80% of the orginal data base available.
* *camp_train.pkl*: train set with 367 variables of  42962 individuals that were targeted for a marketing campaign. The RESPONSE variable states whether or not the individuals have become  customers after the campaing. 
* *camp_test.pkl*: train set with 366 variables of  42962 individuals that were targeted for a marketing campaign. The RESPONSE variable was withdraw.

Two Excel spreadsheets with information about the data were also provided: 
* DIAS Information Levels - Attributes 2017.xlsx – is a top-level list of attributes and descriptions, organized by informational category. 
* DIAS Attributes - Values 2017.xlsx – is a detailed mapping of data values for each feature in alphabetical order. 

Unfortunately, several important features found in the data sets were not described in neither spreadsheets, so appropriate interpretation of some results found in the project was not possible. 

Detailed information about the columns’ scales were also missing (eg. if categorical features were nominal or ordinal), hindering the possibility of more adequate data preparation.

## Methodology and algorithms

The three jupyter notebooks described below were are used for this project. The data wrangling script is a step previous to the other two.

*Arvato Project Workbook 0 - data wragling*

Imports and perforns data treatments for the four data sets described above in the *Data* item. The treatments applied includes:
* Exclusion of features with a high percentage of missing values or zeros
* Exclusion of columns that seemed to represent identification of each individual or data processing information (eg. “LNR” and “EINGEFUEGT_AM” columns)
* Replacement of missing values by -1 or 9, for features were these values represent “unknown” category. For the other features, the median of the distribution of the non missing were defined as replacement values.
* Transformation of categorical variables into dummy variables for a more adequate use in machine learning algorithms
The result of this process are four wragled data sets: azdias_sample_clean.csv, customers_clean.csv, mailout_train_clean.csv and mailout_test_clean.csv.

*Arvato Project Workbook 1 – Segmentation*

Script used to find segments within the data. It basically performs a k-means clustering and explains the resulting clusters, following these steps:
* Perform feature scaling
* Definition of the number of clusters 
* Process a k-means clustering fitting
* Create a simple classification tree model to find the most important features that differentiate the defined segments
* Applies the segmentation process to find how both azdias_sample_clean and customers_clean set and displays exploratory analysis explaining how both the German individuals as a whole and the company’s customers fit in the defined segments. 

*Arvato Project Workbook 2 - classification model*

Several supervised learning techniques were tested using a grid search function, for parameter tuning, and applying a 3-fold cross validation for model evaluation. Special attention was given to tree based algorithms such as random forest, gradient boosting and adaboost. 
The camp_train.csv was used for model fitting. It includes 403 features.
Due to processing limitations the hyperparameter tuning process was restrained. For the three up mentioned algorithms, the following grid of parameters were tested (the other parameters kept the default values):
* Random Forest – 'n_estimators': [300, 400, 500], 'min_samples_leaf': [50, 100, 200] 
* Adaboost – 'n_estimators': [50, 100, 150] and 'learning_rate': [0.001, 0.01, 0.05]
* Stochastic Gradient Boosting (SGB) – 'max_depth': [2,4,5], 'n_estimators': [100, 150, 250],  subsample:[0.7, 0.75, 0.85] and 'learning_rate': [0.0001, 0.001, 0.01, 0.1]

As the response variable is highly unbalanced (only 1,24% of “1” and 98,76% of “0”), the area under the ROC curve (AUROC) was used to evaluate the performance, instead of accuracy. 

Stochastic Gradient Boosting showed the best result, with an AUROC of 76,62%, against 76,22% for Adaboost and 73,75% for Random Forest. 
The parameters defined by the grid search for SGB were a learning rate of 0.001, a maximum tree depth (max_depth) of 4, subsamples of 0.7 and 150 as the number of estimators.

All the files used in this project can be found in [this]( https://github.com/captorres/arvato-financial-services.git) Github repository.
