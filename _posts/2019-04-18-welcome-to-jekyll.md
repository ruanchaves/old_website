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

As far as I can tell, I'm the first person to claim that Joyce intended the word *had* to mark a cyclical pattern, maybe because no one else (other than Joyce himself) ever bothered to count how many times the word occurs in the book. I'm well aware that [pareidolia](https://en.wikipedia.org/wiki/Pareidolia) and [apophenia](https://en.wikipedia.org/wiki/Apophenia) are very real risks while dealing with the Wake. However, I feel there is strong evidence to suggest that the word *had* wasn't being utilized by Joyce in a casual fashion.   

First of all, let's count all occurrences of the word *had*. 

```python
import requests
import spacy
import pandas as pd
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

df = pd.DataFrame(
  [
    {'word': key, 'frequency': value} for key, value in dict(frequency_dictionary).items()
  ]
  ).sort_values('frequency', ascending=False).reset_index(drop=True)
print(df.to_markdown())
```

This code gives us the following table.

|    | word           |   frequency |
|---:|:---------------|------------:|
|  0 | **had**            |         362 |
|  1 | Shady          |          14 |
|  2 | shadow         |          12 |
|  3 | shade          |           7 |
|  4 | shades         |           7 |
|  5 | shadows        |           5 |
|  6 | shady          |           2 |
|  7 | hadded         |           2 |
|  8 | shadowed       |           2 |
|  9 | Thaddeus       |           2 |
| 10 | marchadant     |           1 |
| 11 | hade           |           1 |
| 12 | shaddo         |           1 |
| 13 | limpshades     |           1 |
| 14 | Shadow         |           1 |
| 15 | bahad          |           1 |
| 16 | haddock        |           1 |
| 17 | shad           |           1 |
| 18 | showshadows    |           1 |
| 19 | pleashadure    |           1 |
| 20 | shadowstealers |           1 |
| 21 | nighadays      |           1 |
| 22 | retchad        |           1 |
| 23 | shadyside      |           1 |
| 24 | hadding        |           1 |
| 25 | comrhade       |           1 |
| 26 | whad           |           1 |
| 27 | shadowers      |           1 |
| 28 | Upanishadem    |           1 |
| 29 | treeshade      |           1 |
| 30 | writchad       |           1 |
| 31 | Whaddingtun    |           1 |
| 32 | Whad           |           1 |
| 33 | **hadbeen**        |           1 |
| 34 | chades         |           1 |
| 35 | hads           |           1 |
| 36 | Drughad        |           1 |
| 37 | shadda         |           1 |
| 38 | Shadows        |           1 |
| 39 | **hadtobe**        |           1 |
| 40 | rehad          |           1 |
| 41 | shader         |           1 |
| 42 | hadde          |           1 |
| 43 | Thady          |           1 |
| 44 | thadark        |           1 |


Words such as *rehad* and *hadded* are either a made-up words or loanwords in disguise (*rehad* is the Estonian for *rake*).
*hads* and *hadde*, are Old English forms of the word *had*. Other words that feature *had* as a substring are not related to its meaning.

The word *had* appears 362 times, and if *hadbeen* and *hadtobe* are to be counted, then Joyce utilized the word *had* **364 times** in the Wake.  

This brings us to my main point: Joyce utilized the word *had* to mark the days in the Finnegans Wake. The entire Wake is a calendar of 364 days, where the sunrise of each day is indicated by an occurrence of the word *had*. 

This was intended as a tribute to Aleister Crowley. Crowley wrote a very favorable review of Ulysses called *The Genius of Mr. James Joyce*. The first line of his most famous book, Liber AL Vel Legis ( The Book of the Law ), starts with **"Had! The manifestation of Nuit"**. Had is the egyptian sun-god, the manifestation of the egyptian sky goddess Nuit. 

Crowley constantly referred to Enochian magic, and in the Book of Enoch we read about the [Enoch calendar](https://en.wikipedia.org/wiki/Enoch_calendar):

> 12. And the sun and the stars bring in all the years exactly, so that they do not advance or delay their position by a single day unto eternity; but complete the years with perfect justice in **364 days**.

The word *had* appeared for the first time as *“had passencore arrived”*. Notice that the syllable *“pa”* didn’t appear on the first draft version ( Rather it was *“Had not encore arrived”* ).

*pa* is a reference to Giambattista Vico. In his *La Scienza Nuova*, he states that "It seems likely that, when the first lightning bolts had awakened the wonder of humankind, Jupiter’s exclamations called forth the first human exclamation, **the syllable pa.**". So here we have a reference to Jupiter and Horus, both which rule over the sunrise, put together side by side.
