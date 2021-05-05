# Disparate impact definition
mentions the 80% rule - if the rate of hiring/loaning an underprivileged group is less than 80% of a privileged group then there's disparate impact

the problem is that unprotected attributes often predict protected ones such as location predicting race
you can't just ignore the unprotected attribute because it may have useful information

# Useful Classifiers
- Naive Bayes
- SVMs

we use a confusion matrix for evaluation
- accuracy
- balanced error rate (formula given in the paper) (0.5 x ((FN/(FN+TP)) + (FP/(FP+TN))
- we measure the utility of a model as 1-BER

# Previous work
there are two approaches for reducing bias, modify the data or modify the ML algorithm.

## CND - Classification with No Discrimination
this is most similar to the one the paper uses.
we take a biased data set with a single BINARY FEATURE (Eg. Race), we call this X.
we also take one of the two values X has and call it x.
the measure of discrimination we use KCDM (Kamira-Calders Discrimination Measure) is calculated as:
P(1 | X != x) - P(1 | X = x) where 1 is the desirable class (good credit) and x is the instance of the underprileged group.
so this reads as the probability that the credit is good given the applicant wasn't in the underprivileged class - the probability that the credit is good given that they were in the underprivileged class.

naturally we wish to minimse the KCDM

### Learning a CND
1. learn a Naive Bayes classifier
2. Pick two groups from the data s.t. they meet the following two conditions
- group one are CP (entries belong to underprivileged and were classified negatively) these are candidates for promotion
- group two are CD (entries belong to privileged and were classified positively). These are candidates for demotion.
3. Sort the candidates in CP group by decreasing probability (where the probability is the output from the NB classifier to describe the likelihood of it receiving the positive outcome). this means the first entry is the most likely underprivileged entry that missed out.
4. Sort the candidates in CD by increasing probability. this means the first entry is the least likely privileged entry that got a positive classification
5. go through the two lists side by side and swap the classes, retesting the KCDM and stop when it reaches 0.

the naive bayes classifier gives a probability for +ve class for each entry, but the dataset has the true class label. we swap those class labels to create a new dataset. you can then use any ML algorithm to train with that dataset and the result will be an unbiased model.

the authors of the old paper (not this one) used age as the protected class: x is young and !x is old. They wanted to assign good credit scores so good is the +ve class.

## Naive Bayes adaptations
The other method for reducing discrimination is to adapt the algorithms we use. By adapting Naive Bayes in the following three ways we can reduce discrimination a bit.

1. Modified NB. This works by adjusting the probabilities of a label being assigned based on the classes and is not hugely relevant.
2. 2 Naive Bayes. Train 2 classifiers on subsets of the dataset, those entries belong to the underprivileged class belong to one subset and the others belong to another. Depending on the value of X the appropriate classifier is selected. Each classifier receives no useful information about the value of X so it can't discriminate based on X. This method gives good accuracy and low KCDM.
3. Latent variable model, according to the discussion this didn't work well.

## Prejudice Remover Regularizer
this works with logistic regression and i'd rather not learn about it.

# Applying the methods
The novel contribution of the paper is a method outlined in section 4 which is applied to the datasets in section 5 and evaluated in section 6. I reckon I'll have an easier time of this by using the paper which describes the dataset adjustment 
