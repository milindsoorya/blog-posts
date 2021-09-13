## Mushroom dataset analysis and classification in python

## Introduction

Mushroom classification is a beginner machine learning problem and the objective is to correctly classify if the mushroom is edible or poisonous by it's specifications like cap shape, cap color, gill color, etc. using different classifiers.

In this project I have used the following classifiers to make the prediction:-

1. Logistic Regression
2. KNN Logistic Regression,
3. SVM,
4. Naive Bayes
5. Decision Tree,
6. Random Forest Classifier

If you want to see the original notebook in Kaggle please visit [kaggle-milindsoorya](https://www.kaggle.com/milindsoorya/mushroom-classification#Classification-Methods).

## Dataset

The dataset used in this project contains 8124 instances of mushrooms with 23 features like cap-shape, cap-surface, cap-color, bruises, odor, etc.

you can download the dataset from kaggle if you want to follow along locally - [mushroom-dataset](https://www.kaggle.com/uciml/mushroom-classification)

The python libraries and packages we’ll use in this project are namely:

- NumPy
- Pandas
- Seaborn
- Matplotlib
- Graphviz
- Scikit-learn

## Loading the dataset

If you are using kaggle then loading the data is done for you, you just have to run the first cell. If you find that confusing you can refer the below piece of code.

### 🎐 Incase of kaggle

```python
import os
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)

for dirname, _, filenames in os.walk('/kaggle/input'):
    for filename in filenames:
        print(os.path.join(dirname, filename))

df = pd.read_csv('/kaggle/input/mushroom-classification/mushrooms.csv')
df.head()
```

### 🎐 Incase of google colab

If you are using google colab, you can either use google drive to load the data or upload the data from your pc, you just have to download the dataset from kaggle and when you run the code you can upload it to colab. I find the later method easier and you can do it like below.

```python
import pandas as pd

from google.colab import files
uploaded = files.upload()

df =  pd.read_csv("./mushrooms.csv")
```

Pandas `read_csv()` function imports a CSV file (in our case, ‘mushrooms.csv’) to DataFrame format.


## Import modules
```python
import pandas as pd
import numpy as np
import os 
import matplotlib.pyplot as plt
import seaborn as sns
```

## Examining the Data

🎈 - To get an overview of the dataset we can use the `.describe()` method

```python
df.describe()
```

The `.describe()` method will gives you the statistics of the columns.

- count shows the number of responses.
- unique shows the number of unique categorical values.
- top shows the highest-occurring categorical value.
- freq shows the frequency/count of the highest-occurring categorical value.

Here is a part of the output:

![describe](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k277t4mhf9arxo0mv0t9.png)
 

🎈 - To get the column names, number of values, datatypes etc we can use the `info()` method.

```python
# display basic info about data type
df.info()
```

![df_info](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/g1uzred8ky85itrvgxyv.png)
 

🎈 - Using `value_counts()` method we can see that the dataset is balanced

```python
# display number of samples on each class
df['class'].value_counts()

// Output
e    4208
p    3916
Name: class, dtype: int64
```

🎈 - Let's also make sure there are no null values

```python
# check for null values
df.isnull().sum()
```

## Data manipulation

The data is categorical so we’ll use LabelEncoder to convert it to ordinal. LabelEncoder converts each value in a column to a number.

This approach requires the category column to be of ‘category’ datatype. By default, a non-numerical column is of ‘object’ datatype. From the `df.info()` method, we saw that our columns are of ‘object’ datatype. So we will have to change the type to ‘category’ before using this approach.

```python
df = df.astype('category')
df.dtypes
```

Now that we have converted the columns to be of category type, we can use LabelEncoder to make the columns into machine understandable format.

```python
# Using LabelEncoder to convert catergory values to ordinal
from sklearn.preprocessing import LabelEncoder
labelencoder=LabelEncoder()
for column in df.columns:
    df[column] = labelencoder.fit_transform(df[column])

df.head()
```

![df_head](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/w5l676fdg7umjyyl93xu.png)
 

from the above figure we can see that veil-type has only one unique value and hence won't contribute anything to the data. So we can safely remove it.

```python
df = df.drop(["veil-type"],axis=1)
```

here `axis-1` means we are droping the entire column(ie. vertically), if it was `axis-0` we would be dropping the entire row(ie. horizontally).

## Preparing the data

We can make use of scikit-learn's `train_test_split` method for creating the training and testing data.

```python
from sklearn.model_selection import train_test_split
# "class" column as numpy array.
y = df["class"].values
# All data except "class" column.
x = df.drop(["class"], axis=1).values
# Split data for train and test.
x_train, x_test, y_train, y_test = train_test_split(x,y,random_state=42,test_size=0.2)
```

## Classification Methods

### 1. Logistic Regression Classification

```python
from sklearn.linear_model import LogisticRegression

## lr = LogisticRegression(solver="lbfgs")
lr = LogisticRegression(solver="liblinear")
lr.fit(x_train,y_train)

print("Test Accuracy: {}%".format(round(lr.score(x_test,y_test)*100,2)))

// Output

Test Accuracy: 94.65%
```

### 2. KNN Classification

```python
from sklearn.neighbors import KNeighborsClassifier

best_Kvalue = 0
best_score = 0

for i in range(1,10):
    knn = KNeighborsClassifier(n_neighbors=i)
    knn.fit(x_train,y_train)
    if knn.score(x_test,y_test) > best_score:
        best_score = knn.score(x_train,y_train)
        best_Kvalue = i

print("""Best KNN Value: {}
Test Accuracy: {}%""".format(best_Kvalue, round(best_score*100,2)))

// Output
Test Accuracy: 94.65%
```

### 3. SVM Classification

```python
from sklearn.svm import SVC

svm = SVC(random_state=42, gamma="auto")
svm.fit(x_train,y_train)

print("Test Accuracy: {}%".format(round(svm.score(x_test,y_test)*100,2)))

// Output

Test Accuracy: 100.0%
```

### 4. Naive Bayes Classification

```python
from sklearn.naive_bayes import GaussianNB

nb = GaussianNB()
nb.fit(x_train,y_train)

print("Test Accuracy: {}%".format(round(nb.score(x_test,y_test)*100,2)))

// Output

Test Accuracy: 92.18%
```

### 5. Decision Tree Classification

```python
from sklearn.tree import DecisionTreeClassifier

dt = DecisionTreeClassifier()
dt.fit(x_train,y_train)

print("Test Accuracy: {}%".format(round(dt.score(x_test,y_test)*100,2)))

// Output

Test Accuracy: 100.0%
```

### 6. Random Forest Classification

```python
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier(n_estimators=100, random_state=42)
rf.fit(x_train,y_train)

print("Test Accuracy: {}%".format(round(rf.score(x_test,y_test)*100,2)))

// Output

Test Accuracy: 100.0%
```

## Checking Classification Results with Confusion Matrix

In this section I will check the results with confusion matrix on Logistic Regression and KNN Classification.

A confusion matrix is a technique for summarizing the performance of a classification algorithm. Classification accuracy alone can be misleading if you have an unequal number of observations in each class or if you have more than two classes in your dataset.

Logistic Regression's accuracy was 97.05% and KNN's was 100%.

![confusion_matrix](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k4zj34ev4i55wlhtkstn.png)
 

```python
from sklearn.metrics import confusion_matrix
# Linear Regression
y_pred_lr = lr.predict(x_test)
y_true_lr = y_test
cm = confusion_matrix(y_true_lr, y_pred_lr)
f, ax = plt.subplots(figsize =(5,5))
sns.heatmap(cm,annot = True,linewidths=0.5,linecolor="red",fmt = ".0f",ax=ax)
plt.xlabel("y_pred_lr")
plt.ylabel("y_true_lr")
plt.show()
```

![confusion-lr](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hu0wrcpsvi5jaxnmegbz.png)
 

```python
# Random Forest
y_pred_rf = rf.predict(x_test)
y_true_rf = y_test
cm = confusion_matrix(y_true_rf, y_pred_rf)
f, ax = plt.subplots(figsize =(5,5))
sns.heatmap(cm,annot = True,linewidths=0.5,linecolor="red",fmt = ".0f",ax=ax)
plt.xlabel("y_pred_rf")
plt.ylabel("y_true_rf")
plt.show()
```

![confusion-rf](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/c44vskjwneb44eyfj25q.png)
 

## Conclusion

From the confusion matrix, we saw that our train and test data is balanced.

Most of classfication methods hit 100% accuracy with this dataset.

## You might also like:-

- [How To Set Up Jupyter Notebook with Python 3 on Ubuntu 20.04](https://milindsoorya.site/blog/how-to-Set-up-jupyter-notebook-with-python-3-on-ubuntu-20.04)
- [How to use python virtual environment with conda](https://milindsoorya.site/blog/how-to-use-virtual-environment-with-conda)

## References

- https://machinelearningmastery.com/confusion-matrix-machine-learning/
- https://towardsdatascience.com/categorical-encoding-using-label-encoding-and-one-hot-encoder-911ef77fb5bd
- https://medium.com/analytics-vidhya/mushroom-classification-using-different-classifiers-aa338c1cd0ff
