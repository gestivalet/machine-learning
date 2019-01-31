# Udacity Machine Learning Capstone Project

**Update - Jan 31th, 2019:** the file `model4.sav` was removed from this repo due to file size constraints.

**by Gabriel Estivalet
October 19th, 2018.**


## ORIGINAL DATA
The data is provided on Kaggle and can be accessed through the link https://www.kaggle.com/c/home-credit-default-risk/data

From all the tables made available by Home Credit, only the "application_train.csv" was used for simplification purposes.


## TRANSFORMED DATA
In the Jupyter notebooks, some transformations were applied in the original dataset, as well as descriptive statistics were collected. This way, 2 csv files were created:

- data_sampled: the data that was balanced and imputed in the Preprocessing phase. This was later transformed and used to fit the models.

- data_description: descriptive statistics and overall information for all the variables in the original dataset.


## MODELS
Due to the size of the ".sav" files, only model 4, which was the best performing one, will be provided in the accompanying files. However, for descriptive purposes, the models trained are the following:

- model1: trained on original features
- model2: trained on transformed features (RobustScaler+PCA and SelectFromModel)
- model3: trained on transformed features (quantile_transform+PCA and SelectFromModel)
- model4: trained on selected features (SelectFromModel)

## MODEL PARAMETERS
All models were trained with RandomizedSearchCV, which evaluates random combinations of the parameters value options in order to find the best performing combination. All the parameter search results for best performances of all models are available in the "model_parameters.html" file.


## VERSION
This project was done in python 3 with all the required modules saved in the "estivalet_env.yaml" file that is provided. Therefore, in case of any compatibility problems with package's versions, a copy of the environment used to develop this project can be created with the following command:

`conda env create -f estivalet_env.yaml`
