# CND Paper

Often impartial classifications are required even if datasets are biased. this paper aims to massage the dataset by making the least intrusive modifications (hence the ordering business) in order to de-bias it.

redlining is the process of abusing the effect of attributes predicting other, sensitive, attributes.

the paper introduces a model which learns on biased data but works impartially for future data

changing the data can result in lower accuracy scores but the changes are kept as minimal as possible to minimise this trade off.

This has been tested on the German credit dataset and was able to teach classifiers that no longer discriminate future data without losing much accuracy.

# II formal definition of the problem and discrimination measure

we have a set of attributes, A, and a set of classes, C.
we have a dataset, D, described formally s.t. the entries are tuples with a label c from C
and a sensitive attribute SA which should be binary, s from SA is the sensitive instance.
we also have a desired class denoted + which is one of 2 classes in C

we divide the dataset into 4 parts:
1. s denotes all the rows that have the sensitive instance
2. s+ stores all the rows that have the sensitive instance and the positive class
3. s! stores all the rows that don't have the sensitive instance
4. s!+ stores all the rows that don't have the sensitive instance and do have the positive class.

we can now describe a discrimination metric using the sizes of the parts
discrimination = s!+ / s! - s+ / s
which we wish to minimise by creating a new dataset.

# III example

in their toy example we can see the discrimination score is 40% which is interpreted as old people are 40% more liely to be assigned the + class than young people.

we know now that the data is biased but wish to build an unbiased classifier.

# IV Proposed solution

We want to adjust the labels of the most likely discriminated and favoured individuals. to do this we need to rank the class labels with a classification model.

1. We learn a biased ranker for predicting the class without taking into account the sensitive attribute (simply omit this column).
2. We can then rank the instances to calculate the probability of the class for each row
3. Now we identify 2 groups, CP (candidates for promotion - belong to s and received -) and CD (candidates for demotion - don't belong to s and received +).
4. We can reduce discrimination by promoting objects from CP or demoting from CD. This is done in tandom and according to the ranker until we reach 0 discrimination measure.
5. We can calculate the number of swaps required to reach 0 discrimination with the formula in the paper.

the formula for the number of swaps is:
n = ( (s * s!+) - (s! * s+) ) / (s + s!) using the sizes of the classes

after learning a NB classifier we can add a new column to the dataset with the probabilities of the class label in it. This can then be used to create the CP and CD lists. The percentages are the probability of belong to the + class according to NB. 

we know how many swaps we need to make (n per class label) and we can do this easily.

there are two pseudocode algorithms which can be used

# V results in experimentation

learn a naive bayes classifier on the original data for comparison.

for all experiments we split the data into training and test but we don't massage the test set (this does mean the the accuracy is evaluated on biased data)

## German credit

there are 1000 instances with good or bad labels.
there are 20 attributes for each row, 13 categorical and 7 numerical.
we use Age as the sensitive attribute, discretized young and aged at 25.

with this split there are 190 Young and 810 aged.

the discrimination comes out as 15% which whilst bad isn't very bad.

Test both a dataset with the SA and one without the SA

depending how you sample (50% training) you get different discrimination for each dataset.

# VI Related work

not relevant

# VII Conclusion

Discrimination is tricky
CND is simple yet powerful as a starting point
it's able to classify future data from an biased training set with minimum discrimination and high accuracy.
it addresses the redlining problem.
