
### Observations
-  BBC has the highest overall sentiment
-  CNN has the lowest overall sentiment
-  FOX seems to have the most neutral sentiment


```python
import os
import tweepy
import json
import time
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()
```


```python
file_name = "api_keys.json"
data = api_keys = json.load(open("c:/Users/charl/Desktop/APIKeys/api_keys.json"))

gkey = data['google_places_api_key']
consumer_key = data['twitter_consumer_key']
consumer_secret = data['twitter_consumer_secret']
access_token = data['twitter_access_token']
access_token_secret = data['twitter_access_token_secret']
```


```python
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth, parser=tweepy.parsers.JSONParser())
```


```python
compound_list = []
positive_list = []
negative_list = []
neutral_list = []

```


```python
news_networks = ['@BBC', '@CNN', '@CBS', '@FoxNews', '@NYT']

```


```python
news = []
account =[]
date = []
text =[]
tweet_number=[]
compound_list = []
positive_list = []
negative_list = []
neutral_list = []
for network in news_networks:
    public_tweets = api.search(network, count=100, result_type="recent")
    for tweet in public_tweets['statuses']:
        compound = analyzer.polarity_scores(tweet["text"])["compound"]
        pos = analyzer.polarity_scores(tweet["text"])["pos"]
        neu = analyzer.polarity_scores(tweet["text"])["neu"]
        neg = analyzer.polarity_scores(tweet["text"])["neg"]
        news.append(network)
        account.append(tweet['user']['name'])
        date.append(tweet['created_at'])
        text.append(tweet['text'])
        compound_list.append(compound)
        positive_list.append(pos)
        negative_list.append(neg)
        neutral_list.append(neu)
```


```python
df = pd.DataFrame({
    "Network":news,
    "User":account,
    "Date":date,
    "Text":text,
    "Positive":positive_list,
    "Neutral":neutral_list,
    "Negative":negative_list,
    "Compound":compound_list,
})
df = df[['Network','User','Date','Text','Positive','Neutral','Negative','Compound']]

```


```python
df.to_csv("Twitter_Sentiments.csv")
df = df.sort_values("Date")
df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Network</th>
      <th>User</th>
      <th>Date</th>
      <th>Text</th>
      <th>Positive</th>
      <th>Neutral</th>
      <th>Negative</th>
      <th>Compound</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>499</th>
      <td>@NYT</td>
      <td>Chakradhar Mohanta</td>
      <td>Thu Feb 22 13:27:46 +0000 2018</td>
      <td>#BhasaAndolanOdisha at #Chhatrapur, Ganjam on ...</td>
      <td>0.000</td>
      <td>0.849</td>
      <td>0.151</td>
      <td>-0.4939</td>
    </tr>
    <tr>
      <th>498</th>
      <td>@NYT</td>
      <td>🎗nthropologies||☆||</td>
      <td>Thu Feb 22 13:37:56 +0000 2018</td>
      <td>RT @NYT: 28 Years After His Death, a Composer ...</td>
      <td>0.000</td>
      <td>0.738</td>
      <td>0.262</td>
      <td>-0.5994</td>
    </tr>
    <tr>
      <th>497</th>
      <td>@NYT</td>
      <td>#ParklandCrisisActors</td>
      <td>Thu Feb 22 14:08:42 +0000 2018</td>
      <td>@Anthony When are they going to ban @CNN @MSNB...</td>
      <td>0.000</td>
      <td>0.783</td>
      <td>0.217</td>
      <td>-0.5574</td>
    </tr>
    <tr>
      <th>496</th>
      <td>@NYT</td>
      <td>N McKeel Petrella</td>
      <td>Thu Feb 22 14:10:15 +0000 2018</td>
      <td>@rbutler23 @NYT The idea is ludicrous. Fortuna...</td>
      <td>0.000</td>
      <td>0.800</td>
      <td>0.200</td>
      <td>-0.3612</td>
    </tr>
    <tr>
      <th>495</th>
      <td>@NYT</td>
      <td>Jose Tijam, PMP, CSM</td>
      <td>Thu Feb 22 14:14:08 +0000 2018</td>
      <td>"Leaping Over the Language Barrier" via @NYT T...</td>
      <td>0.000</td>
      <td>0.903</td>
      <td>0.097</td>
      <td>-0.1280</td>
    </tr>
    <tr>
      <th>494</th>
      <td>@NYT</td>
      <td>thehalpernteam</td>
      <td>Thu Feb 22 14:18:03 +0000 2018</td>
      <td>"Homes for Sale in Manhattan and Brooklyn" by ...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>493</th>
      <td>@NYT</td>
      <td>thehalpernteam</td>
      <td>Thu Feb 22 14:18:04 +0000 2018</td>
      <td>"Homes for Sale in New York and New Jersey" by...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>492</th>
      <td>@NYT</td>
      <td>thehalpernteam</td>
      <td>Thu Feb 22 14:18:05 +0000 2018</td>
      <td>"On the Market in New York and New Jersey" by ...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>491</th>
      <td>@NYT</td>
      <td>thehalpernteam</td>
      <td>Thu Feb 22 14:18:05 +0000 2018</td>
      <td>"On the Market in New York City" by Unknown Au...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>490</th>
      <td>@NYT</td>
      <td>Sean Sasso</td>
      <td>Thu Feb 22 14:18:10 +0000 2018</td>
      <td>"Homes for Sale in Manhattan and Brooklyn" via...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>489</th>
      <td>@NYT</td>
      <td>Sean Sasso</td>
      <td>Thu Feb 22 14:18:11 +0000 2018</td>
      <td>"Homes for Sale in New York and New Jersey" vi...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>488</th>
      <td>@NYT</td>
      <td>Sean Sasso</td>
      <td>Thu Feb 22 14:18:12 +0000 2018</td>
      <td>"On the Market in New York and New Jersey" via...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>487</th>
      <td>@NYT</td>
      <td>Sean Sasso</td>
      <td>Thu Feb 22 14:18:13 +0000 2018</td>
      <td>"On the Market in New York City" via @NYT http...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>486</th>
      <td>@NYT</td>
      <td>Sean Sasso</td>
      <td>Thu Feb 22 14:30:36 +0000 2018</td>
      <td>"Think Your Commute is Bad?" via @NYT https://...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>485</th>
      <td>@NYT</td>
      <td>thehalpernteam</td>
      <td>Thu Feb 22 14:33:22 +0000 2018</td>
      <td>"Think Your Commute is Bad?" by MICHAEL KOLOMA...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>484</th>
      <td>@NYT</td>
      <td>FusionDOJ</td>
      <td>Thu Feb 22 14:40:39 +0000 2018</td>
      <td>@adamgoldmanNYT Tweets like this do not help y...</td>
      <td>0.324</td>
      <td>0.576</td>
      <td>0.100</td>
      <td>0.6177</td>
    </tr>
    <tr>
      <th>483</th>
      <td>@NYT</td>
      <td>cjland</td>
      <td>Thu Feb 22 14:46:16 +0000 2018</td>
      <td>Trilobites: For Vampire Bats to Live on Blood,...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>482</th>
      <td>@NYT</td>
      <td>AVST-CX-E</td>
      <td>Thu Feb 22 14:46:17 +0000 2018</td>
      <td>Trilobites: For Vampire Bats to Live on Blood,...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>481</th>
      <td>@NYT</td>
      <td>Shirley Moore</td>
      <td>Thu Feb 22 15:04:00 +0000 2018</td>
      <td>America sold out by its Media @GOP Congress an...</td>
      <td>0.000</td>
      <td>0.844</td>
      <td>0.156</td>
      <td>-0.5719</td>
    </tr>
    <tr>
      <th>480</th>
      <td>@NYT</td>
      <td>Bruce Wolman</td>
      <td>Thu Feb 22 15:11:59 +0000 2018</td>
      <td>@dfunkedtt @CharlesMBlow @KillerMike @Kaeperni...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>479</th>
      <td>@NYT</td>
      <td>Sean Sasso</td>
      <td>Thu Feb 22 15:15:14 +0000 2018</td>
      <td>"Homes for Sale in Toronto" via @NYT https://t...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>478</th>
      <td>@NYT</td>
      <td>Sean Sasso</td>
      <td>Thu Feb 22 15:15:16 +0000 2018</td>
      <td>"On the Market in Toronto" via @NYT https://t....</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>477</th>
      <td>@NYT</td>
      <td>thehalpernteam</td>
      <td>Thu Feb 22 15:18:09 +0000 2018</td>
      <td>"Homes for Sale in Toronto" by TARA DESCHAMPS ...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>476</th>
      <td>@NYT</td>
      <td>thehalpernteam</td>
      <td>Thu Feb 22 15:18:10 +0000 2018</td>
      <td>"On the Market in Toronto" by Unknown Author v...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>475</th>
      <td>@NYT</td>
      <td>Jose Tijam, PMP, CSM</td>
      <td>Thu Feb 22 15:19:18 +0000 2018</td>
      <td>"Fosun Triumphs in Bidding War for Lanvin, Tro...</td>
      <td>0.126</td>
      <td>0.586</td>
      <td>0.289</td>
      <td>-0.5994</td>
    </tr>
    <tr>
      <th>474</th>
      <td>@NYT</td>
      <td>Hugoden</td>
      <td>Thu Feb 22 15:40:17 +0000 2018</td>
      <td>@Nuke_The_Liars @ds13_manon An media manipulat...</td>
      <td>0.137</td>
      <td>0.758</td>
      <td>0.104</td>
      <td>0.1779</td>
    </tr>
    <tr>
      <th>473</th>
      <td>@NYT</td>
      <td>SquareOne™</td>
      <td>Thu Feb 22 15:43:13 +0000 2018</td>
      <td>Fosun Triumphs in Bidding War for Lanvin, Trou...</td>
      <td>0.151</td>
      <td>0.503</td>
      <td>0.347</td>
      <td>-0.5994</td>
    </tr>
    <tr>
      <th>472</th>
      <td>@NYT</td>
      <td>No pasarán</td>
      <td>Thu Feb 22 15:49:33 +0000 2018</td>
      <td>@exercitooficial @nyt @TheYoungTurks @OEA_ofic...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>471</th>
      <td>@NYT</td>
      <td>Rich with Empathy</td>
      <td>Thu Feb 22 16:00:00 +0000 2018</td>
      <td>RT @LynnHildebrandt: Hey look, Mikey!  @RepMik...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>470</th>
      <td>@NYT</td>
      <td>Jose Tijam, PMP, CSM</td>
      <td>Thu Feb 22 16:04:34 +0000 2018</td>
      <td>"Margaret Brennan Named Host of ‘Face the Nati...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>316</th>
      <td>@FoxNews</td>
      <td>Woman on a Mission🗽🇺🇸 🌏🏛❄</td>
      <td>Thu Feb 22 22:34:29 +0000 2018</td>
      <td>@FoxNews @EricTrump No thanks a**hole https://...</td>
      <td>0.319</td>
      <td>0.440</td>
      <td>0.242</td>
      <td>0.1779</td>
    </tr>
    <tr>
      <th>312</th>
      <td>@FoxNews</td>
      <td>Strongbow Enterprise</td>
      <td>Thu Feb 22 22:34:29 +0000 2018</td>
      <td>Breaking:\n\nMissouri Governor Eric Greitens i...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>313</th>
      <td>@FoxNews</td>
      <td>Alex Kelley</td>
      <td>Thu Feb 22 22:34:29 +0000 2018</td>
      <td>RT @tweetfromalex: @FoxNews @POTUS @FBI A repo...</td>
      <td>0.119</td>
      <td>0.881</td>
      <td>0.000</td>
      <td>0.3182</td>
    </tr>
    <tr>
      <th>114</th>
      <td>@CNN</td>
      <td>The Citizen Cyborg</td>
      <td>Thu Feb 22 22:34:29 +0000 2018</td>
      <td>what the hell is going on...?\nSecurity footag...</td>
      <td>0.099</td>
      <td>0.617</td>
      <td>0.284</td>
      <td>-0.6705</td>
    </tr>
    <tr>
      <th>112</th>
      <td>@CNN</td>
      <td>Christian DeWeese</td>
      <td>Thu Feb 22 22:34:30 +0000 2018</td>
      <td>RT @CNN: Months before Nikolas Cruz killed 17 ...</td>
      <td>0.000</td>
      <td>0.677</td>
      <td>0.323</td>
      <td>-0.8885</td>
    </tr>
    <tr>
      <th>111</th>
      <td>@CNN</td>
      <td>🌹Mimi Frederick🌹</td>
      <td>Thu Feb 22 22:34:30 +0000 2018</td>
      <td>@BasedMonitored @CNN Hey @CNN. Why don’t you t...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>110</th>
      <td>@CNN</td>
      <td>Pat Hahn</td>
      <td>Thu Feb 22 22:34:30 +0000 2018</td>
      <td>RT @CNN: The FCC's repeal of net neutrality is...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>109</th>
      <td>@CNN</td>
      <td>nan k. wiggins</td>
      <td>Thu Feb 22 22:34:30 +0000 2018</td>
      <td>@CNN Good!</td>
      <td>0.761</td>
      <td>0.239</td>
      <td>0.000</td>
      <td>0.4926</td>
    </tr>
    <tr>
      <th>108</th>
      <td>@CNN</td>
      <td>Denise Downing</td>
      <td>Thu Feb 22 22:34:30 +0000 2018</td>
      <td>RT @RightWingLawman: #ThursdayThoughts: @CNN, ...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>311</th>
      <td>@FoxNews</td>
      <td>TrumpisaTraitor</td>
      <td>Thu Feb 22 22:34:30 +0000 2018</td>
      <td>@FoxNews @JesseBWatters @POTUS Mueller</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>310</th>
      <td>@FoxNews</td>
      <td>David  Benjamin</td>
      <td>Thu Feb 22 22:34:30 +0000 2018</td>
      <td>#Five @greggutfeld making fun of @NancyPelosi ...</td>
      <td>0.183</td>
      <td>0.817</td>
      <td>0.000</td>
      <td>0.6440</td>
    </tr>
    <tr>
      <th>309</th>
      <td>@FoxNews</td>
      <td>Dewane</td>
      <td>Thu Feb 22 22:34:31 +0000 2018</td>
      <td>@ElderLansing @Blklivesmatter @NRA @FoxNews sh...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>107</th>
      <td>@CNN</td>
      <td>Dale Schuster</td>
      <td>Thu Feb 22 22:34:31 +0000 2018</td>
      <td>@Crypto_keepr @Djcabot1Cabot @CNN Crypto - so ...</td>
      <td>0.000</td>
      <td>0.739</td>
      <td>0.261</td>
      <td>-0.7184</td>
    </tr>
    <tr>
      <th>106</th>
      <td>@CNN</td>
      <td>Randy Sutton</td>
      <td>Thu Feb 22 22:34:31 +0000 2018</td>
      <td>@KrisParonto @CNN @cnnbrk  https://t.co/bmnXsY...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>103</th>
      <td>@CNN</td>
      <td>45yr progressive</td>
      <td>Thu Feb 22 22:34:31 +0000 2018</td>
      <td>And to  their faces. @RepTimRyan These childre...</td>
      <td>0.000</td>
      <td>0.833</td>
      <td>0.167</td>
      <td>-0.3400</td>
    </tr>
    <tr>
      <th>200</th>
      <td>@CBS</td>
      <td>45yr progressive</td>
      <td>Thu Feb 22 22:34:31 +0000 2018</td>
      <td>And to  their faces. @RepTimRyan These childre...</td>
      <td>0.000</td>
      <td>0.833</td>
      <td>0.167</td>
      <td>-0.3400</td>
    </tr>
    <tr>
      <th>306</th>
      <td>@FoxNews</td>
      <td>Sophie Rowlett</td>
      <td>Thu Feb 22 22:34:31 +0000 2018</td>
      <td>@WithoutaTRACE @Sunny_Sun_Belt @FoxNews 😂😂</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>307</th>
      <td>@FoxNews</td>
      <td>Mark1963USA!</td>
      <td>Thu Feb 22 22:34:31 +0000 2018</td>
      <td>RT @minnman47: McCain associate takes Fifth on...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>308</th>
      <td>@FoxNews</td>
      <td>NEVER SILENCED</td>
      <td>Thu Feb 22 22:34:31 +0000 2018</td>
      <td>RT @RedBirdRight: McCain associate takes Fifth...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>105</th>
      <td>@CNN</td>
      <td>brian wise</td>
      <td>Thu Feb 22 22:34:31 +0000 2018</td>
      <td>@sledfast597 @passantino @exjon @CNN Fuck you?</td>
      <td>0.000</td>
      <td>0.588</td>
      <td>0.412</td>
      <td>-0.5423</td>
    </tr>
    <tr>
      <th>104</th>
      <td>@CNN</td>
      <td>THE GUARDIAN #Trump Train @Beverly04409771</td>
      <td>Thu Feb 22 22:34:31 +0000 2018</td>
      <td>RT @President1Trump: UNBELIEVABLE: @DLoesch de...</td>
      <td>0.119</td>
      <td>0.881</td>
      <td>0.000</td>
      <td>0.4263</td>
    </tr>
    <tr>
      <th>101</th>
      <td>@CNN</td>
      <td>Jim Lambert</td>
      <td>Thu Feb 22 22:34:32 +0000 2018</td>
      <td>RT @KrisParonto: 🤦🏻‍♂️🤦🏻‍♂️ ..... @CNN @MSNBC ...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>100</th>
      <td>@CNN</td>
      <td>Deplorable Chicklet 🇺🇸🇺🇸</td>
      <td>Thu Feb 22 22:34:32 +0000 2018</td>
      <td>@forward4US @CNN LOL, you stopped at deplorabl...</td>
      <td>0.151</td>
      <td>0.597</td>
      <td>0.252</td>
      <td>-0.3328</td>
    </tr>
    <tr>
      <th>102</th>
      <td>@CNN</td>
      <td>CJ Pfeiff</td>
      <td>Thu Feb 22 22:34:32 +0000 2018</td>
      <td>@CNN You still have no idea the hand you playe...</td>
      <td>0.209</td>
      <td>0.709</td>
      <td>0.082</td>
      <td>0.5267</td>
    </tr>
    <tr>
      <th>305</th>
      <td>@FoxNews</td>
      <td>David Edgerton</td>
      <td>Thu Feb 22 22:34:33 +0000 2018</td>
      <td>@toddstarnes @DLoesch @CNN Signs of a liar - \...</td>
      <td>0.000</td>
      <td>0.845</td>
      <td>0.155</td>
      <td>-0.5106</td>
    </tr>
    <tr>
      <th>304</th>
      <td>@FoxNews</td>
      <td>Kyle $ Gibson</td>
      <td>Thu Feb 22 22:34:33 +0000 2018</td>
      <td>@RobertoLagomas1 @LeslieLKing_JR @belikemike @...</td>
      <td>0.153</td>
      <td>0.678</td>
      <td>0.169</td>
      <td>-0.0772</td>
    </tr>
    <tr>
      <th>300</th>
      <td>@FoxNews</td>
      <td>Tuck Frump🖕🍊🤡</td>
      <td>Thu Feb 22 22:34:34 +0000 2018</td>
      <td>@NBCNews Is anyone really surprised that this ...</td>
      <td>0.102</td>
      <td>0.741</td>
      <td>0.157</td>
      <td>-0.2975</td>
    </tr>
    <tr>
      <th>301</th>
      <td>@FoxNews</td>
      <td>debbie p</td>
      <td>Thu Feb 22 22:34:34 +0000 2018</td>
      <td>RT @Judie_Chitwood: @FoxNews @Judgenap Will be...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>302</th>
      <td>@FoxNews</td>
      <td>tells you how</td>
      <td>Thu Feb 22 22:34:34 +0000 2018</td>
      <td>RT @Mrsbowmanocsa: @FoxNews @POTUS I am a teac...</td>
      <td>0.000</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.0000</td>
    </tr>
    <tr>
      <th>303</th>
      <td>@FoxNews</td>
      <td>Kelsey, M.S.</td>
      <td>Thu Feb 22 22:34:34 +0000 2018</td>
      <td>RT @FoxNews: .@JesseBWatters: "You've had so m...</td>
      <td>0.107</td>
      <td>0.893</td>
      <td>0.000</td>
      <td>0.3400</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 8 columns</p>
</div>




```python
counter1=0
counter2=0
counter3=0
counter4=0
counter5=0

for index, row in df.iterrows():
    if (row["Network"]=="@CNN"):
        counter1 = counter1+1
        CNN = plt.scatter(counter1,row["Compound"],c ='r',marker="o")
    if (row["Network"]=="@NYT"):
        counter2 = counter2+1
        NYT = plt.scatter(counter2,row["Compound"],c ='y',marker="o")
    if (row["Network"]=="@FoxNews"):
        counter3 = counter3+1
        FOX = plt.scatter(counter3,row["Compound"],c ='b',marker="o")
    if (row["Network"]=="@BBC"):
        counter4 = counter4+1
        BBC = plt.scatter(counter4,row["Compound"],c ='cyan',marker="o")
    if (row["Network"]=="@CBS"):
        counter5 = counter5+1
        CBS = plt.scatter(counter5,row["Compound"],c ='g',marker="o")
        
plt.xlabel("Tweets Ago")
plt.ylabel("Tweet Polarity")
plt.title("Sentiment Analysis of Media Tweets (2/22)")
plt.legend(handles = [CNN,BBC,NYT,FOX,CBS], labels = ["CNN","BBC","NYT","FOX","CBS"], loc='center left', bbox_to_anchor=(1, 0.5))

sns.set
plt.show()
```


![png](output_9_0.png)



```python
new_df = pd.DataFrame(df.groupby("Network")["Compound"].mean())
new_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Compound</th>
    </tr>
    <tr>
      <th>Network</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>@BBC</th>
      <td>0.082417</td>
    </tr>
    <tr>
      <th>@CBS</th>
      <td>0.065172</td>
    </tr>
    <tr>
      <th>@CNN</th>
      <td>-0.066806</td>
    </tr>
    <tr>
      <th>@FoxNews</th>
      <td>-0.026990</td>
    </tr>
    <tr>
      <th>@NYT</th>
      <td>-0.039949</td>
    </tr>
  </tbody>
</table>
</div>




```python
x_axis = np.arange(len(new_df["Compound"]))
compound_score = new_df["Compound"]
tick_locations = [value+0.4 for value in x_axis]
plt.xticks(tick_locations, ["BBC", "CBS", "CNN", "FOX", "NYT"])
plt.bar(x_axis, compound_score, color = ['b','g','m','c','y'], alpha=0.5, align="edge")
plt.xlabel("Networks")
plt.ylabel("Tweet Polarity")
plt.title("Overall Media Sentiment Based on Twitter (2/22/2018)")
sns.set()
plt.show()
```


![png](output_11_0.png)

