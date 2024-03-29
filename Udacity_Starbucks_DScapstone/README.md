# Udacity DS Nanodegree Capstone Project

## Description
According to the task description provided by Starbucks and Udacity,
> "_This data set contains simulated data that mimics customer behavior on the Starbucks rewards mobile app. Once every few days, Starbucks sends out an offer to users of the mobile app. An offer can be merely an advertisement for a drink or an actual offer such as a discount or BOGO (buy one get one free). Some users might not receive any offer during certain weeks.  
Not all users receive the same offer, and that is the challenge to solve with this data set.  
Your task is to combine transaction, demographic and offer data to determine which demographic groups respond best to which offer type. This data set is a simplified version of the real Starbucks app because the underlying simulator only has one product whereas Starbucks actually sells dozens of products.  
Every offer has a validity period before the offer expires. As an example, a BOGO offer might be valid for only 5 days. You'll see in the data set that informational offers have a validity period even though these ads are merely providing information about a product; for example, if an informational offer has 7 days of validity, you can assume the customer is feeling the influence of the offer for 7 days after receiving the advertisement._" **Source: project description**.

Therefore, this represents a typical problem any retailer faces, where it has to keep its customers engaged by sending them offers and promotions.

For more information on the dataset provided, check file `Starbucks_Capstone_notebook_part1`, which contains the data exploration steps.


## Goal
With the task in mind, my goal will be to model whether one can predict if an offer will be successful based on customer demographics and some offer characteristics. For that, 2 tables will be organized: one containing the similar offers of `BOGO` (buy one and get one), as well as `discount`; and a second containing `informational` offers. The criterias to identify a successful offer are:

- **BOGO & discount offers:** for these offers to be considered successful, these events need to happen in sequence:
- > **offer received >> offer viewed >> transaction >> offer completed**

If these events take place in sequence for the same offer, we will consider it successful and our target for it will be `1`.

- **informational offer:** in this case we will need to observe the following:
- > **offer received >> offer viewed >> transaction**

If these events are seen in this order AND the transaction takes place during the influence period of the offer (variable `duration`), we will consider it successful and the target will also be `1`.

For all other scenarios, the offers will be considered unsuccessful and the target will be `0`.

Given these parameters, this is a supervised classification problem that will be approached by creating a benchmark model based on sklearn's `DummyClassifier` that will always predict the most common value. this way, if there are more negative cases in the data, the model will always predict `0`. Next, some other modeling techniques will be tested first without parameter tunning and the best model will be seleced for optimization.

During optimization, `RandomizedSearchCV` will be used for hyper-parameter tunning so that the models can present better results. After that, the metrics proposed will be evaluated and conlusions about the feasibility of the task will be made, as well as considerations about potential improvements.


## Methodology
**Methodology:** for that matter, a [`CRISP-DM`](https://www.sv-europe.com/crisp-dm-methodology/) approach will be used to cover the following main parts:  
- **Business Understanding:** understanding of how are the operations, how the data can be used to support them and what are the main objectives of the project.
- **Data Understanding & Preparation:** explore the data to observe the available information, any potential problems and constraints. In addition, any data related issues discovered here will be addressed as the data is ultimately prepared for modeling.
- **Data Modeling:** test different modeling approaches and optimize the one(s) with a better initial performance.
- **Evaluate the Results:** evaluate the results produced by the optimized classifier and understand how the outputs can best support the business.


## Metrics
Given these parameters, this is a supervised classification problem and, as such, potential metrics to evaluate the task's success are:
- [`accuracy score`](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html): according to the Scikit-Learn documentation, _the accuracy_score function computes the accuracy, either the fraction (default) or the count (normalize=False) of correct predictions._ This metric is widely used in such classification problems and it's best used for more balanced datasets (when the number of positive and negative cases are similar).
- [`f1 score`](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html): similarly, the F1 Score also evaluates the performance of the classifier, but as it takes into account both precision (_the ability of the classifier not to label as positive a sample that is negative_) and recall (_the ability of the classifier to find all the positive samples_), it normally has more evaluation power in unbalanced datasets. In fact, this is the harmonic mean of precision and recall and is defined as: `F1 = 2 * (precision * recall) / (precision + recall)`  Source: Scikit-Learn Documentation.

For both metrics, the higher, the better. In addition to these, other supporting evaluation metrics provided by the [classification report](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.classification_report.html), [confusion matrix](https://scikit-learn.org/stable/auto_examples/model_selection/plot_confusion_matrix.html) and [ROC Curve](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_curve.html) will be used.


## Models used
In our approach, different models will be tested on the data with default parameters. The best performing one will be optimized so we can improve our metrics.

Important to note that some features were organized with a tree-based model in mind. Such features are `year`, `month`, `week` and `day`. Each one has the period in which the customer has become a member. Together, they used to form the variable `became_member_on`, which was a time stamp. Note that for linear models, a more common approach would be to create dummy variables for at least `year` and `month`, since we wouldn't greatly increase the feature space. As `week` and `day` have many possible values, creating dummies out of them would increase the dimensions of the table without necessarily providing more information. Tree-based models make axis parallel partitions in the data and, thus, dummies are not necessary.

Nonetheless, some linear models will be tested on the data to provide us a reference of how they could perform.

The models that will be tested are:
- [SVC](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html)
- [Logistic Regression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)
- [Dummy Classifier](https://scikit-learn.org/stable/modules/generated/sklearn.dummy.DummyClassifier.html)
- [Random Forest Classifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)
- [XGBoost Classifier](https://xgboost.readthedocs.io/en/latest/)

`Dummy Classifier` will be used as a benchmark, as it will always yield the `most frequent value` in the data (this is the selected strategy)


## Conclusions
After an initial attempt of **predicting if an offer will be successful**, this appears to be a feasible task with the data available. Given demographic and offer characteristics, as well as behavioral data, the models seem to offer reasonable performances. This is specially true for the one trained with the `BOGO` and `discount` offer data. In this case, the model performs fairly good in predicting who would likely act upon one of this offers, by being actively engaged with the company. Once again, in order for these offers to be considered successful, the following criteria must be met:
- > **offer received >> offer viewed >> transaction >> offer completed**

Only if these events take place in sequence for the same offer, we will consider it successful (positive case). In this scenario, the best classifier was optimized and it yielded an ROC score of over 0.91, which is fairly high and could be used in a real life situation for such a task. Important to note here that sending promotional offers to customers does not require the same level of performance that a ML-model trained for a medical application for example, as a certain level of mistakes (customer don't act upon the offer) is unlikely to hurt the company by much.

Metrics for this case are:
- accuracy score: 0.8296
- f1_score: 0.7853
- roc_auc_score(test): 0.913

Moreover, the best parameters found show that the trees are quite shallow (`max_dept=3`) and not many estimators were necessary to produce the best results (`n_estimators=100`). In addition, `learning_rate`(0.1) also has a lower value, reinforcing the idea the the model can easily separate both positive and negative cases.


On the dataset, the one containing `informational` offers, the criteria for a successful offer is:
- > **offer received >> offer viewed >> transaction**

The classifiers for this dataset also show a good performance, however slight lower than the previous case. The results were:
- accuracy score: 0.7930
- f1_score: 0.7438
- roc_auc_score(test): 0.812

As one can observe, a similar conclusion as the one for the previous dataset is also valid, which states that a certain level of error might not be such a problem in an marketing department of a retailing company like Starbucks. In general, this classifier also performs well and its main optimized parameters `max_dept`(3), `n_estimators`(100) and `learning_rate`(0.1) present the same low values (given parameters' search ranges). In addition, these are the same ones found for the best classifier in the `BOGO` and `discount` case and it's likely that the provided seed played some part in it.


The optimized classifiers produced results that are only slightly better, which could indicate that potential improvements in 3 main areas:
- feature engineering and preprocessing: more often than not, this is the area that mostly improves a model's performance. Nowadays, the algorithms are so optimized that data scientists spend most of their time engineering the data so that new features can increase the model's metrics. Some examples could be different feature representations (binning, scaling, polynomials, derived features, feature selection, etc.)
- wider parameter spaces: another topic is to tune the model with wider (or with mode combinations of) parameter ranges. Some can greatly improve if properly tunned, but even the ones that are more robust to parameter settings can improve with it. Therefore, this is a must for every model built.
- other models: each algorithm has it's own way of reaching conclusions and there is not a particular way that will solve all problems better. So, additional models can potentially show better performances.

These areas could be explored for further improvements in the results, but overall the goal of **predicting if an offer will be successful given customer demographic information, as well as some of the offer's characteristics** is not only feasible, but the classifiers trained here show that the features have a great discriminatory power to support the task.

The algorithm implementation is one of the most interesting parts of the data science pipeline, but in this particular project, organizing the data has proven to be the most challenging task. That is true specially given the elaborated sequence of events that would need to take place and sometimes even within a time window. I certainly learned a great many deal of things with it and I'm proud of the solution I reached.


## Files
- `Starbucks_Capstone_notebook_part1`: this notebook contain the initial part of the analysis, where the data is explored, cleaned and engineered for modeling. Following the [`CRISP-DM`](https://www.sv-europe.com/crisp-dm-methodology/) guideline, this notebook comprehends the steps of `business understanding`, `data understanding` and `data preparation`.
- `Starbucks_Capstone_notebook_part2`: this notebook reads the clean datasets produced in the previous notebook and models the provided target. This file comprehends the steps of `data modeling` and `evaluation of results`.
- `requirements.txt`: contain the packages and versions used in this project. With this, anyone can recreate the environment necessary to run the code provided in the jupyter notebooks.


## Sources
- [Udacity Data Science Nanodegree Capstone project - Data provided by Starbucks](https://www.udacity.com/course/data-scientist-nanodegree--nd025)
- [Scikit-Learn Documentation](https://scikit-learn.org/stable/)
- [Pandas Documentation](https://pandas.pydata.org/pandas-docs/stable/)
- [SV-Europe: CRISP-DM Methodology](https://www.sv-europe.com/crisp-dm-methodology/)