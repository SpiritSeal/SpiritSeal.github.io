---
title: "Plotting Spotify Listening Data"
permalink: "/vst-malware-musician/"
layout: post
---


This post was the output of an applied Stats project I was assigned during my High School AP Statistics where we were tasked with finding some data in the wild and using software to extract insights from the raw numbers. In this post, I'll go over the steps I took to extract my Spotify listening data and create meaningful visualizations from it in R.


## Personal Spotify Trends by Saketh Reddy

For my data, I decided to analyse some of my listening habits on Spotify based on the songs that have accumulated in my (personal) “Liked Songs” playlist over the past five years. In order to obtain the data from my playlist, I have used Exportify [[https://exportify.net/](https://exportify.net/)] to extract the metadata from my songs in this playlist.




![alt_text](/assets/images/2021-09-09-Plotting-Spotify-Listening-Data/image1.png "image_tooltip")


The data downloaded as a .csv file (available as “liked_songs.csv” in this zip file).

Taking a look at it through Notepad, we are greeted with this mess:




![alt_text](/assets/images/2021-09-09-Plotting-Spotify-Listening-Data/image2.png "image_tooltip")


So, woah, that’s a lot of information.

Let's open the file in Excel to see what it looks like formatted.





![alt_text](/assets/images/2021-09-09-Plotting-Spotify-Listening-Data/image3.png "image_tooltip")


That’s a lot better.

Some numerical data from this metadata that I found interesting was the Track Duration and the Popularity Index. 





![alt_text](/assets/images/2021-09-09-Plotting-Spotify-Listening-Data/image4.png "image_tooltip")


Let’s analyze them!

First, I imported the data into R using 


```
songs = read.csv(file="liked_songs.csv")
```


Loading my csv file into R




![alt_text](/assets/images/2021-09-09-Plotting-Spotify-Listening-Data/image5.png "image_tooltip")


In order to determine how I should interact and access the individual data columns of my dataset, I used

	`typeof(songs)`

to reveal that I was dealing with a ``list`` of ``lists``

So, I could conveniently access each sublist of my overall dataset as it’s own list by using something formatted as

	`songs[["PropertyToUse"]]`



First, I used a boxplot to visualize the Popularity data of the songs in my playlist.

To clarify the meaning of this data point, for every song uploaded to their platform, Spotify assigns a numerical “Popularity” value to the song based on the number and recency of the piece’s listens. 

To be more accurate, here is an excerpt from the Spotify API[^1] that describes this value:


<table>
  <tr>
   <td><code>popularity</code>
<p>
The popularity of the artist. The value will be between 0 and 100, with 100 being the most popular. The artist’s popularity is calculated from the popularity of all the artist’s tracks.
   </td>
   <td>Integer
   </td>
  </tr>
</table>


Since I am using a list, the section of my data relevant to “popularity” can be accessed with 


```
songs[["Popularity"]]
```


Pushing this list into a boxplot using


```
boxplot(songs[["Popularity"]], main = "Popularity of Saketh's Liked Songs on Spotify", ylab = "Popularity Index [1-100]", 
        ylim= c(0, 100), yaxs = "i",
        frame.plot = FALSE)
```


(Since the range of values is strictly within 1-100 for this data set, I set the ylim of this data set to be between 0-100)

I get something that looks like:




![alt_text](/assets/images/2021-09-09-Plotting-Spotify-Listening-Data/image6.png "image_tooltip")


_PopularBox.png_

From this box plot, I can see that the distribution of the popularity of the songs in my Spotify playlist are skewed right (likely due to the fact that I tend to a combination of older artists and indie artists, with relatively less time spent listening to mainstream songs). There are no outliers. The median of the distribution is approximately 40/100 on the Popularity Index. The middle 50% of my songs were between 1 and approximately 60 inclusive on the Popularity index, creating an IQR of approximately 60 index points.



After analyzing the popularity data of these songs, I decided to use a histogram to analyze the trends of the durations of the songs in this playlist.

I could access the list of my duration data by using


```
songs[["Track.Duration..ms."]]
```


However, this duration data was in a massive number of Milliseconds (think n*10^5), which are not easily cognitively processed by most humans. So, a few unit conversions later…


```
timeInSeconds = songs[["Track.Duration..ms."]]/1000
timeInMinutes = timeInSeconds/60
```


… I had the duration data mapped into minutes.

I proceeded to map this cleaned list into a histogram using:


```
hist(timeInMinutes, main = "Duration of Saketh's Liked Songs on Spotify", xlab = "Length in Minutes")
```


Giving me something that looked like:




![alt_text](/assets/images/2021-09-09-Plotting-Spotify-Listening-Data/image7.png "image_tooltip")


_durationBar.png_

From this histogram, I can see that the distribution of the length of the songs in my Spotify playlist Unimodal and roughly symmetric (with the ~+50 more songs than expected in the 0-1 minute range likely being the entirety of Toby Fox’s soundtracks from the game “Undertale” that I likely added back when I was in 8th grade and have since forgotten about until I ran this plot). There are no gaps in this data set. The center of the distribution is approximately 3.5 minutes and approximately two thirds or the songs fall between 2 and 5 minutes.



Full Code:


```
songs = read.csv(file="liked_songs.csv")
typeof(songs)

# Artist API popularity endpoint:
#   The popularity of the track. The value will be between 0 and 100, with 100 being the most popular.
# The popularity of a track is a value between 0 and 100, with 100 being the most popular. The popularity is calculated by algorithm and is based, in the most part, on the total number of plays the track has had and how recent those plays are.
# Generally speaking, songs that are being played a lot now will have a higher popularity than songs that were played a lot in the past. Duplicate tracks (e.g. the same track from a single and an album) are rated independently. Artist and album popularity is derived mathematically from track popularity. Note that the popularity value may lag actual popularity by a few days: the value is not updated in real time.
songs[["Popularity"]]
# hist(songs[["Popularity"]], main = "Popularity of Saketh's Liked Songs on Spotify", xlab = "Popularity Index [1-100]")
boxplot(songs[["Popularity"]], main = "Popularity of Saketh's Liked Songs on Spotify", ylab = "Popularity Index [1-100]", 
        ylim= c(0, 100), yaxs = "i",
        frame.plot = FALSE)

# Duration of songs in Milliseconds
songs[["Track.Duration..ms."]]
timeInSeconds = songs[["Track.Duration..ms."]]/1000
timeInMinutes = timeInSeconds/60
hist(timeInMinutes, main = "Duration of Saketh's Liked Songs on Spotify", xlab = "Length in Minutes")
# boxplot(timeInMinutes, main = "Duration of Saketh's Liked Songs on Spotify", ylab = "Length in Minutes", frame.plot = FALSE)


```


Data gathered and Code written in ~1 hour by a Musitech Nerd

Writeup itself + statistical analysis took ~2 hours (Formatting = timesink)

Sincerely,

Saketh Reddy


<!-- Footnotes themselves at the bottom. -->
## Notes

[^1]:
     [[https://developer.spotify.com/documentation/web-api/reference/#endpoint-get-an-artist](https://developer.spotify.com/documentation/web-api/reference/#endpoint-get-an-artist), scroll down to “ArtistObject”]
