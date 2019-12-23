# Udacity DS Nanodegree Capstone Project

## Description
According to the task description provided by Starbucks and Udacity, "_This data set contains simulated data that mimics customer behavior on the Starbucks rewards mobile app. Once every few days, Starbucks sends out an offer to users of the mobile app. An offer can be merely an advertisement for a drink or an actual offer such as a discount or BOGO (buy one get one free). Some users might not receive any offer during certain weeks.  
Not all users receive the same offer, and that is the challenge to solve with this data set.  
Your task is to combine transaction, demographic and offer data to determine which demographic groups respond best to which offer type. This data set is a simplified version of the real Starbucks app because the underlying simulator only has one product whereas Starbucks actually sells dozens of products.  
Every offer has a validity period before the offer expires. As an example, a BOGO offer might be valid for only 5 days. You'll see in the data set that informational offers have a validity period even though these ads are merely providing information about a product; for example, if an informational offer has 7 days of validity, you can assume the customer is feeling the influence of the offer for 7 days after receiving the advertisement._" **Source: project description**.


## Goal
With the task in mind, my goal will be to model whether one can predict if an offer will be successful based on customer demographics and some offer characteristics. For that, 2 tables will be organized: one containing the similar offers of `BOGO` (buy one and get one), as well as `discount`; and a second containing `informational` offers. The criterias to identify a successful offer are:

- **BOGO & discount offers:** for these offers to be considered successful, these events need to happen in sequence:
- > **offer received >> offer viewed >> transaction >> offer completed**

If these events take place in sequence for the same offer, we will consider it successful and our target for it will be `1`.

- **informational offer:** in this case we will need to observe the following:
- > **offer received >> offer viewed >> transaction**

If these events are seen in this order AND the transaction takes place during the influence period of the offer (variable `duration`), we will consider it successful and the target will also be `1`.

For all other scenarios, the offers will be considered unsuccessful and the target will be `0`.

Given these parameters, this is a supervised classification problem.


## Metrics
**Methodology:** for that matter, a [CRISP-DM](https://www.sv-europe.com/crisp-dm-methodology/) approach will be used to cover the following main parts:  
- **Business Understanding:** understanding of how are the operations, how the data can be used to support them and what are the main objectives of the project.
- **Data Understanding & Preparation:** explore the data to observe the available information, any potential problems and constraints. In addition, any data related issues discovered here will be addressed as the data is ultimately prepared for modeling.
- **Data Modeling:** test different modeling approaches and optimize the one(s) with a better initial performance.
- **Evaluate the Results:** evaluate the results produced by the optimized classifier and understand how the outputs can best support the business.

## Methodology

## Conclusions

## Files

## Sources