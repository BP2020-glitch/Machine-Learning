### Project Background

Banking industry is highly competitive and banking products are easily to duplicate. The project provides business owners with a clear picture of what product is the most profitable one to be promoted, what customers to be targeted, what are their common behaviours and attributes, what can we do to keep them, how to convert potential churners to loyal customers before we lose them to our competitors on the first year.
There are two datasets being involved in this project. ‘bank_transaction’ is a customer level dataset which has columns CustomerID, TransactionID, TransactionDT, TransactionAmt, CardType, isRetention and Frequency. Each data point is a transaction record made by a customer. ‘cap_bank’ dataset has 48 columns and most of them are product name, so it is product level based.This project contains three main parts: A/B Test, Clustering, Classification.



#### Part One: A/B test

Techniques being applied: check missing values, check sizing on each group, downsampling, customer behaviour analysis, bootstrap sampling.

I conducted A/B test to decide what type of credit card we want to promote by comparing the mean of target metric ‘isRetention’ between two types of cardholder: platinum and gold.  Our goal is to see more or less Retention Rate of one group against the other after customers received their new credit card in one week. Retention rate is the percentage of customers that come back and use the card after they have activated it, which is an important metric in measuring new customers. The higher the retention rate is, the easier it is to retain customers and build a large customer base. We are certain of the difference in the existing retention rate, but how to tell the changes in the future? There are a couple of ways we can get at the certainty of retention numbers. Here I used bootstrapping: I repeatedly re-sample our dataset (with replacement) and calculate one week retention for those samples. The variation in one week retention will give us an indication of how uncertain the retention numbers are. I plotted the two distributions and the difference of two groups to represent the bootstrap uncertainty over what the underlying one week retention could be for the two AB-groups. From these graphs, we can see that the most likely % difference is around 4%, and that 92% of the distribution is above 0%, in favor of gold group. The bootstrap result tells us that there is strong evidence that one week retention is higher when people have gold card than others have platinum card. The conclusion is: If we want to keep retention high — we should upsell gold card to customers than only letting them keep existing platinum card.
There are, of course, other metrics we could look at, like the frequency or how much transaction amount are made by the two AB-groups. But retention is one of the most important metrics. If we don't retain our customer base, it doesn't matter how much money they spend using this credit card.




#### Part Two: Classification 

Techniques being applied: Logistic Regression, Random Forest, KNN, SVC, feature importance, downsampling, pipeline, gridsearch, hyperparameter optimization, accuracy score, precision, recall, F1 score, roc, and auc.

Considered this is a classification problem, I selected to fit Logistic Regression, Random Forest, KNN, SVC.
The accuracies on the train set and test set are 0.73 and 0.72 for a Logistic Regression model. I pull out confusion matrix and classification report. The conclusions are as the following: 
Recall : 39% of data points that our model correctly classified as class 1 among all the data points that belong to the class 1. That is low!
Precision: 67% of data points that our model correctly classified as class 1 among the number of data points that we either correctly or incorrectly classified as class 1.
In terms of improving model accuracy, firstly I lowered the threshold from 0.5 to 0.2 to increase recall and capture more members of the positive class. Recall does increase from 0.39 to 0.90. But the accuracies on the train set and test set are still the same as 0.73 and 0.72, didn’t change. Secondly, I used downsampling to handle imbalanced classes problem and restarted the modelling process, this time the accuracies on the train set and test set are 0.71 and 0.72. The accuracy on test set stay the same, however the accuracy on train set decreased by 0.01.



#### Part three: Clustering

Techniques being applied: check missing values, check duplications, missing values imputation, EDA, K-means, H-clustering, scree plot, silhouette score, rand index.

In order to understand our customers better so as to treat them differently, I conducted Unsupervised Machine Learning algorithms K-means and Hierarchical Clustering to compare the agreement percentage of the data so as to unhide the underlying patterns of our customers. The result shows model accuracy does increase after detecting 3 customer segments. The contingency table shows both the algorithms agree 21% of the data, it’s not a high number. So I looked into the rand score which is 0.46, not bad. The score range is between -1.0 and 1.0. It  confirms that there is some agreement between two clustering methods, so we can safely use the labels to do the prediction. After that, I fitted Logistic Regression, Random Forest, KNN and SVC again to compare both the accuracies before adding labels and after adding labels. Accuracies did increase except for Random Forest!But from the perspective of auc, Random Forest preforms the best.




### Findings and conclusions 

    1. We helped the bank identify that promote gold credit card could lead to the highest customer conversion rate.
    2. We figured out what customers have high probability to accept offers and what kind of attributes do they have through feature importance analysis.
    The most impotent features that play a huge influence in how much the likelihood of customers conversion based on the coefficient of each bank product. Customers have such bank products are the right audience we are looking for.  Because they are more likely to convert than others without these features.The most six biggest coefficient belong to features SAVBAL, DDABAL, MM, CD, SAV and CC.The most six smallest coefficient belong to DDA, CHECKS, ILS, MMBAL, ACCTAGE and PHONE.
    3. We detected 3 customer segments through clustering, which remind us to send tailored campaign offers and reach out them by correct channels
    4. We unhide the patterns of different customer segments based on each feature.
    5. Thanks to the highly targeting, we could save the marketing costs and improve ROI.

