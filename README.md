# Twitter-Project

#access for API


import json

import tweepy
from tweepy import API
from tweepy import Cursor
from tweepy import OAuthHandler
from tweepy import Stream
from textblob import TextBlob
import numpy as np
import pandas as pd
from datetime import datetime, timedelta


# Step 1 - Authenticate
CONSUMER_KEY= 'SgqPbWWvqTGPTHy8E3x6pdstW'
CONSUMER_KEY_SECRET= 'WOh5trXoQqOgPpOA34nU5LiXTGYon4DUq4Z6IdrwgIiJwTbro5'

ACCESS_TOKEN='1343849853265145858-SVzu7ZmphqPZsfZEaikk6VgvtSu04C'
ACCESS_TOKEN_SECRET='V8xGQ6UzPHTmef5Kd2DWu5oCyGXDMFgri0aICRaRA3jb5'

userName = '@JoeBiden'
hashtags = []
mentions = []
hashtagsRT = []
mentionsRT = []
allTweets = []
tweetCount = 0
#add username
auth = OAuthHandler(CONSUMER_KEY, CONSUMER_KEY_SECRET)

auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)

authAPI = API(auth)

user = authAPI.get_user(userName)

api = tweepy.API(auth)

endDate = datetime.utcnow() - timedelta(days = 0) 

for status in Cursor(authAPI.user_timeline, id = userName, tweet_mode = 'extended').items():
    # Here status is a form of dictionary which has several parameters for each tweets.
    # Storing the tweets in a list called allTweets

    if "retweeted_status" in dir(status):
        allTweets.append(status.retweeted_status.full_text)
    else:
        allTweets.append(status.full_text)
    tweetCount += 1
    



tweets = user.statuses_count
accountCreatedDate = user.created_at
delta = datetime.utcnow() - accountCreatedDate
accountAge = delta.days
print("RETRIEVING TWEETS OF " + userName)
print("NAME: " + user.name)
print("DESCRIPTION: " + user.description)
print("TOTAL TWEETS COUNT: " + str(user.statuses_count))
print("FOLLOWING COUNT: " + str(user.friends_count))
print("FOLLOWERS COUNT: " + str(user.followers_count))
print("ACCOUNT'S AGE (IN DAYS): " + str(accountAge))
if accountAge > 0:
    print("AVERAGE TWEETS PER DAY: " + "%.2f" %(float(tweets)/float(accountAge)))
print("DATE OF LAST PROCESSED TWEET: " + str(status.created_at))    
print("TOTAL NUMBER OF TWEETS PROCESSED: " + str(tweetCount))

# Finally saving all the collected tweets in a text file.

with open('tweets.txt', 'w') as fileWrite:
    for item in allTweets:
        text = item.split("\n")
        text = ' '.join(text).split()
        fileWrite.write(' '.join(text))
        fileWrite.write('\n')



