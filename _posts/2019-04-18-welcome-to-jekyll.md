---
title: "The word 'had' in Finnegans Wake"
date: 2019-04-18T15:34:30-04:00
categories:
  - Literature and Linguistics
tags:
  - Jekyll
  - update
---

The [Finnegans Wake](https://en.wikipedia.org/wiki/Finnegans_Wake) (FW) can be seen as an attempt to understand the history of humanity through the lens of Viconian philosophy. There are some clear cyclic patterns in the book: it is widely known that the book has "[Doublends Jined](http://www.finnegansweb.com/wiki/index.php/Doublends_Jined)".

However, Finnegans Wake wouldn't be a book "[that people should spend a lifetime figuring out](https://theamericanscholar.org/a-slow-devouring/)" unless there were cyclic patterns not so trivial to notice. One pattern, initially deciphered by [Marshall McLuhan](https://en.wikipedia.org/wiki/Marshall_McLuhan), is the thunder:

> Joyce's Wake is claimed to be a gigantic cryptogram which reveals a cyclic pattern for the whole history of man through its Ten Thunders. Each "thunder" below is a 100-character portmanteau of other words to create a statement he likens to an effect that each technology has on the society into which it is introduced. In order to glean the most understanding out of each, the reader must break the portmanteau into separate words (and many of these are themselves portmanteaus of words taken from multiple languages other than English) and speak them aloud for the spoken effect of each word. There is much dispute over what each portmanteau truly denotes.
> - [War and Peace in the Global Village](https://en.wikipedia.org/wiki/War_and_Peace_in_the_Global_Village), a book by Marshall McLuhan and Quentin Fiore

As far as I can tell, I'm the first to claim that Joyce intended the word **had** to mark a cyclical pattern. I'm well aware that [pareidolia](https://en.wikipedia.org/wiki/Pareidolia) and [apophenia](https://en.wikipedia.org/wiki/Apophenia) are very real risks while dealing with the Wake. However, I feel there is strong evidence to suggest that the word **had** wasn't being utilized by Joyce in a casual fashion.   

First of all, let's count all occurrences of the word had. 

```python
import requests
import spacy
from collections import Counter

# download the Wake
r = requests.get('https://archive.org/stream/finneganswake00joycuoft/finneganswake00joycuoft_djvu.txt')
text = r.content.decode()
content = text.split("\n")

# Remove lines without the "had" character sequence
content = [x for x in content if 'had' in x]

nlp = spacy.load("en_core_web_sm")
lines = [ nlp(x) for x in content]

# Count all occurences of "had", either as a word or as a subword unit
token_list = []
for item in lines:
    for token in item:
        if 'had' in token.text:
            token_list.append(token.text
frequency_dictionary = Counter(token_list)
```

|    | word           |   frequency |
|---:|:---------------|------------:|
|  2 | had            |         362 |
|  0 | Shady          |          14 |
|  1 | shadow         |          12 |
|  8 | shade          |           7 |
| 12 | shades         |           7 |
| 22 | shadows        |           5 |
|  6 | shady          |           2 |
| 29 | hadded         |           2 |
| 30 | shadowed       |           2 |
| 20 | Thaddeus       |           2 |
| 26 | marchadant     |           1 |
| 28 | hade           |           1 |
| 31 | shaddo         |           1 |
| 32 | limpshades     |           1 |
| 33 | Shadow         |           1 |
| 34 | bahad          |           1 |
| 35 | haddock        |           1 |
| 36 | shad           |           1 |
| 37 | showshadows    |           1 |
| 38 | pleashadure    |           1 |
| 39 | shadowstealers |           1 |
| 40 | nighadays      |           1 |
| 41 | retchad        |           1 |
| 42 | shadyside      |           1 |
| 43 | hadding        |           1 |
| 27 | comrhade       |           1 |
| 25 | whad           |           1 |
|  7 | shadowers      |           1 |
| 24 | Upanishadem    |           1 |
|  5 | treeshade      |           1 |
|  9 | writchad       |           1 |
| 10 | Whaddingtun    |           1 |
| 11 | Whad           |           1 |
|  4 | hadbeen        |           1 |
| 13 | chades         |           1 |
| 14 | hads           |           1 |
| 15 | Drughad        |           1 |
| 16 | shadda         |           1 |
| 17 | Shadows        |           1 |
| 18 | hadtobe        |           1 |
| 19 | rehad          |           1 |
| 21 | shader         |           1 |
|  3 | hadde          |           1 |
| 23 | Thady          |           1 |
| 44 | thadark        |           1 |
