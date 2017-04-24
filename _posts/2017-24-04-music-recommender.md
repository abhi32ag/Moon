---
layout: post
title: "Music Recommender using Last.FM data"
date: 2017-04-24
excerpt: "Project to utilize artist-plays to recommend different artists to a user"
url: music-recommender/
tags: [IPython, Pandas, Python, NumPy]
project: true
comments: true
---



This project works on the Last.FM dataset that was provided by Oscar Celma. It consists of user-artist-plays tuples that was collected from the LastFM API. This small project dweleved from my interest in music and also a need to understand item-based collaborative filtering as part of my independent study on recommender systems The project involved the use of `NumpPy`, `Pandas`, `Scipy`, `Sci-kit Learn`, `fuzzywuzzy`. It introduced me to a variety of new modules and methods. Let's take a look at a few code snippets to understand what this project is all about. 

So, firstly we load the user-data file which contains users, artists that each user listens to and the number of plays that user has for that artist. We also load the user-profile data that gives information like the users country of origin, age 

{% highlight python %}
artist_plays = (user_data.groupby(by = ['artist']).sum().
                reset_index().
                rename(columns = {'plays': 'total_artist_plays'})
                       [['artist','total_artist_plays']])
{% endhighlight %}

Here we want to calculate the total plays an artist gets from all users combined in order to get an understanding of the really popular artists. This is needed as we next create a threshold of artists that we would consider for our recommendations that lie above this threshold. We take the top 3 % artists and set the threshold to 40000

{% highlight python %}
popularity_threshold = 40000
user_data_popular_artists = user_data_with_artist_plays.query('total_artist_plays >= @popularity_threshold')
{% endhighlight %}

After we've obtained our subsetted user-artist data, we restrict our data to just users that are from the United States to reduce the complexity of the project and obtain a more narrow result. Once we have this wide matrix created we implement a nearest neighbors model in order to obtain artists closer to the artist of concern by utilizing the artist-plays vector. We calculate the distance between each artist using `Cosine` distance. Once we have these distances calculated, we pick the closest 10 to make our recommendations. Here's a snippet of how sklearn is used. 

{% highlight python %}
from sklearn.neighbors import NearestNeighbors

model_knn = NearestNeighbors(metric = 'cosine', algorithm = 'brute')
model_knn.fit(wide_artist_data_sparse)

#Making recommendations 
query_index = np.random.choice(wide_artist_data.shape[0])
print(query_index)
distances, indices = model_knn.kneighbors(wide_artist_data.iloc[query_index,:].reshape(1,-1),
                                         n_neighbors = 6)
for i in range(0, len(distances.flatten())):
    if i == 0:
        print('Recommendations for {0}: \n'.format(wide_artist_data.index[query_index]))
    else:
        print('{0}: {1}, with distance: {2}'.format(i,wide_artist_data.index[indices.flatten()[i]]
                                                   , distances.flatten()[i]))
{% endhighlight %}

Here is just a sample of the type of recommendations we observe if snoop dogg is selected as a users selected artist into consideration 

{ % highlight %}
Possible matches: [('snoop dogg', 100)]

Recommendations for snoop dogg:

1: dr. dre, with distance of 0.676800315101:
2: 2pac, with distance of 0.703038818823:
3: the game, with distance of 0.70661498309:
4: 50 cent, with distance of 0.729609338712:
5: ludacris, with distance of 0.73608257713:
6: notorious b.i.g., with distance of 0.736465827754:
7: jay-z, with distance of 0.739437837488:
8: nas, with distance of 0.759249192896:
9: ice cube, with distance of 0.761719788076:
10: t.i., with distance of 0.763270831769:

{% endhighlight %}

Credits to <a href = https://beckernick.github.io/datascience/> Nick Becker </a> whose blog post on music recommenders inspired this project 
