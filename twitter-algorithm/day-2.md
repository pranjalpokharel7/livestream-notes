- majority of time is spent on tweaking obs settings - maybe will have an audio engineering learning session?
- some additional separation of concern
	- home timeline generation
	- ML serving infra
	- content filtering and safety

- negative signals on twitter
```
NegativeEngagedTweetId = 901 // tweetId for all negative engagements
NegativeEngagedUserId = 902 // userId for all negative engagements
AccountBlock = 903,
AccountMute = 904,

// skip 905 - 906 for Account report abuse / report spam

// User clicked dont like from past 90 Days
TweetDontLike = 907

// User clicked see fewer on the recommended tweet from past 90 Days
TweetSeeFewer = 908

// User clicked on the "report tweet" option in the tweet caret dropdown menu from past 90 days
TweetReport = 909
```

- even the duration a video is watched on average is recorded as user action

### recommendation flow
1. user requests their timeline to home-mixer/server - all latter stages of recommendation flow is orchestrated by this service
2. **candidate generation** (discussed yesterday)
	1. in-network tweets (search index)
	2. **out-of-network tweets** (cr-mixer)
	3. interaction-based tweets (user-tweet-entity graph)
	4. user recommendations
3. feature hydration (eg, fetching image for tweet?)
4. **ranking**
	1. light ranker - initial scoring to pre-filter candidates
	2. heavy ranker - deep neural net for final scoring
5. filtering
6. deliver timeline

- since there are a lot of features that we could dive deep into and just get lost, we'll simply focus on 
	- cr-mixer