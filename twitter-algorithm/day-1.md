**Streamed on: [[2025-10-04]]**

- twitter feed is very provocative compared to other social media - subjective feeling
- first thing we do is read the README

## recommendation pipeline
1. candidate sourcing: fetching tweets
2. ranking
3. filtering (blocked/muted user tweets)

**Home Mixer** 
- built on top of `product-mixer` -> framework for building feeds of content
- "for you" timeline

## candidate sourcing
- extract top 1500 tweets at this stage for a particular user
- people you follow/in-network source (50%) + don't follow/out-of-network source (50%) -> on average
- in-network sourcing: **real graph** -> predicting likelihood of engagement between 2 users (implying i see tweets from people i interact with more)

### out of network sourcing

> _How can we tell if a certain Tweet will be relevant to you if you don’t follow the author?_

- graphjet - social graph
	- "if you don't know what i like, maybe you can infer from the choices of the friends i hang out with"
	- real time content recommendations (currently serves about 15% of home timeline tweets)
- simclusters - embedding spaces
	- "what Tweets and Users are similar to my interests?"
	- [sbf](https://github.com/twitter/sbf) - sparse binary factorization
	- discovers "communities" anchored by a cluster of influential users
	- about 145k communities "updated" (what are they updating anyway?)

## Ranking
- rank the 1500 candidate tweets
- ~48M parameter neural network 
- "positive" engagment (likes, retweets, replies)
- just apply scores do not remove anything yet

### Filtering
- filtering over a small subset of ranked tweets (better optimized) - we won't need all 1500 to view at once anyway
- "**Author Diversity**: Avoid too many consecutive Tweets from a single author."
	- if i make too many tweets consecutively, all of my tweets will not be shown
- "negative feedback" - how is this measured?
- ensure someone you follow engaged with the Tweet or follows the Tweet’s author
- "Determine if the Tweets currently on a device are stale, and send instructions to replace them with the edited versions." - how would I know a tweet is stale?

two repos that I need to look at
- the algorithm
- the algorithm ml


reduce pagerank of users with low followers but high followings
- so lesser the ratio of `following:followers` the better?
- seems to only apply above threshold of following number over 2500 (number might be different on production?)

### tomorrow's plans
- setup scala and look a bit into it's documentation (before stream)
- outline on what directories to explore (so that it's more exploitative over explorative)
- read [deepwiki](https://deepwiki.com/twitter/the-algorithm) indexed contents for the repo 