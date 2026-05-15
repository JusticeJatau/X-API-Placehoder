# X API Models — Structured Data Dictionary

## Object Description

| Object | Description |
|--------|-------------|
| **User** | Profile metadata, metrics, verification, and relationship status |
| **Post** | Core content unit with text, metrics, attachments, and reply chain info |
| **Media** | Images, videos, GIFs with metadata and view counts |
| **Poll** | Poll questions, options, duration, and vote counts |
| **Place** | Geographic location with bounding box and place hierarchy |
| **Space** | Live or scheduled audio conversations |
| **List** | Curated collections of users |
| **Community** | Group spaces for topic‑based discussions |
| **DmEvent** | Direct message events with text and media attachments |
| **ComplianceJob** | Batch compliance data export jobs |
| **Topic** | Topic entity used in Space expansions |
| **Trend** | Hashtag trend with tweet volume |

---

## 1. User

**Default fields:** `id` (string), `name` (string), `username` (string)

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Unique user identifier |
| `name` | string | Yes | Display name (up to ~50 chars) |
| `username` | string | Yes | Handle without @ (max 15 chars) |
| `created_at` | string (ISO 8601) | No | Account creation timestamp |
| `description` | string | No | Profile bio |
| `location` | string | No | Free‑form location string |
| `url` | string | No | URL from profile |
| `profile_image_url` | string | No | Profile image URL |
| `profile_banner_url` | string | No | Banner image URL |
| `protected` | boolean | No | Private account flag |
| `verified` | boolean | No | Verified account flag |
| `verified_type` | string | No | `blue`, `business`, `government` |
| `parody` | boolean | No | Parody label indicator |
| `is_identity_verified` | boolean | No | ID verification status |
| `confirmed_email` | string | No | Authenticated user’s confirmed email |
| `most_recent_tweet_id` | string | No | Latest Tweet ID |
| `pinned_tweet_id` | string | No | Pinned Tweet ID |
| `receives_your_dm` | boolean | No | Accepts DM from authenticated user |
| `subscription` | object | No | Premium subscription details |
| `subscription_type` | string | No | `None`, `Basic`, `Premium`, `PremiumPlus` |
| `verified_followers_count` | number | No | Count of verified followers |
| `public_metrics` | object | No | `followers_count`, `following_count`, `tweet_count`, `listed_count` |
| `entities` | object | No | URL/hashtag/mention entities in description |
| `affiliation` | object | No | Affiliate badge details |
| `withheld` | object | No | Country withholding details |
| `connection_status` | array | No | Relationship with auth user |

---

## 2. Post

**Default fields:** `id` (string), `text` (string), `edit_history_tweet_ids` (string[])

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Unique Tweet identifier |
| `text` | string | Yes | Tweet content (up to 280 chars, longer for notes) |
| `author_id` | string | No | User ID of the author |
| `created_at` | string (ISO 8601) | No | Creation timestamp |
| `conversation_id` | string | No | Root Tweet ID of the conversation |
| `in_reply_to_user_id` | string | No | User ID being replied to |
| `lang` | string | No | BCP47 language code |
| `source` | string | No | Client/app name |
| `possibly_sensitive` | boolean | No | Potentially sensitive content flag |
| `reply_settings` | string | No | `everyone`, `mentioned_users`, `followers` |
| `card_uri` | string | No | Card URI |
| `community_id` | string | No | Community ID |
| `display_text_range` | [number, number] | No | Start and end indices of displayed text |
| `edit_history_tweet_ids` | string[] | Yes | IDs of all versions (original + edits) |
| `attachments` | object | No | `media_keys`, `poll_ids`, `media_source_tweet` |
| `public_metrics` | object | No | `retweet_count`, `reply_count`, `like_count`, `quote_count`, `impression_count`, `bookmark_count` |
| `non_public_metrics` | object | No | Requires user context auth |
| `organic_metrics` | object | No | Organic engagement metrics |
| `promoted_metrics` | object | No | Promoted engagement metrics |
| `entities` | object | No | URLs, mentions, hashtags, cashtags, annotations |
| `geo` | object | No | Geo‑tag location or place |
| `referenced_tweets` | array | No | `{ type, id }` (retweeted, quoted, replied_to) |
| `context_annotations` | array | No | Domain/entity pairs |
| `edit_controls` | object | No | Edit eligibility info |
| `note_tweet` | object | No | Long‑form post text (>280 chars) |
| `media_metadata` | array | No | Alt‑text etc. for media attachments |
| `matched_media_notes` | array | No | Notes for matched media |
| `scopes` | object | No | Visibility scope (promoted posts) |
| `suggested_source_links` | array | No | Suggested link metadata |
| `suggested_source_links_with_counts` | array | No | Suggested links with counts |
| `article` | object | No | Article metadata |
| `withheld` | object | No | Country withholding details |

---

## 3. Media

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `media_key` | string | Yes | Unique media identifier |
| `type` | string | Yes | `photo`, `video`, `animated_gif` |
| `alt_text` | string | No | Accessibility description |
| `duration_ms` | number | No | Video duration in milliseconds |
| `height` | number | No | Pixel height |
| `width` | number | No | Pixel width |
| `url` | string | No | Media URL |
| `preview_image_url` | string | No | Thumbnail preview URL |
| `variants` | array | No | Video/GIF variants with `bit_rate`, `content_type`, `url` |
| `public_metrics` | object | No | `view_count` (video) |
| `non_public_metrics` | object | No | Non‑public metrics |
| `organic_metrics` | object | No | Organic metrics |
| `promoted_metrics` | object | No | Promoted metrics |

---

## 4. Poll

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Unique poll identifier |
| `duration_minutes` | number | No | Length of poll in minutes |
| `end_datetime` | string (ISO 8601) | No | Poll end timestamp |
| `voting_status` | string | No | `open` or `closed` |
| `options` | array | No | Array of `{ label, position, votes }` |

---

## 5. Place

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Place identifier |
| `full_name` | string | No | Full place name (e.g., "Brooklyn, NY") |
| `name` | string | No | Short place name |
| `country` | string | No | Country name |
| `country_code` | string | No | ISO country code |
| `place_type` | string | No | `city`, `neighborhood`, `poi`, etc. |
| `contained_within` | string[] | No | Parent place IDs |
| `geo` | object | No | GeoJSON‑like object with `type`, `bbox`, `properties`, `geometry` |

---

## 6. Space

**Default fields:** `id` (string), `state` (`live`, `scheduled`, `ended`)

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Unique Space identifier |
| `state` | enum | Yes | `live`, `scheduled`, `ended` |
| `title` | string | No | Space title |
| `created_at` | string (ISO 8601) | No | Creation timestamp |
| `updated_at` | string (ISO 8601) | No | Last update timestamp |
| `creator_id` | string | No | User ID of the creator |
| `host_ids` | string[] | No | Host user IDs |
| `speaker_ids` | string[] | No | Speaker user IDs |
| `invited_user_ids` | string[] | No | Invited user IDs |
| `topic_ids` | string[] | No | Topic IDs |
| `topics` | array | No | `[{ id, name, description }]` |
| `lang` | string | No | Language code |
| `is_ticketed` | boolean | No | Ticketed Space flag |
| `participant_count` | number | No | Current participants |
| `subscriber_count` | number | No | Subscriber count |
| `scheduled_start` | string (ISO 8601) | No | Scheduled start time |
| `started_at` | string (ISO 8601) | No | Actual start time |
| `ended_at` | string (ISO 8601) | No | End time |

---

## 7. List

**Default fields:** `id`, `name`

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Unique list identifier |
| `name` | string | Yes? | List name |
| `description` | string | No | Description |
| `created_at` | string (ISO 8601) | No | Creation timestamp |
| `follower_count` | number | No | Follower count |
| `member_count` | number | No | Member count |
| `owner_id` | string | No | Owner user ID |
| `private` | boolean | No | Private list flag |

---

## 8. Community

**Default fields:** `id`, `name`, `description`, `created_at`, `member_count`, `access`, `join_policy`

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Unique community identifier |
| `name` | string | Yes? | Community name |
| `description` | string | No | Description |
| `created_at` | string (ISO 8601) | No | Creation timestamp |
| `access` | string | No | Access level |
| `join_policy` | string | No | Join policy |
| `member_count` | number | No | Member count |

---

## 9. DmEvent

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Unique DM event identifier |
| `event_type` | string | No | `MessageCreate`, `ParticipantsJoin`, etc. |
| `dm_conversation_id` | string | No | Conversation identifier |
| `created_at` | string (ISO 8601) | No | Timestamp |
| `sender_id` | string | No | Sender user ID |
| `participant_ids` | string[] | No | Participant user IDs |
| `text` | string | No | Message text |
| `attachments` | object | No | `media_keys` array |
| `entities` | object | No | URLs, mentions, hashtags |
| `referenced_tweets` | array | No | `{ id, type }` |

---

## 10. ComplianceJob

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Unique job identifier |
| `type` | enum | Yes | `tweets` or `users` |
| `name` | string | No | Job name |
| `status` | enum | No | `created`, `in_progress`, `failed`, `complete` |
| `resumable` | boolean | No | Resumable flag |
| `created_at` | string (ISO 8601) | No | Creation timestamp |
| `upload_expires_at` | string (ISO 8601) | No | Upload URL expiry |
| `upload_url` | string | No | Upload URL |
| `download_expires_at` | string (ISO 8601) | No | Download URL expiry |
| `download_url` | string | No | Download URL |

---

## 11. Topic

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Unique topic identifier |
| `name` | string | Yes | Topic name |
| `description` | string | No | Topic description |

---

## 12. Trend

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `trend_name` | string | Yes | Hashtag or trending phrase |
| `tweet_count` | number | No | Number of tweets (if available) |

---

# MongoDB / Mongoose Schema Recommendations

## General Principles

| Principle | Details |
|-----------|---------|
| **Default fields required** | `id`, `name`, `username` for User; `id`, `text`, `edit_history_tweet_ids` for Post; other objects follow their API defaults |
| **Use `_id` as the X API id** | Set `_id: id` to avoid duplicate indexes |
| **Store expansions in separate collections** | Media, Poll, Place, etc. – reference by key |
| **Index on `author_id`** | Essential for timeline queries |
| **Index on `conversation_id`** | For reply threading |
| **Use embedded documents sparingly** | Metrics objects, entities, attachments are small and accessed together → embed |
| **Normalize high‑cardinality references** | Users, Posts, Media → separate collections |
| **Use `timestamps: true`** | Mongoose option for `createdAt`/`updatedAt` |
| **Leverage `select: false`** | Hide large fields unless requested |

## Example Reference Relationships

| Source Field | Target |
|--------------|--------|
| `Post.author_id` | User |
| `Post.referenced_tweets[].id` | Post |
| `Post.attachments.media_keys` | Media |
| `Post.attachments.poll_ids` | Poll |
| `Post.geo.place_id` | Place |
| `User.pinned_tweet_id` | Post |
| `Space.creator_id`, `host_ids`, `speaker_ids` | User |
| `List.owner_id` | User |

## Mongoose Schema Example (User)

```javascript
const userSchema = new mongoose.Schema({
  _id: { type: String, required: true }, // X API id
  name: { type: String, required: true },
  username: { type: String, required: true, index: true },
  created_at: Date,
  description: String,
  location: String,
  url: String,
  profile_image_url: String,
  profile_banner_url: String,
  protected: Boolean,
  verified: Boolean,
  verified_type: { type: String, enum: ['blue', 'business', 'government'] },
  parody: Boolean,
  is_identity_verified: Boolean,
  confirmed_email: { type: String, select: false },
  most_recent_tweet_id: String,
  pinned_tweet_id: { type: String, ref: 'Post' },
  receives_your_dm: Boolean,
  subscription: {
    type: {
      // ... subscription fields
    },
    required: false,
  },
  subscription_type: { type: String, enum: ['None', 'Basic', 'Premium', 'PremiumPlus'] },
  verified_followers_count: Number,
  public_metrics: {
    followers_count: Number,
    following_count: Number,
    tweet_count: Number,
    listed_count: Number,
  },
  entities: {
    url: { urls: [Object] },
    description: { urls: [Object], hashtags: [Object], mentions: [Object] },
  },
  affiliation: Object,
  withheld: { copyright: Boolean, country_codes: [String] },
  connection_status: [String],
}, { timestamps: true, _id: false });

userSchema.index({ username: 1 });
userSchema.index({ pinned_tweet_id: 1 });
