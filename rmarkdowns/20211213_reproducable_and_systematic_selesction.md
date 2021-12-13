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

    ## # A tibble: 871 x 90
    ##    user_id status_id created_at          screen_name text  source
    ##    <chr>   <chr>     <dttm>              <chr>       <chr> <chr> 
    ##  1 863306… 14678714… 2021-12-06 15:00:01 sesamestre… "Lif… Twitt…
    ##  2 863306… 14696833… 2021-12-11 15:00:00 sesamestre… "Ses… Twitt…
    ##  3 863306… 14700457… 2021-12-12 15:00:02 sesamestre… "Our… Twitt…
    ##  4 237845… 14697057… 2021-12-11 16:28:49 GeorgeTakei "Bra… Twitt…
    ##  5 863306… 14682338… 2021-12-07 15:00:03 sesamestre… "Tod… Twitt…
    ##  6 863306… 14679167… 2021-12-06 18:00:00 sesamestre… "Who… Twitt…
    ##  7 863306… 14672378… 2021-12-04 21:02:16 sesamestre… "Cli… Twitt…
    ##  8 863306… 14686566… 2021-12-08 19:00:01 sesamestre… "You… Twitt…
    ##  9 863306… 14675090… 2021-12-05 15:00:01 sesamestre… "Gon… Twitt…
    ## 10 238839… 14682670… 2021-12-07 17:12:01 DanielDBec… "Hol… Twitt…
    ## # … with 861 more rows, and 84 more variables: display_text_width <dbl>,
    ## #   reply_to_status_id <chr>, reply_to_user_id <chr>,
    ## #   reply_to_screen_name <chr>, is_quote <lgl>, is_retweet <lgl>,
    ## #   favorite_count <int>, retweet_count <int>, quote_count <int>,
    ## #   reply_count <int>, hashtags <list>, symbols <list>, urls_url <list>,
    ## #   urls_t.co <list>, urls_expanded_url <list>, media_url <list>,
    ## #   media_t.co <list>, media_expanded_url <list>, media_type <list>,
    ## #   ext_media_url <list>, ext_media_t.co <list>, ext_media_expanded_url <list>,
    ## #   ext_media_type <chr>, mentions_user_id <list>, mentions_screen_name <list>,
    ## #   lang <chr>, quoted_status_id <chr>, quoted_text <chr>,
    ## #   quoted_created_at <dttm>, quoted_source <chr>, quoted_favorite_count <int>,
    ## #   quoted_retweet_count <int>, quoted_user_id <chr>, quoted_screen_name <chr>,
    ## #   quoted_name <chr>, quoted_followers_count <int>,
    ## #   quoted_friends_count <int>, quoted_statuses_count <int>,
    ## #   quoted_location <chr>, quoted_description <chr>, quoted_verified <lgl>,
    ## #   retweet_status_id <chr>, retweet_text <chr>, retweet_created_at <dttm>,
    ## #   retweet_source <chr>, retweet_favorite_count <int>,
    ## #   retweet_retweet_count <int>, retweet_user_id <chr>,
    ## #   retweet_screen_name <chr>, retweet_name <chr>,
    ## #   retweet_followers_count <int>, retweet_friends_count <int>,
    ## #   retweet_statuses_count <int>, retweet_location <chr>,
    ## #   retweet_description <chr>, retweet_verified <lgl>, place_url <chr>,
    ## #   place_name <chr>, place_full_name <chr>, place_type <chr>, country <chr>,
    ## #   country_code <chr>, geo_coords <list>, coords_coords <list>,
    ## #   bbox_coords <list>, status_url <chr>, name <chr>, location <chr>,
    ## #   description <chr>, url <chr>, protected <lgl>, followers_count <int>,
    ## #   friends_count <int>, listed_count <int>, statuses_count <int>,
    ## #   favourites_count <int>, account_created_at <dttm>, verified <lgl>,
    ## #   profile_url <chr>, profile_expanded_url <chr>, account_lang <lgl>,
    ## #   profile_banner_url <chr>, profile_background_url <chr>,
    ## #   profile_image_url <chr>

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

    ## # A tibble: 20 x 4
    ##    favorite_count screen_name   verified text                                   
    ##             <int> <chr>         <lgl>    <chr>                                  
    ##  1            968 sesamestreet  TRUE     "Life is a balancing act and we think …
    ##  2            941 sesamestreet  TRUE     "Sesame Street's most trusted news sou…
    ##  3            825 sesamestreet  TRUE     "Our favorite part of the holiday seas…
    ##  4            708 GeorgeTakei   TRUE     "Bravo, @SesameStreet! 💕 Such an impor…
    ##  5            581 sesamestreet  TRUE     "Today we're celebrating our friend Ro…
    ##  6            574 sesamestreet  TRUE     "Who else is catching up on emails and…
    ##  7            560 sesamestreet  TRUE     "Clicking \"Accept All Cookies\" feels…
    ##  8            522 sesamestreet  TRUE     "You deserve a little break! Stop scro…
    ##  9            376 sesamestreet  TRUE     "Gonger isn't the only culinary star i…
    ## 10            286 DanielDBeckw… FALSE    "Holiday Card #7, 2016\n\"Gee, Bert, d…
    ## 11            213 sesamestreet  TRUE     "Two friends, two ideas, and one piece…
    ## 12            207 sesamestreet  TRUE     "'Tis the season to be jolly with all …
    ## 13            207 sesamestreet  TRUE     "Everyone's favorite Bug Scouts are he…
    ## 14            187 sesamestreet  TRUE     "It's the most wonderful time of the y…
    ## 15            166 Bigbird96     FALSE    "\"How do I know I'm here, not in some…
    ## 16            128 sesamestreet  TRUE     "We’re more than just a children's sho…
    ## 17             98 MHermannPhoto FALSE    "A joyous night as @StreetGangMovie ce…
    ## 18             82 GirlOnAnIsla… FALSE    "We just put a mini tree up in my daug…
    ## 19             73 MuppetWiki    FALSE    "Long before social media was used to …
    ## 20             58 snkrdunk      TRUE     "🍪販売情報🍪\nSesame Street × Curry Flow 9 …

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

    ## # A tibble: 826 x 90
    ##    user_id status_id created_at          screen_name text  source
    ##    <chr>   <chr>     <dttm>              <chr>       <chr> <chr> 
    ##  1 238839… 14682670… 2021-12-07 17:12:01 DanielDBec… "Hol… Twitt…
    ##  2 423374… 14684103… 2021-12-08 02:41:22 Bigbird96   "\"H… Twitt…
    ##  3 270949… 14697017… 2021-12-11 16:12:46 MHermannPh… "A j… Twitt…
    ##  4 189820… 14700472… 2021-12-12 15:05:48 GirlOnAnIs… "We … Twitt…
    ##  5 284002… 14682977… 2021-12-07 19:14:00 MuppetWiki  "Lon… Twitt…
    ##  6 987334… 14681832… 2021-12-07 11:38:50 miszaszym1  "Loo… Twitt…
    ##  7 745564… 14688026… 2021-12-09 04:40:16 ladyheathe… "All… Twitt…
    ##  8 124377… 14683917… 2021-12-08 01:27:22 Nakwame_Sa… "\"H… Twitt…
    ##  9 142007… 14679019… 2021-12-06 17:01:11 momrrystag… "I l… Twitt…
    ## 10 121330… 14682489… 2021-12-07 16:00:00 magic_ratd… "🎶🔔H… Twitt…
    ## # … with 816 more rows, and 84 more variables: display_text_width <dbl>,
    ## #   reply_to_status_id <chr>, reply_to_user_id <chr>,
    ## #   reply_to_screen_name <chr>, is_quote <lgl>, is_retweet <lgl>,
    ## #   favorite_count <int>, retweet_count <int>, quote_count <int>,
    ## #   reply_count <int>, hashtags <list>, symbols <list>, urls_url <list>,
    ## #   urls_t.co <list>, urls_expanded_url <list>, media_url <list>,
    ## #   media_t.co <list>, media_expanded_url <list>, media_type <list>,
    ## #   ext_media_url <list>, ext_media_t.co <list>, ext_media_expanded_url <list>,
    ## #   ext_media_type <chr>, mentions_user_id <list>, mentions_screen_name <list>,
    ## #   lang <chr>, quoted_status_id <chr>, quoted_text <chr>,
    ## #   quoted_created_at <dttm>, quoted_source <chr>, quoted_favorite_count <int>,
    ## #   quoted_retweet_count <int>, quoted_user_id <chr>, quoted_screen_name <chr>,
    ## #   quoted_name <chr>, quoted_followers_count <int>,
    ## #   quoted_friends_count <int>, quoted_statuses_count <int>,
    ## #   quoted_location <chr>, quoted_description <chr>, quoted_verified <lgl>,
    ## #   retweet_status_id <chr>, retweet_text <chr>, retweet_created_at <dttm>,
    ## #   retweet_source <chr>, retweet_favorite_count <int>,
    ## #   retweet_retweet_count <int>, retweet_user_id <chr>,
    ## #   retweet_screen_name <chr>, retweet_name <chr>,
    ## #   retweet_followers_count <int>, retweet_friends_count <int>,
    ## #   retweet_statuses_count <int>, retweet_location <chr>,
    ## #   retweet_description <chr>, retweet_verified <lgl>, place_url <chr>,
    ## #   place_name <chr>, place_full_name <chr>, place_type <chr>, country <chr>,
    ## #   country_code <chr>, geo_coords <list>, coords_coords <list>,
    ## #   bbox_coords <list>, status_url <chr>, name <chr>, location <chr>,
    ## #   description <chr>, url <chr>, protected <lgl>, followers_count <int>,
    ## #   friends_count <int>, listed_count <int>, statuses_count <int>,
    ## #   favourites_count <int>, account_created_at <dttm>, verified <lgl>,
    ## #   profile_url <chr>, profile_expanded_url <chr>, account_lang <lgl>,
    ## #   profile_banner_url <chr>, profile_background_url <chr>,
    ## #   profile_image_url <chr>

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

    ## # A tibble: 42 x 4
    ##    favorite_count screen_name   verified text                                   
    ##             <int> <chr>         <lgl>    <chr>                                  
    ##  1            286 DanielDBeckw… FALSE    "Holiday Card #7, 2016\n\"Gee, Bert, d…
    ##  2            166 Bigbird96     FALSE    "\"How do I know I'm here, not in some…
    ##  3             98 MHermannPhoto FALSE    "A joyous night as @StreetGangMovie ce…
    ##  4             82 GirlOnAnIsla… FALSE    "We just put a mini tree up in my daug…
    ##  5             73 MuppetWiki    FALSE    "Long before social media was used to …
    ##  6             48 miszaszym1    FALSE    "Look! It's a cookie! @LEGO_Group #leg…
    ##  7             43 ladyheatherl… FALSE    "All the doctor books are always “you …
    ##  8             35 Nakwame_Sasha FALSE    "\"Hey Bert!\"\n.\n.\n#sesamestreet #s…
    ##  9             33 momrrystagram FALSE    "I love U! ❤️ #TheLetterU #SesameStree…
    ## 10             24 magic_ratdem… FALSE    "🎶🔔Happy Birthday Rosita🔔🎶\n#sesamestr…
    ## # … with 32 more rows

# Exporting the new dataset as a JSON file

Once again you use the `toJSON`-function to export our data into a local
JSON file.

    Top_21_liked_tweets_non_verified <- jsonlite::toJSON(sesamestreet_data_favorite_count_over_15_non_verified)

    write(Top_21_liked_tweets_non_verified, "Top_21_liked_tweets_non_verified.json")

You should now have two JSON files stored in your designated directory,
ready to be loaded into another R Markdown for a close reading analysis,
or you can inspect the text column of the datasets in your current R
Global Environment.
