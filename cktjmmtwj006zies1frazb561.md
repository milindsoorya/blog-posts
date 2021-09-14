## Generate cryptographically safe passwords with Python in 4 lines of code

## Introduction

The secrets module is used for generating cryptographically strong random numbers suitable for managing data such as passwords, account authentication, security tokens, and related secrets.

In particular, secrets should be used in preference to the default pseudo-random number generator in the random module, which is designed for modelling and simulation, not security or cryptography.

read more from the [docs](https://docs.python.org/3/library/secrets.html)

## Using `secrets.token_hex`

This is the easiest method to generate a random text of string, in hexadecimal. 

```python
>>> token_hex(16)  
'f9bf78b9a18ce6d46a0cd2b0b86df9da'
```
In a single line, you get a good enough password. But, yeah there is a but, if you want real hardcore, unbreakable password this is how to do it.

## Using `secrets.choice`
 
`secrets.choice` return a randomly-chosen element from a non-empty sequence. we just have to provide enough variation as the input string.

```python
import string
from secrets import choice

characters = string.ascii_letters + string.punctuation + string.digits
print(characters)
# abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~0123456789

password = "".join(choice(characters) for x in range(0, 32))
print(password)
# ONFe[)P>5^(jnJ\7ZgYw,|671BZ&!8R"
```

## Conclusion 

There you go a strong 32 character password 🎉🎉

Follow me on [Twitter](https://twitter.com/milindsoorya)

## Related articles

If you like this article I am pretty sure you would like the following ones too 😉

- [Build a Spam Classifier in python](https://milindsoorya.site/blog/build-a-spam-classifier-in-python)
- [Introduction to Word Frequencies in NLP](https://milindsoorya.site/blog/introduction-to-word-frequencies-in-nlp)

