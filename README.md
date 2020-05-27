# MovieRecommendationEngine
Conducted **user-based collaborative filtering** in R and supplemented with **regularized regression** to compute user/movie bias for targets with no history data

## SUMMARY 
Our team simulated the **Netflix recommendation algorithm** (not the most current) to predict movie ratings with information gathered from all the UCD MSBA students. After thorough comparison, we chose **user-based collaborative filtering** normalized by centered mean. However, for new users/movies that we have no existing information on, we supplement it with **regularized regression** which can **calculate user/movie bias separately**, and use them for estimation. 

Apart from the specific scores we predicted for each user/movie, we got the benchmark score for the MSBA cohort, which suggests that *the whole class are kind critics or are slightly selective, meaning only watching the movies in which they have confidence.*
This recommender system can be used in many ways beyond movie prediction, such as in stocking prediction for white labeling companies, bulk buying quantities prediction for supply chain management and churn prediction in customer analysis. 

There are several more ways to finetune the model. 
On the algorithm side, we could mix the collaborative filtering with content-based filtering ; 
On the evaluation side, apart from error rate, we could also use Mean Average Precision, coverage, personalization, intralist, customer happiness index to further improve the model.

## MAIN PACKAGE 
The main package we use is 'recommenderlab' of which the documentation is attached too. 
The following is the common way of using it: 
```
model_train_scheme <- Realrating %>%
  evaluationScheme(method = 'cross-validation', 
                   train = train_proportion, # proportion of rows to train.
                   given = 1, 
                   goodRating = NA, # for binary classifier analysis.
                   k = 5)
                   
model_params1 <- list(method = "cosine",
                     nn = 10, # find each user's 10 most similar users.
                     sample = FALSE, 
                     normalize=NULL)

model1 <- getData(model_train_scheme, "train") %>% #fit on the 75% training data.
  Recommender(method = "UBCF", parameter = model_params1)
``` 

## MODEL SELECTION, EVALUATION, AND INTERPRETATIONS
We built several neighborhood models with different specifications on two approaches: **user-based and item-based collaborative filtering**, which predict ratings based on one user’s ratings of similar movies or a movie’s ratings by similar users respectively. And we chose the best model with the lowest error rate.

As a result, we found that user-based collaborative filtering **normalized** by centered mean (referred to as “UBCF_center”) has the lowest error rate. However, although such an approach is the best for predicting existing users, it can go far off for new users with no information. Under such circumstances, we will need user bias and movie bias.

The **regularized regression** is used here to compute all the user bias and movie bias, therefore generating an unbiased rating dataset to feed the UBCF_center model. But **the error rate (RMSE)** of the new version(referred to as “bias_remove model”) is still higher than the previous UBCF_center model. Therefore, we will use the more accurate, the UBCF_center model and only use our bias_remove model for new users/movie cases. 

## WHAT'S MORE 
The value of our recommendation system extends beyond the movie sector. For example, our model could help online wholesale companies predict the bulk buying quantities based on the characteristics of each retailer. It may also help white labeling companies to predict user activities and avoid over-stocking. 

It is possible to supplement collaborative filtering with **content-based filtering**, which compares the content of an item and its user profile . The method collects user information such as sex and age but not user history. Thus, it can solve the cold-start problems.

A few ‘value’ functions could be useful when evaluating and refining the system. A popular metric called the **Mean Average Precision** appraises the positive predicted value of a recommendation system. Other metrics include coverage, personalization, intralist. In addition, we can use the **churn rate** or **Customer Happiness Index (CHI)** to further explore how the recommendation effects on the users.

Customer analytics and the recommendation system are closely intertwined. For example, when we are predicting the likelihood a customer is going to churn, we can use the Mean Average Precision to evaluate how precise our model is. The more precise the customer analytics is, the better the recommendation system is.

