# X API Endpoint Master List

## 1. AUTHENTICATION

### OAuth 2.0
- `POST /2/oauth2/token`
- `POST /2/oauth2/revoke`

### OAuth 1.0a
- `POST /oauth/request_token`
- `POST /oauth/access_token`
- `POST /oauth/authenticate`
- `POST /oauth/authorize`

### Bearer Token
- `POST /oauth2/token`
- `POST /oauth2/invalidate_token`

**Brief:** Authentication, token generation, login flows, user authorization.

---

## 2. USERS

### User Lookup
- `GET /2/users/:id`
- `GET /2/users`
- `GET /2/users/by`
- `GET /2/users/by/username/:username`
- `GET /2/users/me`

### Follows
- `GET /2/users/:id/followers`
- `GET /2/users/:id/following`
- `POST /2/users/:id/following`
- `DELETE /2/users/:id/following/:target_user_id`

### Blocks
- `GET /2/users/:id/blocking`
- `POST /2/users/:id/blocking`
- `DELETE /2/users/:id/blocking/:target_user_id`

### Mutes
- `GET /2/users/:id/muting`
- `POST /2/users/:id/muting`
- `DELETE /2/users/:id/muting/:target_user_id`

**Brief:** User profiles, relationships, follows, blocks, mutes.

---

## 3. TWEETS / POSTS

### Tweet Lookup
- `GET /2/tweets`
- `GET /2/tweets/:id`

### Tweet Creation
- `POST /2/tweets`
- `DELETE /2/tweets/:id`

### User Timelines
- `GET /2/users/:id/tweets`
- `GET /2/users/:id/mentions`

### Likes
- `GET /2/tweets/:id/liking_users`
- `GET /2/users/:id/liked_tweets`
- `POST /2/users/:id/likes`
- `DELETE /2/users/:id/likes/:tweet_id`

### Retweets
- `GET /2/tweets/:id/retweeted_by`
- `POST /2/users/:id/retweets`
- `DELETE /2/users/:id/retweets/:source_tweet_id`

### Bookmarks
- `GET /2/users/:id/bookmarks`
- `POST /2/users/:id/bookmarks`
- `DELETE /2/users/:id/bookmarks/:tweet_id`

### Quote Tweets
- `GET /2/tweets/:id/quote_tweets`

### Hidden Replies
- `PUT /2/tweets/:id/hidden`

**Brief:** Post creation, timelines, likes, retweets, replies, bookmarks, engagement.

---

## 4. SEARCH

### Recent Search
- `GET /2/tweets/search/recent`

### Full Archive Search
- `GET /2/tweets/search/all`

### Tweet Counts
- `GET /2/tweets/counts/recent`
- `GET /2/tweets/counts/all`

**Brief:** Search tweets, hashtags, trends, archived content.

---

## 5. STREAMING API

### Filtered Streams
- `GET /2/tweets/search/stream`
- `GET /2/tweets/search/stream/rules`
- `POST /2/tweets/search/stream/rules`
- `DELETE /2/tweets/search/stream/rules`

### Sample Streams
- `GET /2/tweets/sample/stream`

### Compliance Streams
- `GET /2/tweets/compliance/stream`
- `GET /2/users/compliance/stream`

**Brief:** Realtime tweets, keyword monitoring, event streams.

---

## 6. DIRECT MESSAGES

### DM Events
- `GET /2/dm_events`
- `GET /2/dm_events/:id`

### DM Conversations
- `GET /2/dm_conversations`
- `GET /2/dm_conversations/with/:participant_id`
- `GET /2/dm_conversations/:dm_conversation_id/dm_events`
- `POST /2/dm_conversations/with/:participant_id/messages`
- `POST /2/dm_conversations/:dm_conversation_id/messages`

**Brief:** Private messaging, conversations, DM events.

---

## 7. SPACES

### Spaces Lookup
- `GET /2/spaces/:id`
- `GET /2/spaces`
- `GET /2/spaces/by/creator_ids`
- `GET /2/spaces/by/search`

### Space Content
- `GET /2/spaces/:id/buyers`
- `GET /2/spaces/:id/tweets`

**Brief:** Twitter/X audio rooms and live discussions.

---

## 8. LISTS

### List Management
- `POST /2/lists`
- `GET /2/lists/:id`
- `PUT /2/lists/:id`
- `DELETE /2/lists/:id`

### List Followers
- `GET /2/lists/:id/followers`
- `POST /2/users/:id/followed_lists`
- `DELETE /2/users/:id/followed_lists/:list_id`

### List Members
- `GET /2/lists/:id/members`
- `POST /2/lists/:id/members`
- `DELETE /2/lists/:id/members/:user_id`

### User Lists
- `GET /2/users/:id/owned_lists`
- `GET /2/users/:id/list_memberships`
- `GET /2/users/:id/followed_lists`
- `GET /2/users/:id/pinned_lists`

### Tweets in Lists
- `GET /2/lists/:id/tweets`

**Brief:** Curated user groups and tweet feeds.

---

## 9. MEDIA

### Upload
- `POST /2/media/upload`

### Metadata
- `POST /1.1/media/metadata/create`

**Brief:** Image/video uploads and metadata handling.

---

## 10. COMMUNITIES

### Community Lookup
- `GET /2/communities/:id`
- `GET /2/communities/search`
- `GET /2/users/:id/communities`

**Brief:** Communities and group participation.

---

## 11. TRENDS

### Trends
- `GET /1.1/trends/place.json`
- `GET /1.1/trends/available.json`
- `GET /1.1/trends/closest.json`

**Brief:** Trending topics and locations.

---

## 12. COMPLIANCE

### Compliance Jobs
- `POST /2/compliance/jobs`
- `GET /2/compliance/jobs`
- `GET /2/compliance/jobs/:id`

**Brief:** Data compliance and deletion tracking.

---

## 13. ACCOUNT ACTIVITY API

### Webhooks
- `GET /1.1/account_activity/all/webhooks.json`
- `POST /1.1/account_activity/all/:env_name/webhooks.json`
- `DELETE /1.1/account_activity/all/:env_name/webhooks/:id.json`

### Subscriptions
- `POST /1.1/account_activity/all/:env_name/subscriptions.json`
- `GET /1.1/account_activity/all/:env_name/subscriptions/list.json`
- `DELETE /1.1/account_activity/all/:env_name/subscriptions/:user_id.json`

**Brief:** Webhook-based realtime account events.

---

## 14. ADS API

### Accounts
- `GET /accounts`

### Campaigns
- `GET /campaigns`
- `POST /campaigns`

### Line Items
- `GET /line_items`
- `POST /line_items`

### Promoted Tweets
- `GET /promoted_tweets`
- `POST /promoted_tweets`

### Analytics
- `GET /stats/accounts`
- `GET /stats/jobs/accounts`

**Brief:** Advertising, campaign management, analytics.

---

## 15. SAFETY & MODERATION

### Reported Content
- `PUT /2/tweets/:id/hidden`

**Brief:** Moderation, hidden replies, safety controls.

---

## 16. BOOKMARKS

### Bookmark Management
- `GET /2/users/:id/bookmarks`
- `POST /2/users/:id/bookmarks`
- `DELETE /2/users/:id/bookmarks/:tweet_id`

**Brief:** Saved tweets for users.

---

## 17. LIKES

### Likes Management
- `GET /2/users/:id/liked_tweets`
- `GET /2/tweets/:id/liking_users`
- `POST /2/users/:id/likes`
- `DELETE /2/users/:id/likes/:tweet_id`

**Brief:** Like interactions and engagement.

---

## 18. RETWEETS

### Retweet Management
- `GET /2/tweets/:id/retweeted_by`
- `POST /2/users/:id/retweets`
- `DELETE /2/users/:id/retweets/:source_tweet_id`

**Brief:** Retweet and repost functionality.

---

## 19. GEO & PLACE DATA

### Places
- `GET /1.1/geo/search.json`
- `GET /1.1/geo/reverse_geocode.json`

**Brief:** Location and geotagging data.

---

## 20. LABS / ENTERPRISE FEATURES

### Enterprise Search
- `GET /2/tweets/search/all`

### Enterprise Streams
- `GET /2/tweets/search/stream`

**Brief:** Advanced enterprise-level search and streams.
