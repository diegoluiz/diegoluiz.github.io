---
layout: post
title:  "A Road to ML"
date:   2017-04-17 22:00:00 +0100
tags:
comments: true
---

So, as I said in the last post, I started reading and trying some Machine Learning (ML).
It's hard to write down when I started reading about it and became interested on that. But some good references that come to mind mind are a post in [WBW](http://waitbutwhy.com/2015/01/artificial-intelligence-revolution-1.html) (which the title of the post came from) and a talk about Products Recommendations with a friend.

So, last Friday I found a **fantastic** youtube channel about ML. You can find it [here](https://www.youtube.com/channel/UCWN3xxRkmTPmbKwht9FuE5A/).
So far, this is the best channel about ML I found. Some people might say he doesn't dive deep into some concepts, but that's not the concept of the channel. IMO he tries to show the applications of ML (and other things) and cites the concepts and how they work, leaving you to do your homework and dive deeper.

Currently he has a nano degree in [Udacity](https://www.udacity.com/course/deep-learning-nanodegree-foundation--nd101), and all videos are posted on [youtube](https://www.youtube.com/playlist?list=PL2-dafEMk2A7YdKv4XfKpfbTH5z6rEEj3) ! As I'm not doing the course there (but considering to do it), so I can't say anything about that.

<!--more-->
So, moving forward to what I did. Yesterday I posted about the mass fail to install cuda and cudnn, guess what, I reinstalled and the system and when I tried to install it again, **it failed**. I was a pain in the **** to make my laptop works again, but after a while playing with some nvidia drivers I managed to make it works without resintalling, therefore not testing my script haha
And what makes me angrier is that I could install it easily on Windows, and it's working perfectly!!!! But I refuse to boot on Windows to use it! (Although some packages don't work fine on Windows).

But I could install `Tensorflow` without GPU support, which is a shame, but it works for now. Installed `ipython notebook` and `jupyter notebook`.
And I started watching the `python playlist` from Siraj. I'm watching everything, but right now, I'm implementing only things I couldn't understand properly. When I finish the playlist I will run through it again and implement everything.

For the [first](https://www.youtube.com/watch?v=T5pRlIbr6gg&list=PL2-dafEMk2A6QKz1mrk1uIGfHkC1zZ6UU&index=1) and [second](https://www.youtube.com/watch?v=o_OZdbCzHUA&index=2&list=PL2-dafEMk2A6QKz1mrk1uIGfHkC1zZ6UU) video I didn't implement anything, however they are very interesting, builind a classification program using [sci-kit](http://scikit-learn.org/stable/) and a text analysis using [textblob](https://textblob.readthedocs.io/en/dev/) with [twitter client](https://github.com/tweepy/tweepy).

So I started struggling in the third video, which is considerably harder. He built a movie recommendation system using [lightfm](https://github.com/lyst/lightfm) a very known package for recommendation algorithms. The [movielens](https://grouplens.org/datasets/movielens/) dataset was used for it.
The concept is simple, you load all the users' rating history for movies, and based on that you recommend new movies to it. Sounds like netflix, doesn't it?

Going to lightfm's github you can find an example of how it works using movielens as well:

```python
from lightfm import LightFM
from lightfm.datasets import fetch_movielens
from lightfm.evaluation import precision_at_k

# Load the MovieLens 100k dataset. Only five
# star ratings are treated as positive.
data = fetch_movielens(min_rating=5.0)

# Instantiate and train the model
model = LightFM(loss='warp')
model.fit(data['train'], epochs=30, num_threads=2)

# Evaluate the trained model
test_precision = precision_at_k(model, data['test'], k=5).mean()
```

It looks like magic, doesn't it?
Well, I don't like magic :( So I wanted to understand first how it formats the `data` object. As it's open source, just go to the code [here](https://github.com/lyst/lightfm/blob/master/lightfm/datasets/movielens.py).



So the only thing I could actually do was related to this [video](https://www.youtube.com/watch?v=9gBC9R-msAk&list=PL2-dafEMk2A6QKz1mrk1uIGfHkC1zZ6UU&index=3). 
The beginning is pretty simple, just downloads a zip file and reads the data:

```python
def _read_raw_data(path):
    """
    Return the raw lines of the train and test files.
    """

    with zipfile.ZipFile(path) as datafile:
        return (datafile.read('ml-100k/ua.base').decode().split('\n'),
                datafile.read('ml-100k/ua.test').decode().split('\n'),
                datafile.read('ml-100k/u.item').decode(errors='ignore').split('\n'),
                datafile.read('ml-100k/u.genre').decode(errors='ignore').split('\n'))

zip_path = _common.get_data(data_home,
                            'http://files.grouplens.org/datasets/movielens/ml-100k.zip',
                            'movielens100k',
                            'movielens.zip',
                            download_if_missing)

(train_raw, test_raw,
    item_metadata_raw, genres_raw) = _read_raw_data(zip_path)
```

and then a simple function is called

```python
def _get_dimensions(train_data, test_data):

    uids = set()
    iids = set()

    for uid, iid, _, _ in itertools.chain(train_data,
                                          test_data):
        uids.add(uid)
        iids.add(iid)

    rows = max(uids) + 1
    cols = max(iids) + 1

    return rows, cols

num_users, num_items = _get_dimensions(_parse(train_raw),
                                        _parse(test_raw))
```

so, the first question appears, what the hell is `itertools.chain` ?
Reading its [documentation](https://docs.python.org/3/library/itertools.html#itertools.chain), it's simple, it receives a list of iterables items and iterates through all of them.
So, let's say you have to arrays, and you want to display all values of them, you can use `itertools.chain` for it.

```python
import itertools

a = [1,2,3]
b = [3,4,5]

# with fors
for i in a:
    print(i)
for i in b:
    print(i)

# with itertools.chain
for i in itertools.chain(a,b):
    print(i)
```

It might look like a waste of key press, but it helps a lot when you want to run the same logic for some arrays, and you can send more than 2 as it receives `*iterables`.
Moving on, you find:

```python
def _build_interaction_matrix(rows, cols, data, min_rating):

    mat = sp.lil_matrix((rows, cols), dtype=np.int32)

    for uid, iid, rating, _ in data:
        if rating >= min_rating:
            mat[uid, iid] = rating

    return mat.tocoo()

train = _build_interaction_matrix(num_users,
                                    num_items,
                                    _parse(train_raw),
                                    min_rating)
```

And another what the hell...
So I started looking for what lil_matrix was. I saw it's part of [scipy](https://www.scipy.org/) package and it builds a matrix. So the code is simply getting the array, filtering the rating, and adding to matrix, and creating it when it invokes `.tocoo()`.

You can see the difference running this code

```python
import scipy.sparse as sp
import numpy as np
a=[[1,1,5],[1,2,3],[2,1,3],[2,2,10],[3,1,1],[3,2,2]]
m = sp.lil_matrix((3,2), dtype=np.int32)

for id1,id2,rating in a:
    m[id1-1,id2-1]=rating # -1 because it's 0 based

print(a)
print(m.tocoo())
```

And why a sparse matrix? Simple answer, the training algorithms expects it. I'm sure I saw somewhere (that I can't find now) that working with sparse matrix is considerably faster than array because of the operations you can run over it.


Some methods are missing, but I stopped for today, on Wednesday I might go back to it, until I fully understand this script.
And why this class only and not the shiny [lightfm.py](https://github.com/lyst/lightfm/blob/master/lightfm/lightfm.py) one? Well, that class loads and formats the dataset, so it's mandatory for anything you want to do with lightfm. Apart from that, lightfm is the pure implementation of algorithms, which might be over my technical level for now, but maybe one day I might come back for it!

Apart from that I read about [loss functions](https://en.wikipedia.org/wiki/Loss_function), especially the [WARP](http://www.hongliangjie.com/2012/08/24/weighted-approximately-ranked-pairwise-loss-warp/) one, but I couldn't understand much :(
The idea is to try to understand the big picture, and not the statistical algorithm behind it. Soon I will return to it again.

And today I took some time to help my girlfriend to move her blog to github pages. The url is [https://renatakc.github.io/](https://renatakc.github.io/).
It's underconstruction, but soon she will put her studies about statistics, data analysis, ML and so on there. So it might be good resource :)


Well, that's all for today. :)