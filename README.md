# Pet-Ratings
Tweets from the twitter archive of a popular pet rating twitter handle @we_rate_dogs were curated using the twitter API and then analyzed for insight

Wrangling was done in three phases:
1. Gathering.
2. Assessment.
3. Cleaning.

## Gathering
The data needed for this project was spread across three different data sets;
• Files 1 Was download manually by clicking the link to it the project resources. Once
it was downloaded, upload it and read the data into a pandas DataFrame as twit-
ter_archive_enhanced.csv.
• file 2 was downloaded programmatically using the Requests library from Udacity’s servers.
A folder was created and the response object of the request which was a TSV file was written
into the folder. This was then read into a pandas Datframe object using pd.read_csv() setting
the "sep" parameter to ’’ as image_predictions.tsv.
• File 3 contained each tweet’s retweet count and favorite ("like") count and other additional
data. This was gathered Using the tweet IDs in the twitter_archive_enhanced dataframe.
The Twitter API for each tweet’s JSON data was queried using Python’s Tweepy library
after which each tweet’s entire set of JSON data was written line by line in a .txt file called
tweet_json.txt. The .txt file was then read line by line using pd.read_JSON.
1.2 Assessment
• The Dataframes were first assessed visually using MS Excel.
• The Dataframes were then assessed programmatically using the info(), Describe(), Query()
among others.

A summary of the findings from the assessment of the three datasets are as follows;
1

### Quality issues
1. 181 non-null values in the retweet_id column indicating retweets on the tweet_archive table.
2. Timestamp is a string on the tweet_archive table.
3. 78 non-null values in the in_reply_to_status_id column indicating a reply tweets on the tweet_archive table.
4. Some ratings Denominators are invalid e.g 170 on the tweet_archive table.
5. Some ratings numerators are invalid eg 1,776 on the tweet_archive table.
6. non descript column name on the tweet IDs table.
7. 324 predictions that aren’t dog breeds on the image prediction table.
8. Non descript column names on the image prediction table.
9. some tweets were beyond Aug. 1st 2017 on the twitter archove Table.

### Tidiness issues
1. The dog stage being stored with the texts in the text column of the twitter enhanced table.
2. Ratings is a single variable but it was spread into two columns on the twitter enhanced Table.
3. 29 columns on the tweets IDs table are not needed because some are repeated columns or
they donot help the current analysis in any way.
4. 13 columns not neccesary on the archive table(including retweet counts because we wouldnt
be making use of retweets for this analysis)
5. three predictions with different confidence levels on the image prediction table.
6. Drop unneccesary columns on the image prediction table.
7. only one table is neccesary.

## Cleaning
Before cleaning the datframes, a copy of all the original data was made. After this, all of the
Quality and Tidiness isuess identified in the three dataframes were then cleaned sequentially in
the following steps:

## Quality issues
• Dropped all retweets using the retweet status id column from the twitter archive table.
• Changed timestamp to datetime format on the twitter archive table.
• Dropped all replies using the reply status id column from the twitter archive table.
• Dropped all rows with rating denominators with 3 digits (>99)on the twitter archive table.
• Dropped all rows with rating Numerators with 3 digits (>99) on the twitter archive table.
• The favorite_count column was renamed to like_count and ID changed to tweets ID on the
tweets id table.
• Removed all predictions without atleast one dog breed prediction on the image prediction
table.
• Non descript column names on the image prediction table were cleaned by changing all "p"
to "prediction".
• Dropped all tweets beyond Aug. 1st 2017.

## Tidiness issues
• Extracted the dog stage from the text column into a new "stage" column on the twit-
ter_enhanced table.
• Divided the numerator rating by the denominator rating and stored it in a "ratings" column
on the twitter archive table.
• Dropped all columns except tweet_ids, like_count and retweet_count on the tweets id table.
• Dropped all coumns except; tweet_id, timestamp, text, name, stage, ratings twitter archive
table.
• Applied a function that select the breed prediction with the highest confidence level among
all three predictions and dropped the rest on the image prediction table.
• Dropped all other columns apart from tweet id and highest conf breed on the image predic-
tion table.
• All three tables were merged using the tweets id column.
The merged dataframe was then stored in a CSV file named twitter_archive_Master.csv
 
## Insights
The insights derived from the analysis were guided by three research questions;
Research Questions
Are certain breeds associated with higher ratings?
Do higer ratings correspond with higher retweet and like counts?
What are the characteristics of posts with the least ratings?

1. Are certain breeds associated with higher ratings?
The approach used to answer this question was to group the various dog breeds by their mean ratings and showing the top 10.
This showed the following breeds were associated with higher mean ratings with the clumber breed taking the lead with an average rating of 2.7.

Dog breed       Average rating

clumber                    2.7

military_uniform            1.4

laptop                      1.4

upright                     1.3

paper_towel                 1.3

canoe                       1.3

syringe                     1.3

Arabian_camel               1.3

Pomeranian                  1.2

Saluki                      1.2



2. Do higher ratings correspond with higher retweet and like counts and vice versa?
The method employed was to group the tweets into quartiles by ratings and then comparing the sum of total retweets and likes for the 1st and 4th quartile (<25% and >74% respectively) and also comparing the average retweet and like counts for both quartiles.
* Total number of likes
The total number of likes for tweets with ratings in the 4th quartile = 9,362,320
The total number of likes for tweets with ratings in the 1st quartile = 1,931,930
Likes per tweet
The average likes per tweets with ratings in the 4th quartile = 14,142.47
The average likes per tweets with ratings in the 1st quartile = 3,032.8
* Total number of tweets
The total number of retweets for tweets with ratings in the 4th quartile = 2,546,832
The total number of retweets for tweets with ratings in the 1st quartile = 641,288
Retweets per tweet
The average retweet per tweets with ratings in the 4th quartile = 3,847.1
The average retweet per tweets with ratings in the 1st quartile = 1,006.7
It can therefore be inferred that posts with higher mean ratings had more engagements compared to those with lower mean ratings.

3. What are the characteristics of posts with the least ratings?
The method employed was to query the dataframe for all tweets with ratings in the least 10%. Then this was explored.


* Top 10 Dominant breeds in this class
Chihuahua 16
pug 14
seat_belt 9
Labrador_retriever 8
golden_retriever 8
dalmatian 6
Italian_greyhound 6
French_bulldog 6
Pembroke 5
Shetland_sheepdog 5
The chihuahua was the most dominant breed having 16 tweets in the least 10% group.

* Most dominant Dog stage in the group

 
pupper  26
doggo   4
puppo   1


>The pupper Dog stage was the most dominant dog stage in this group.
>Average like count per tweet in this group is 820.4.
>Average retweet count per tweet in this group is 2548.1.


## Visualization
Four visualizations were created to give a few more insights using the mean counts since the proportion of the distribution were disproportinate.

Percentage of mean like counts by Dog stages


Percentage of mean retweet counts by Dog stages

 
Top 10 breeds with the highest mean like count

 
Top 10 breeds with the highest retweet mean retweet count
**These visualizations  are contained in the act report.pdf**

 
