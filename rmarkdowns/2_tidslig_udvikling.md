# Timedispersion analysis with twitterdata

    library(rtweet)
    library(tidyverse)

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.3     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.5     ✓ dplyr   1.0.3
    ## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
    ## ✓ readr   1.4.0     ✓ forcats 0.5.0

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter()  masks stats::filter()
    ## x purrr::flatten() masks rtweet::flatten()
    ## x dplyr::lag()     masks stats::lag()

    library(lubridate)

    ## 
    ## Attaching package: 'lubridate'

    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

# Acuirreing your data

In the following example, you will learn how to process and visualize
data acquired from Twitter.com using the Essential access Twitter API.
In this example, you will create your dataframe by making a free-text
search on the term “sesamestreet” using the `search_tweets()`-function
from the “rtweet”-package.

    sesamestreet_data <- search_tweets(q = "sesamestreet", n = 18000)

# Timely dispersion of hashtags and fretext

In the following we start of with some data processing before moving on
to the actual visualisation. The question we are asking the data here is
a two-piece one.

-   First of we want to know the dispersion of the tweets over time.
-   Second we want to know how many of these contain a the hashtag
    “\#sesamestreet”.

Especially the last question needs some data wranglig before it is
possible to answer it. The process here is to create a new column which
has the value “TRUE” if the tweet contains the hashtag and FALSE if not.
This is done with the `mutate()`-function, which creates a new column
called “has\_sesame\_ht”. To put the TRUE/FALSE-values in this column we
use the `str_detect()`-function. This function is told that it is
detecting on the column “text”, which contains the tweet. Next it is
told what it is detecting. Here we use the `regex()`-function within
`str_detect()` and by doing that we can specify that we are interested
in all variants of the hashtag (eg \#SesameStreet, \#Sesamestreet,
\#sesamestreet, \#SESAMESTREET, etc.). This is achieved by setting
“ignore\_case = TRUE”.

The next step is another `mutate()`-function, where we create a new
column “date”. This column will contain just the date of the tweets
instead of the entire timestamp from Twitter that not only contains the
date, but also the hour, minute and second of the Tweet. This is
obtained with the `date()`-function from the “lubridate”-packages, which
is told that it should extract the date from the “created\_at”-column.  
Lastly we use the `count`-function from the “tidyverse”-package to count
TRUE/FALSE-values in the “has\_same\_ht”-column per day in the data set.

    sesamestreet_data %>% 
      mutate(has_sesame_ht = str_detect(text, regex("#sesamestreet", ignore_case = TRUE))) %>% 
      mutate(date = date(created_at)) %>% 
      count(date, has_sesame_ht)

    ## # A tibble: 20 x 3
    ##    date       has_sesame_ht     n
    ##    <date>     <lgl>         <int>
    ##  1 2021-12-04 FALSE            98
    ##  2 2021-12-04 TRUE             17
    ##  3 2021-12-05 FALSE           164
    ##  4 2021-12-05 TRUE             53
    ##  5 2021-12-06 FALSE           372
    ##  6 2021-12-06 TRUE             62
    ##  7 2021-12-07 FALSE           264
    ##  8 2021-12-07 TRUE             85
    ##  9 2021-12-08 FALSE           186
    ## 10 2021-12-08 TRUE             92
    ## 11 2021-12-09 FALSE           150
    ## 12 2021-12-09 TRUE             54
    ## 13 2021-12-10 FALSE           142
    ## 14 2021-12-10 TRUE             59
    ## 15 2021-12-11 FALSE           196
    ## 16 2021-12-11 TRUE             41
    ## 17 2021-12-12 FALSE           255
    ## 18 2021-12-12 TRUE             44
    ## 19 2021-12-13 FALSE            67
    ## 20 2021-12-13 TRUE             37

This is the result we now want to visualise. In the code below we have
appended the code for the visualisation to the four lines of code above
that transforms the data to our needs.  
To pick up where we left in the previous code chunk we continue with the
`ggplot()`-function, which is the graphics package of the “tidyverse”.
This function is told that it should put date on the x-axis and the
counted number of TRUE/FALSE-values on the y-axis. The next line of the
creation of the visualisation is `geom_line()`,where we specify
linetype=has\_sesame\_ht, thus creating creating two lines for; one for
TRUE and one for FALSE.

The lines of code following the `geom_line()` argument tweaks the
aesthetics of the visualisation. `scale_linetype()`tells R, what the
lines should be labeled as. `scale_x_date()` and `scale_y_continuous()`
changes the looks of the x- and y-axis respectively. At last, the
`labs()` and `guides()` arguments are used to create descriptive text on
the visualisation.

Remember to inspect your data in your R environment and adjust these
labels to fit your acquired data.

    sesamestreet_data%>% 
      mutate(has_sesame_ht = str_detect(text, regex("#sesamestreet", ignore_case = TRUE))) %>% 
      mutate(date = date(created_at)) %>% 
      count(date, has_sesame_ht) %>% 
      ggplot(aes(date, n)) +
      geom_line(aes(linetype=has_sesame_ht)) +
      scale_linetype(labels = c("No #sesamestreet", "#sesamestreet")) +
      scale_x_date(date_breaks = "1 day", date_labels = "%b %d") +
      scale_y_continuous(breaks = seq(0, 30000, by = 5000)) +
      theme(axis.text.x=element_text(angle=40, hjust=1)) +
      labs(title = "Figure 1 - Daily tweets dispersed on whether or not they\ncontain #sesamestreet", y="Number of Tweets", x="Day", subtitle = "Period: 4 december 2021 - 13 december 2021", caption = "Total number of tweets: 2.435") +
      guides(linetype = guide_legend(title = "Whether or not the\ntweet contains \n#sesamestreet"))

![](2_tidslig_udvikling_files/figure-markdown_strict/unnamed-chunk-4-1.png)
You should now have a graph dipicting the timely dispersion of tweets in
your dataset.

In the next codechunk you can save the plot as a png-file for further
use:

    ggsave("20211213_sesamestreet_2021_tweet_timeline_dispersed_on_hastag_or_not.png", width = 8, height = 5, dpi = 800)
