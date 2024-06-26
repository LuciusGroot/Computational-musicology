# Computational-musicology
computational musicology
Dear Reader,
I was having problems, that is why i added here the whole code. It would be amazing if you open this in a Rstudios and knit it, so that you can see what I did. Copy past from line 6.
Kind Regards, Lucius Groot
---
title: "portfolio"
author: "Lucius Groot"
date: "2024-02-21"
output: 
  flexdashboard::flex_dashboard:
    storyboard: true
    theme: cosmo
    

---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)


```
### Introduction

Dear Reader,

For this project, I am interested in how Spotify will react to a sudden change in my music taste. I do not listen to the same songs as I used to listen between the ages of 2 till 12. I wonder how Spotify will react, if I make a playlist with some of the songs I used to listen.

  I did not have access to Spotify between the age of 2 till 12, but I did have access to CD's and DVD's (and I still have them). That is why I made a list for this project with the songs I used to listen as a child. Spotify does not know that I listen to (most of) these songs.
  
  Since 2017 did I start listening 'Musical Theatre songs' on Spotify. I really like those, but I do not like and listen to Disney songs (with the only exception of songs written by Lin-Manuel Miranda). I want to know which songs Spotify will recommend me.I would also like to know which code I can use for this, but I will hopefully learn that during this course. I can also see how my music taste has changed and compare it, which I find interesting to see.

There is one song in my playlist which has multiple versions of it. That is the 'Weekend Whip'. I will use that song for the homework assignment of week 8. I do not know whether I will keep that in my portfolio (it is in my opinion not something which add extra information to my analysis).

Kind regards, Lucius Groot

```{r}

## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.4.4     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
library(tidyverse)

library(spotifyr)

```
### My lists

I need the lists, which are 2 at this moment. 
The first lists consist of music that I used to listen between the age 2 till 12. All of these are stored at home in CD's or are DVD's showtunes intro.
The second lists is my weekly discovery, which is based on my current taste in music (mostly musical theatre songs and jazz/fusion/pop/rock/country etc, too much to sum up).
I am using short cuts for both the lists, which I also will make in the following chunck

*I have a problem. dw0 did update and I did not know that. This means that it is actually week 1, instead of 0. I will make a playlist of my weekly discovery, so that it will not change again.*


***

```{r}
      #Uploading "Music of Lucius' Childhood" to RStudios via URL; this lists contains the songs I have at home in CD form and showtunes          from TV shows.
MoLC <- get_playlist_audio_features("", "7E5lKSGKAhGwQZaftQViGz") 
        
      #uploading "Discover Weekly on 14 February 2024 of Lucius' Spotify" via URL. 
dw0 <- get_playlist_audio_features("", "37i9dQZEVXcIgiZXlwTluc") 

#shortcut for "Music of Lucius' Childhood" and "Discover Weekly on 14 February 2024 of Lucius' Spotify"
lists <-
  bind_rows(
    MoLC |> mutate(category = "Music of Lucius' Childhood"),
    dw0 |> mutate(category = "discovery weekly week 0 of Lucius")
  )
```
### Summary of the lists

The summary I let Rstudio made of both the lists:
I am interested how fast discover weekly will be adapted to the results of my childhood music
***

```{r}
#summary of "Discover Weekly on 14 February 2024 of Lucius' Spotify"
dw0 |>
  summarise(
    mean_speechiness = mean(speechiness),
    mean_acousticness = mean(acousticness),
    mean_liveness = mean(liveness),
    sd_speechiness = sd(speechiness),
    sd_acousticness = sd(acousticness),
    sd_liveness = sd(liveness),
    median_speechiness = median(speechiness),
    median_acousticness = median(acousticness),
    median_liveness = median(liveness),
    mad_speechiness = mad(speechiness),
    mad_acousticness = mad(acousticness),
    mad_liveness = mad(liveness)
  ) 

#summary of  "Music of Lucius' Childhood"
MoLC |>
  summarise(
    mean_speechiness = mean(speechiness),
    mean_acousticness = mean(acousticness),
    mean_liveness = mean(liveness),
    sd_speechiness = sd(speechiness),
    sd_acousticness = sd(acousticness),
    sd_liveness = sd(liveness),
    median_speechiness = median(speechiness),
    median_acousticness = median(acousticness),
    median_liveness = median(liveness),
    mad_speechiness = mad(speechiness),
    mad_acousticness = mad(acousticness),
    mad_liveness = mad(liveness)
  )

#I do not know how I can facet this one unfortunately
```

### Valence/Energy/Mode

I want to know of both the lists:
Valence versus energy + the mode


***
```{r}
  #the plots combined
ggplot(lists, aes(x = valence, y = energy, color = mode )) +
  geom_point(size = 1) +
  geom_segment(aes(xend = valence, yend = energy), size = 0.1) + 
  facet_wrap(~category) + 
   ggtitle("plot of my listss, valence versus energy combined with mode")

  #the plots individual: dw0
ggplot(dw0, aes(valence, energy)) +
  geom_point(aes(colour= mode)) +
  scale_y_log10() +
   ggtitle("plot of discovery weekly week 0 of Lucius, valence versus energy combined with mode")

  #the plots individual: dw0
ggplot(MoLC, aes(valence, energy)) +
  geom_point(aes(colour= mode)) +
  scale_y_log10() +
   ggtitle("plot of Music of Lucius Childhood, valence versus energy combined with mode")



```

### Valence/Energy/Mode in a smoothcurve


I want to know of both the lists:
Valence versus energy + the mode, but with a smooth curve, so I can see an average


***
```{r}
  #the plots combined
lists |> ggplot(aes(x = valence, y = energy)) + geom_point() + geom_smooth() + 
  facet_wrap(~category) + 
  ggtitle("Smoothcurve of my listss, valence versus energy combined with mode")
  
  #the plots individual: dw0 
dw0 |> ggplot(aes(x = valence, y = energy)) + geom_point() + geom_smooth() +
    ggtitle("Smoothcurve of dw0, valence versus energy combined with mode")

  #the plots individual: MoLC
MoLC |> ggplot(aes(x = valence, y = energy)) + geom_point() + geom_smooth() +
    ggtitle("Smoothcurve of Music of Lucius Childhood, valence versus energy combined with mode")

```

### The code of the homework week 7


I took some inspiration of the homework for my longest code ever


***
```{r}
#making 2 plots in one plot, i want to compare my weekly discovery with computational musicology playlist
lists |>                    #I started with the 2 playlists
  mutate(
    mode = ifelse(mode == 0, "Minor", "Major")
  ) |>
  ggplot(                     #This is the code I used for making the plot with ggplot2, i took inspiration from the assignment during the class
    aes(
      x = valence,
      y = energy,
      size = loudness,
      colour = mode
    )
  ) +
  geom_point() +              #first I tried to make a scatterplot
  geom_rug(linewidth = 0.02) + #deciding that the border is 0.02
  geom_text(                  #valence versus energy labels
    aes(
      x = valence,
      y = energy,
      label = label
    ),
    data = 
      tibble(
        label = c("", "  "),
        category = c("Music of Lucius' Childhood"),
        valence = c(0.104, 0.0339),
        energy = c(0.992, 0.00166)
      ),
    colour = "black",         #colour of the plot is black
    size = 4,                 #i want the size of the plot to be 4
    hjust = "left",           #i was making left side
    vjust = "center",         #i was making right side
    nudge_x = 0.02            #i wanted to place the label slightly to the right
  ) +
  facet_wrap(~ category) +    #i decided that i wanted to use both my playlist in two plots in one, again
  scale_x_continuous(         #i decided to fine-tune the x axis
    limits = c(0, 1),
    breaks = c(0, 0.50, 1),   #i used grid-lines for quadrants only.
    minor_breaks = NULL       #i wanted to remove 'minor' grid-lines.
  ) +
  scale_y_continuous(         #i fine-tuned the y axis in the same way.
    limits = c(0, 1),
    breaks = c(0, 0.50, 1),
    minor_breaks = NULL
  ) +
  scale_colour_brewer(        #the color brewer i used for making a was palette
    type = "qual",            #i have a qualitative set
    palette = "Paired"        #i named the palette to 'Paired'
  ) +
  scale_size_continuous(      #i decided to also fine-tune the sizes of each point
    trans = "exp",            #i used an exp transformation to emphasise loud
    guide = "none"            #i remove the legend for size, did not need it
  ) +
  theme_light() +             #making the theme light
  labs(
    x = "Valence",
    y = "Energy",
    colour = "Mode"
  )

```

### Histogram of the lists


The histogram I made about both the lists:


***
```{r}
#histogram of discovery weekly week 0 and Lucius & Music of Lucius' Childhood together in one plot
lists |>
  ggplot(aes(x = energy)) +
  geom_histogram(binwidth = 0.1) +
  facet_wrap(~category) + ggtitle("discovery weekly week 0 and Lucius & Music of Lucius' Childhood")
```


### Homework week 8 analysis



I want to compare the 'Pirate Whip', 'Ghost Whip', 'Tournament Whip' and 'Remix Whip'. They are both covers of the 'Weekend Whip', but each one has another theme related to the name. 

*I have tried to install remotes::install_github('jaburgoyne/compmus') but it is not working and I do not know why and I find this very frustating. That is why put the code in a text, so that you can check the codes. *


***

##Tournament Whip
TW <-
  get_tidy_audio_analysis("3kOKpxm3mOTVlxBALBQCLC") |>
  select(segments) |>
  unnest(segments) |>
  select(start, duration, pitches)
##Ghost Whip
GW <-
  get_tidy_audio_analysis("507YfKPyJTWnmaR2IJeJkE") |>
  select(segments) |>
  unnest(segments) |>
  select(start, duration, pitches)
##Pirate Whip
PW <-
  get_tidy_audio_analysis("6YRc4fPvtLlTS6K082pmn9") |>
  select(segments) |>
  unnest(segments) |>
  select(start, duration, pitches)

##Remix Whip
RW <-
  get_tidy_audio_analysis("3kOKpxm3mOTVlxBALBQCLC") |>
  select(segments) |>
  unnest(segments) |>
  select(start, duration, pitches)




### Further questions
What do I want to know? 
Will all the next Discovery weekly lists become more as Music of Lucius's Childhood? 
Which kind of songs will spotify recommend?
Which codes can I use the best
Will spotify recommend disney or more music with Dutch as language instead of English or music animatie compagnie (work i used to listen during a job at a camping in the holidays)


### Conclusion and discussion

While reloading the dataset, I saw that there has been a sudden change in the results of my weekly discovery. The playlist of discovery weekly is updating every week. I did not know that the URL stays the same. This means that I lost my information about my last week's discovery weekly. I will be making a playlist of the songs, so I will not loose it again.

I also have had some problems with the assignment of week 8, I tried to download the packages, but I think that it did not work. If you know how I can fix it, please help me!!!
