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


```python
import requests
import spacy
r = requests.get('https://archive.org/stream/finneganswake00joycuoft/finneganswake00joycuoft_djvu.txt')
text = r.content.decode()
content = text.split("\n")
content = [x for x in content if 'had' in x]

import spacy
nlp = spacy.load("en_core_web_sm")
lines = [ nlp(x) for x in content]
```
