## Introduction to Word Frequencies in NLP


Natural Language Processing is one of the most commonly used techniques which is implemented in machine learning applications — given the wide range of analysis, extraction, processing and visualizing tasks that it can perform. The primary goal of this exercise is to tokenize the textual content, remove the stop words, and find the high-frequency words.

The libraries that we are going to use:

- Beautifulsoup: To scrape the data from the HTML of a website and it also helps to process only the text from these HTML codes
- Regular Expressions: Also known as Regex. It will convert the noise data containing special characters and carry the conversion of uppercase to lowercase characters
- NLTK (Natural Language Toolkit): For the tokenization of the sentences into a list of words

In the end, we will look at how the graph looks and also the tokenized word count.


## Data processing
1. Import the libraries needed
2. Load the data from the link
3. Extract the data from HTML
4. Convert text to tokens

## Using NLP
1. Remove stop words from the text
2. Get a list of all words after removing stop words
3. Analysing word frequency and plot the graph

## Environment setup
I used anaconda to setup a python virtual enviroment in conda. You can read how to set it up in this [article](https://milindsoorya.site/blog/how-to-use-virtual-environment-with-conda) 

Using a [Jupyter Notebook](https://jupyter.org/) is the recommended approch to run the following exercise. Jupyter notebook comes preinstalled with anaconda and you can run it by  the command `jupyter notebook` from your anaconda prompt.

## Import the needed libraries


```python
#import the required packages
import requests
from bs4 import BeautifulSoup
import re
import nltk
import seaborn as sns
import matplotlib.pyplot as plt
```

## Load dataset from the link


```python
#get the dataset from link
dataset='https://www.gutenberg.org/files/1661/1661-h/1661-h.htm'
reading=requests.get(dataset)
```

## Extract text from HTML


```python
html=reading.text

# extract the text using web scraping tool
data=BeautifulSoup(html,"html5lib")

data.title

// OUTPUT
<title>The Project Gutenberg Book of The Adventures of Sherlock Holmes, by Arthur Conan Doyle</title>
```


```python
# [:12] gives the first 12 elements in the matched array
data.findAll('a')[:12]

// OUTPUT
    [<a href="https://www.gutenberg.org">www.gutenberg.org</a>,
     <a href="https://www.gutenberg.org/ebooks/48320">
     [ #48320 ]</a>,
     <a href="#chap01">A Scandal in Bohemia</a>,
     <a href="#chap02">The Red-Headed League</a>,
     <a href="#chap03">A Case of Identity</a>,
     <a href="#chap04">The Boscombe Valley Mystery</a>,
     <a href="#chap05">The Five Orange Pips</a>,
     <a href="#chap06">The Man with the Twisted Lip</a>,
     <a href="#chap07">The Adventure of the Blue Carbuncle</a>,
     <a href="#chap08">The Adventure of the Speckled Band</a>,
     <a href="#chap09">The Adventure of the Engineer’s Thumb</a>,
     <a href="#chap10">The Adventure of the Noble Bachelor</a>]
```


## Convert text into tokens


```python
# tokenize the text with regular expressions
# "w+": This expression matches the alphanumeric character in the text
text=data.get_text()
token=re.findall('\w+', text)
token[:10]

// OUTPUT
     ['The',
     'Project',
     'Gutenberg',
     'Book',
     'of',
     'The',
     'Adventures',
     'of',
     'Sherlock',
     'Holmes']
```



```python
words=[]
for word in token:
    words.append(word.lower())
words[:8]

// OUTPUT
    ['the', 'project', 'gutenberg', 'book', 'of', 'the', 'adventures', 'of']
```


## Remove all the stopwords


```python
# download the package
nltk.download("stopwords")

// OUTPUT
    [nltk_data] Downloading package stopwords to
    [nltk_data]     C:\Users\milind\AppData\Roaming\nltk_data...
    [nltk_data]   Package stopwords is already up-to-date!
    True
```



```python
# remove stop words
sw=nltk.corpus.stopwords.words('english')
sw[:5]

// OUTPUT
    ['i', 'me', 'my', 'myself', 'we']
```


## Get a list of words without stop words


```python
# get the list without stop words
words_ne=[]
for word in words:
    if word not in sw:
        words_ne.append(word)

words_ne[:5]

// OUTPUT
    ['project', 'gutenberg', 'book', 'adventures', 'sherlock']
```


## Plot the word frequency


```python
sns.set_style('darkgrid')
nlp_words=nltk.FreqDist(words_ne)
nlp_words.plot(20);
```

![output_19_0.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631428584000/Mga5-7uKG.png)

## Summary

Word frequency identification is one of the initial step in spam detection classifier and a basic understanding of NLTK and beautifulsoup is essential.

## You might also like
- [Mushroom dataset analysis and classification in python](https://milindsoorya.site/blog/mushroom-dataset-analysis-and-classification-python)
 
