# Part 1



## 1. Word count

Write a function that counts words in text and returns a dictionary which maps the word to the count number (int) of that word

* It should convert words to lower case
* Words should not have newlines
* Words should not contain separation characters like ", . ? !"

### Suggested solutions:

First we need to split the text into a list of words that we can loop over
e.g.

~~~
>>> text = 'She loves me, yeah yeah yeah!'
>>> text.split()
['She', 'loves', 'me,', 'yeah', 'yeah', 'yeah!']
>>>
~~~

Within the loop we would have to remove non-letter characters and convert to
lower case, e.g. with `strip` and `lower` string methods

~~~
>>> 'She'.lower()
'she'
>>> 'yeah!'.strip('!')
'yeah'
>>>
~~~

~~~
>>> for w in text.split():
...     word = w.strip(',').strip('!').lower()
>>>
~~~

and finally put those words in a dictionary. 

What we first need then is to start out with an empty dictionary
~~~
>>> d = {}
>>>
~~~
and within the loop there are a couple of approaches to fill the dictionary
with values. A first valid attempt
is to check if a word is already in the dictionary and then increase the value
by 1, otherwise if it is absent insert the new word with value 1

~~~
>>> if word in d:
...    d[word] += 1
... else:
...    d[word] = 1
>>>
~~~

an alternative is to use the `get` method of a dictionary. That returns a
default value for a key if it does not exist, otherwise. So we could have get
return a 0 for words that are not yet in the dictionary 


~~~
>>> d = {}
>>> for word in text.split():
...     d[word] = d.get(word, 0) + 1
>>>
~~~

A third way is the collections module of the standard library
which contains many useful tools. `defaultdict` is a class which is like a
dictionary expects that it creates an entry when you reference something that
is not there

~~~
>>> import collections
>>> d = collections.defaultdict(int)
>>> d['new key']
0
>>> d == {'new key': 0}
True
>>>
~~~

A complete solution with the second version is

~~~
>>> def word_count(text):
...     """
...     Calculates words in text, return type: dict
...     
...     >>> word_count('bye bye')
...     {'bye': 2}
...     >>> word_count('She loves me yeah yeah yeah')
...     {'she': 1, 'loves': 1, 'me': 1, 'yeah': 3}
...     """
...     d = {}
...     for w in text.split():
...         word = w.strip(',').strip('!').strip('?').lower()
...         d[word] = d.get(word, 0) + 1
...     return d

~~~

Satisfying

~~~
>>> word_count('bye bye') == {'bye': 2}
True
>>> word_count('She loves me yeah yeah yeah')
{'she': 1, 'loves': 1, 'me': 1, 'yeah': 3}
>>> word_count("What?")
{'what': 1}
>>> word_count('''
... A word,
... if you please!
... ''')
{'a': 1, 'word': 1, 'if': 1, 'you': 1, 'please': 1}

~~~

## 2. Pirate language translator

 
Double any consonant and put an o in between

~~~
>>> to_pirate('hello') # doctest: +SKIP
hohelollolo
~~~

The output should preserve leading upper case of the characters in the beginning of the sentance. See the tests below

### Solution

We have to find a way of knowing when a character in a sequence is a consonant
or not. The most obvious way is a list all known consonants and check if a
character is member. That is perfectly valid. Here we get a shorter list of
vowels to start with, and one can check if it is not one of those, but a list
of non-consonants would have to be extended with spaces and other separation
characters


~~~
>>> VOWELS = 'eiyaou'
>>> def to_pirate(text):
...     pirate = ""
...     nonconsonants = VOWELS + ' .,!?'
...     for c in text:
...         pirate += c
...         if c.lower() not in nonconsonants:
...             pirate += 'o' + c.lower()
...     return pirate

~~~



~~~
>>> assert to_pirate('hello') == 'hohelollolo'
>>> assert to_pirate('Hello there') == 'Hohelollolo tothoherore'

~~~

Even for a small function like this it is good practise to split up the logic into separate
smaller functions if possible to get more readable code


~~~
>>> VOWELS = 'eiyaou'

>>> def to_pirate(text):
...     pirate = ""
...     for c in text:
...         pirate += c
...         if is_consonant(c):
...             pirate += 'o' + c.lower()
...     return pirate

>>> def is_consonant(char):
...     return char.lower() in 'bcdfghjklmnpqrstvwxz'

>>> to_pirate('Hello there')
'Hohelollolo tothoherore'

~~~



~~~
>>> assert to_pirate('hello') == 'hohelollolo'
>>> assert to_pirate('Hello there') == 'Hohelollolo tothoherore'

~~~

Note: in English the letter `y` is sometimes considered a consonant, but that
does not affect test tests here.
