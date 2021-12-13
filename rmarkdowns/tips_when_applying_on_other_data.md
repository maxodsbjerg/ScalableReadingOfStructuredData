# Tips and tricks 
As mentioned in the beginning of this lesson, there are different ways of obtaining your data. This section of the lesson can help you apply the code from this lesson to data collected in a different way. 

If you have collected your data by following the lesson [Beginner's Guide to Twitter Data](https://programminghistorian.org/en/lessons/beginners-guide-to-twitter-data) you will discover that the date of tweets is shown in a way, which is noncompatible with the code from this lesson. To make the code compatible with data from *Beginner's Guide to Twitter Data* the date column has to be manipulated with some regular expressions:

    df %>% 
    mutate(date = str_replace(created_at, "^[A-Z][a-z]{2} ([A-Z][a-z]{2}) (\\d{2}) (\\d{2}:\\d{2}:\\d{2}) \\+0000 (\\d{4})",
                                "\\4-\\1-\\2 \\3")) %>% 
    mutate(date = ymd_hms(date)) %>% 
    select(date, created_at, everything())

    df$Time <- format(as.POSIXct(df$date,format="%Y-%m-%d %H:%M:%S"),"%H:%M:%S")
    df$date <- format(as.POSIXct(df$date,format="%Y.%m-%d %H:%M:%S"),"%Y-%m-%d")

Some other columns that does not have the same names in our data as in the data extracted with the lesson *Beginner's Guide to Twitter Data* are our columns `verified`and `text` that are called `user.verified` and `full_text`. Here you have two options either you change the code, so that everywhere `verified` or `text` occurs you write `user.verified` or `full_text`. Another approch is to change the column names, which can be done with the following code:

    df %>% 
    rename(verified = user.verified) %>% 
    rename(text = full_text) -> df

