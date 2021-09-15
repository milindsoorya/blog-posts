## The ultimate guide to confusion matrix in machine learning

## Introduction

Yeah, the Confusion matrices are really confusing(No pun intended). In this article, I will go over some of the very important terms in Machine learning such as Accuracy, Precision and Recall. I will also answer some of the popular recurring questions related to the Confusion matrix and accuracy score such as -

1. What is the Confusion matrix in machine learning?
2. What is Accuracy, Precision and Recall?
3. What is the need for a Confusion matrix?
4. What is an F₁ score in machine learning?

let's start

## What is the Confusion matrix in machine learning?

To understand the confusion matrix we need to learn about the accuracy of a classification model. 

![accuracy.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631685868979/aqaEKYbRf.png)

**Accuracy** is the number of times you predicted something correctly divided by how many times you actually predicted it.

Classification accuracy alone can be misleading if you have an unequal number of observations in each class or if you have more than two classes in your dataset. Hence, Accuracy is not a suitable metric to use in situations where the data is skewed, so in these situations, we use a confusion matrix.


This is a confusion matrix, it shows you all the possible scenarios of the predictions of a model vs the ground truth. It is a technique for summarizing the performance of a classification algorithm.

![E1gLaTTXoAAB1qA.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631685723103/TKP3Nxy2l.png)

This meme explains is better, Consider the doctor to be the model and the patient to be the ground truth.
 
![confusion_matrix.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631685668996/VyIcoKi8A.png)

Let's co-relate this to a [spam classifier dataset](https://milindsoorya.site/blog/build-a-spam-classifier-in-python).

Case 1: Our model predicts a message as spam and actually it is spam (TP - True Positive)

Case 2: Our model predicts  a message as spam but in reality, it is not (FP - False Positive)

Case 3: Our model predicts  a message as not spam, but in reality, it is spam (FN - False Negative)

Case 4: Our model predicts  a message as not spam, and actually it is not spam(TN - True Negative) 

## What is the need for a Confusion matrix?

To answer the above question we need to understand what is **precision** and **recall**.

### Precision

Precision is defined using the below formulae

![E1gLaTfXMAIzzp7.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631685735243/MP4HKRwOX.png)

**Precision**: Out of all the positive classes we have predicted correctly, how many are actually positive Or How good is our model at not making false accusations?

### Recall

Recall is defined using the below formulae
 
![E1gLaTlX0AA2WRG.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631685753718/3phisH7fY.png)

**Recall**: Out of all the predictions, how many are actually correct?. This metric basically tells us how good our model is at identifying relevant samples.

Recall and precision are related such that high precision leads to low recall, and low precision leads to high recall. We obviously want both to be as high as possible and the way to do it is by making sure that false negative and false positive is small and almost similar.

we can calculate this by using a confusion matrix. By making sure that the False Negative and False positive are almost the same we can say that the train and test data are balanced and having high precision and recall.

> If you would like to see the implementation of a confusion matrix on a real dataset with an example, please refer to this article - [Mushroom dataset analysis and classification in python](https://milindsoorya.site/blog/mushroom-dataset-analysis-and-classification-python)

## What is an F₁ score in machine learning? 

In the previous section, we saw the importance of Precision and Recall in classification.

The F₁ score is the harmonic mean of precision and recall. The harmonic mean is a special type of mean(average) which is explained by this formula. 

![f1score.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631688045058/JD6spzsO_.png)

> We use this mean instead of the normal mean because the normal punish extreme values.

Now if we optimize our model to have an F₁ score, we can have high precision and recall.

😺 Thanks for reading the entire article, don't stop learning have a great day

## Related articles
- [How to build a Spam Classifier with 6 classifiers in python and sklearn](https://milindsoorya.site/blog/build-a-spam-classifier-in-python)
- [Introduction to Word Frequency in NLP using python](https://milindsoorya.site/blog/introduction-to-word-frequencies-in-nlp)