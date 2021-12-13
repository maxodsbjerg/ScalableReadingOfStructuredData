    library(tidyverse)
    library(jsonlite)
    library(lubridate)
    library(rtweet)

# Loading dataset

The dataset collected from the
Twitter-API\[<https://developer.twitter.com/en/docs/twitter-api/getting-started/getting-access-to-the-twitter-api>\]
is stored in the json-format. You thus loaded into R with the function
`fromJSON` from the jsonlite-package. You name the dataset
“sesamestreet\_data” and you will be using this name, when referring to
the dataset in R.

    sesamestreet_data <- search_tweets("sesamestreet", n = 18000, include_rts = TRUE)

# Filtering for original tweets and observing the minimum favorite\_count value for the top 20

To examine original tweets only, we start by filtering away all the
tweets that are “retweets”.

Viewing the data *sesamestreet\_data* in the Global Environment, you see
that the column is\_retweet indicates whether a tweet is a retweet by
the values TRUE or FALSE. You are therefore able to use the
`filter`-function to retain all rows stating that the tweet is not a
retweet.

You then arrange the remaining tweets by the tweets’ favorite count
which is found in the favorite\_count column.

Both the `filter`-function and the `arrange`-function come from the
dplyr package which is part of tidyverse.

    sesamestreet_data %>% 
      filter(is_retweet == FALSE) %>% 
      arrange(desc(favorite_count))
(Output removed because of privacy reasons) 

As you can see in the Global Environment, your data *sesamestreet\_data*
has a total of 2435 observations. After running our chunk of code, you
can now read off our returned data.frame that there are 852
observations. Meaning 852 original tweets that are not marked as
retweets.

Looking at the column favorite\_count, you can now observe that the top
20 most liked tweets all have a count that is above 50. These numbers
are variables, that may change, if you choose to reproduce this example
by yourself. Be sure to check these numbers.

# Creating a new dataset of the top 20 most liked tweets (all accounts)

As you now know that the minimum favorite\_count value is 50, you add a
second `filter`-function to our previous code chunk which retains all
rows with a favorite\_count value over 50.

As you have now captured the top 20 most liked tweets, you can now
create a new dataset called
*sesamestreet\_data\_favorite\_count\_over\_18*.

    sesamestreet_data %>% 
      filter(is_retweet == FALSE) %>%
      filter(favorite_count > 50) %>% 
      arrange(desc(favorite_count)) -> sesamestreet_data_favorite_count_over_50

# Inspecting our new dafaframe (all)

To create a quick overview of our new dataset, you use the
`select`-function from the dplyr-package to isolate the variables you
wish to inspect. In this case, you wish to isolate the columns
favorite\_count, screen\_name, verified and text.

You then arrange them after their favorite\_count value by using the
`arrange`-function.

    sesamestreet_data_favorite_count_over_50 %>% 
      select(favorite_count, screen_name, verified, text) %>% 
      arrange(desc(favorite_count))
(Output removed because of privacy reasons) 

This code chunk returns a data.frame containing the previously stated
values. It is therefore much easier to inspect, than looking though the
whole dataset *sesamestreet\_data\_favorite\_count\_over\_50* in our
Global Environment.

# Exporting the new dataset as a JSON file

To export our new dataset out of our R environment and save it as a JSON
file, you use the `toJSON`-function from the jsonlite-package.

To make sure our data is stored as manageable and structured as
possible, all of our close reading data files are dubbed with the same
information:

1.  How many tweets/observations the data contains.
2.  What variable the data is arranged after.
3.  Whether the tweets are from all types of accounts or just the
    verified accounts.
4.  The year the data was produced.

<!-- -->

    Top_20_liked_tweets <- jsonlite::toJSON(sesamestreet_data_favorite_count_over_50)

After converting your data to a JSON file format, you are able to use
the `write`-function from R basics to export the data and save it on
your machine

    write(Top_20_liked_tweets, "Top_20_liked_tweets.json")

# Creating a new dataset of the top 20 most liked tweets (non-verified accounts)

You now wish to see the top 20 most liked tweets by the non-verified
accounts.

To do this, you follow the same workflow as before, but in our first
code chunk, you include an extra `filter`-function from the
dplyr-package which retains all rows with the value FALSE in the
verified column, thereby removing all tweets from our data which have
been produced by verified accounts.

    sesamestreet_data %>% 
      filter(is_retweet == FALSE) %>%
      filter(verified == FALSE) %>% 
      arrange(desc(favorite_count))
(Output removed because of privacy reasons)

You observe in the returned data.frame that 809 of the total 2435
observations are not retweets AND are from non-verified accounts.

Looking again at the favorite\_count column, you observe that the top 20
most commented tweets by non-verified accounts all have a count that is
above 15. This time, 2 tweets share the 20th and 21th place. You
therefore get the top 21 most liked tweets for this analysis.

You can now filter tweets that have been liked more than 15 times, and
arrange them from the most commented to the least, and create a new
dataset in our Global Environment called
*sesamestreet\_data\_favorite\_count\_over\_15\_non\_verified*.

    sesamestreet_data %>% 
      filter(is_retweet == FALSE) %>%
      filter(verified == FALSE) %>%
      filter(favorite_count > 8) %>% 
      arrange(desc(favorite_count)) -> sesamestreet_data_favorite_count_over_15_non_verified

# Inspecting our new dafaframe (non-verified)

We once again create a quick overview of our new dataset by using the
`select` and `arrange`-function as in before, and inspect our chosen
values in the returned data.frame.

    sesamestreet_data_favorite_count_over_15_non_verified %>% 
      select(favorite_count, screen_name, verified, text) %>% 
      arrange(desc(favorite_count))
(Output removed because of privacy reasons)

# Exporting the new dataset as a JSON file

Once again you use the `toJSON`-function to export our data into a local
JSON file.

    Top_21_liked_tweets_non_verified <- jsonlite::toJSON(sesamestreet_data_favorite_count_over_15_non_verified)

    write(Top_21_liked_tweets_non_verified, "Top_21_liked_tweets_non_verified.json")

You should now have two JSON files stored in your designated directory,
ready to be loaded into another R Markdown for a close reading analysis,
or you can inspect the text column of the datasets in your current R
Global Environment.
