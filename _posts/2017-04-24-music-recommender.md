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

<figure>
	<a href="https://github.com/abhi32ag/Pronto-Cycle"><img src="/assets/img/pron1.png"></a>
	
</figure>

In the above figure we can see the plot between the count of trips made by date. It is segregated by the type of membership that a user has viz Annual Membership or Day-Pass. We observe how the trip made by Day-Pass user's changes in line with the seasons. While the usage of Annual members is more frequent and regular. Let us go a little more granular and look at an average of the trips by week. 

{% highlight python %}
by_weekday.columns.name = None
fig, ax = plt.subplots(1, 2, figsize =(16,6), sharey = True)
by_weekday.loc[2014].plot(ax = ax[0],title = 'Average Use by Day of Week(2014)');
by_weekday.loc[2015].plot(ax = ax[1],title = 'Average Use by Day of Week(2015)');
for axi in ax:
    axi.set_xticklabels(['Mon', 'Tues', 'Wed', 'Thurs', 'Fri', 'Sat', 'Sun'])
by_weekday = by_date.groupby([by_date.index.year,
                              by_date.index.dayofweek]).mean()
{% endhighlight %}
<figure>
	<a href="https://github.com/abhi32ag/Pronto-Cycle"><img src="/assets/img/pron2.png"></a>
	
</figure>