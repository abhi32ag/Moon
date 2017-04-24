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

