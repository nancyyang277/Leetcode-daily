# Leetcode 1642 Furthest Building You Can Reach (https://leetcode.com/problems/furthest-building-you-can-reach/description/)
## Problem
You are given an integer array heights representing the heights of buildings, some bricks, and some ladders.

You start your journey from building 0 and move to the next building by possibly using bricks or ladders.

While moving from building i to building i+1 (0-indexed),

If the current building's height is greater than or equal to the next building's height, you do not need a ladder or bricks.
If the current building's height is less than the next building's height, you can either use one ladder or (h[i+1] - h[i]) bricks.
Return the furthest building index (0-indexed) you can reach if you use the given ladders and bricks optimally.


## Solution
```
class Solution {
    public int furthestBuilding(int[] heights, int bricks, int ladders) {
        // int minVal = 0;
        // int far = 0;
        // int count = 0;
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> (b - a));
        int i = 0;
        for (; i < heights.length - 1; i++) {
            int diff = heights[i + 1] - heights[i];
            if (diff <= 0) {
                continue;
            }
            if (bricks - diff >= 0) {
                pq.add(diff);
                bricks -= diff;
            } else if (ladders > 0) {
                if (!pq.isEmpty() && pq.peek() > diff) {
                    bricks -= diff;
                    bricks += pq.peek();
                    pq.poll();
                    pq.add(diff);
                    ladders--;
                } else {
                    ladders--;
                }
            } else {
                break;
            }
            
        }
        return i;
    }
}
```

## Summary
- If we choose to exhaust the usage of bricks first, we need to use MaxHeap which stores the maximum usage for each diff
- The reason is because if we encounter a diff that is smaller than the maximum usage of bricks in previous diff, then we can let this diff to take up the previous bricks and spare out some bricks and use a ladder for the maximum usage of bricks in the heap
- If we choose to exhaust the usage of ladder, we need to use MinHeap
- If we encounter a diff that is greater than the least previous diff that uses ladder, we should use ladder for the current diff so we can spare some bricks and move further


# Leetcode Design Twitter (https://leetcode.com/problems/design-twitter/description/)
## Problem
Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and is able to see the 10 most recent tweets in the user's news feed.

Implement the Twitter class:

Twitter() Initializes your twitter object.
- void postTweet(int userId, int tweetId) Composes a new tweet with ID tweetId by the user userId. Each call to this function will be made with a unique tweetId.
- List<Integer> getNewsFeed(int userId) Retrieves the 10 most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be ordered from most recent to least recent.
- void follow(int followerId, int followeeId) The user with ID followerId started following the user with ID followeeId.
- void unfollow(int followerId, int followeeId) The user with ID followerId started unfollowing the user with ID followeeId.

## Solution
```
class Twitter {
    Map<Integer, HashSet<Integer>> followees;
    Map<Integer, List<Pair<Integer,Integer>>> tweets;
    int timeStamp;

    public Twitter() {
        timeStamp = 0;
        followees = new HashMap<>();
        tweets = new HashMap<>();
    }

    public void postTweet(int userId, int tweetId) {
        if (!tweets.containsKey(userId)) {
            tweets.put(userId, new ArrayList<Pair<Integer,Integer>>());
            follow(userId, userId);
        }
        tweets.get(userId).add(0, new Pair<>(tweetId, timeStamp++));
    }

    public List<Integer> getNewsFeed(int userId) {
        PriorityQueue<Pair<Integer,Integer>> feedHeap = new PriorityQueue<>(new Comparator<Pair<Integer,Integer>>() {
            public int compare(Pair<Integer,Integer> p1, Pair<Integer,Integer> p2) {
                return p2.getValue() - p1.getValue();
            }
        });
        
        Set<Integer> myFollowees = followees.get(userId);
        if(myFollowees != null){
            for (int followeeId : myFollowees) {
                List<Pair<Integer,Integer>> followeeTweets = tweets.get(followeeId);
                if(followeeTweets != null){
                    for (Pair<Integer,Integer> p : followeeTweets) {
                        feedHeap.offer(p);
                    }
                }
            }
        }

        List<Integer> myList = new ArrayList<>();
        while (!feedHeap.isEmpty() && myList.size() < 10) {
            myList.add(feedHeap.poll().getKey());
        }
        return myList;
    }

    public void follow(int followerId, int followeeId) {
        if (!followees.containsKey(followerId)) {
            followees.put(followerId, new HashSet<Integer>());
        }
        followees.get(followerId).add(followeeId);
    }

    public void unfollow(int followerId, int followeeId) {
        if(!followees.containsKey(followerId))
            return;
        followees.get(followerId).remove(followeeId);
    }
}
```

## Summary
- getNewsFeed() is the most important and tricky method
- The main idea is to get all the tweets posted by the its followees including himself, and put them in a heap, and then poll the first 10 tweets based on time or until heap is empty
- 
