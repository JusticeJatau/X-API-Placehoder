```javascript
import mongoose from 'mongoose';

const { Schema } = mongoose;

// ==============================
// Shared Subschemas
// ==============================

const entitySchema = new Schema({
  hashtags: [{
    start: Number,
    end: Number,
    tag: String,
  }],
  mentions: [{
    start: Number,
    end: Number,
    username: String,
    id: String,
  }],
  urls: [{
    start: Number,
    end: Number,
    url: String,
    expanded_url: String,
    display_url: String,
    unwound_url: String,
  }],
  cashtags: [{
    start: Number,
    end: Number,
    tag: String,
  }],
  annotations: [{
    start: Number,
    end: Number,
    probability: Number,
    type: String,
    normalized_text: String,
  }],
}, { _id: false });

const publicMetricsSchema = new Schema({
  like_count: Number,
  reply_count: Number,
  repost_count: Number,
  retweet_count: Number,
  quote_count: Number,
  bookmark_count: Number,
  impression_count: Number,
}, { _id: false });

const withheldSchema = new Schema({
  copyright: Boolean,
  country_codes: [String],
  scope: String,
}, { _id: false });

const metaSchema = new Schema({
  fetched_at: Date,
  cached: Boolean,
  source: String,
  api_version: String,
}, { _id: false });

// ==============================
// User Schema
// ==============================

const userSchema = new Schema({
  _id: { type: String, required: true },
  name: { type: String, required: true, trim: true },
  username: { type: String, required: true, unique: true, index: true, trim: true, lowercase: true },
  created_at: Date,
  description: String,
  location: String,
  url: String,
  profile_image_url: String,
  profile_banner_url: String,
  protected: Boolean,
  verified: Boolean,
  verified_type: String,
  parody: Boolean,
  is_identity_verified: Boolean,
  confirmed_email: { type: Boolean, select: false },
  most_recent_tweet_id: { type: String, ref: 'Post' },
  pinned_tweet_id: { type: String, ref: 'Post' },
  receives_your_dm: Boolean,
  subscription: Schema.Types.Mixed,
  subscription_type: String,
  verified_followers_count: Number,
  public_metrics: {
    followers_count: Number,
    following_count: Number,
    tweet_count: Number,
    listed_count: Number,
  },
  entities: {
    url: entitySchema,
    description: entitySchema,
  },
  affiliation: Schema.Types.Mixed,
  withheld: withheldSchema,
  connection_status: [{ type: String }],
  meta: metaSchema,
}, { timestamps: true });

userSchema.index({ username: 1 });
userSchema.index({ verified: 1 });

// ==============================
// Post Schema
// ==============================

const postSchema = new Schema({
  _id: { type: String, required: true },
  text: { type: String, required: true },
  edit_history_tweet_ids: { type: [String], required: true },
  author_id: { type: String, ref: 'User', index: true },
  created_at: Date,
  conversation_id: { type: String, index: true },
  in_reply_to_user_id: { type: String, ref: 'User' },
  lang: String,
  source: String,
  possibly_sensitive: Boolean,
  reply_settings: {
    type: String,
    enum: ['everyone', 'mentionedUsers', 'following', 'subscribers', 'verified'],
  },
  card_uri: String,
  community_id: { type: String, ref: 'Community' },
  display_text_range: [Number],
  attachments: {
    media_keys: [{ type: String, ref: 'Media' }],
    poll_ids: [{ type: String, ref: 'Poll' }],
    media_source_tweet: { type: String, ref: 'Post' },
  },
  public_metrics: publicMetricsSchema,
  non_public_metrics: { type: Schema.Types.Mixed, select: false },
  organic_metrics: { type: Schema.Types.Mixed, select: false },
  promoted_metrics: { type: Schema.Types.Mixed, select: false },
  entities: entitySchema,
  geo: {
    place_id: { type: String, ref: 'Place' },
    coordinates: {
      type: { type: String },
      coordinates: [Number],
    },
  },
  referenced_tweets: [{
    type: { type: String, enum: ['retweeted', 'quoted', 'replied_to'] },
    id: { type: String, ref: 'Post' },
  }],
  context_annotations: [{
    domain: { id: String, name: String, description: String },
    entity: { id: String, name: String, description: String },
  }],
  edit_controls: Schema.Types.Mixed,
  note_tweet: {
    text: String,
    entities: entitySchema,
  },
  media_metadata: [Schema.Types.Mixed],
  matched_media_notes: [Schema.Types.Mixed],
  scopes: Schema.Types.Mixed,
  suggested_source_links: [Schema.Types.Mixed],
  suggested_source_links_with_counts: [Schema.Types.Mixed],
  article: Schema.Types.Mixed,
  withheld: withheldSchema,
  meta: metaSchema,
}, { timestamps: true });

postSchema.index({ author_id: 1, created_at: -1 });
postSchema.index({ conversation_id: 1 });
postSchema.index({ created_at: -1 });

// ==============================
// Media Schema
// ==============================

const mediaSchema = new Schema({
  _id: { type: String, required: true },
  type: { type: String, required: true, enum: ['photo', 'video', 'animated_gif'] },
  alt_text: String,
  duration_ms: Number,
  height: Number,
  width: Number,
  url: String,
  preview_image_url: String,
  variants: [{
    bit_rate: Number,
    content_type: String,
    url: String,
  }],
  public_metrics: { view_count: Number },
  non_public_metrics: { type: Schema.Types.Mixed, select: false },
  organic_metrics: { type: Schema.Types.Mixed, select: false },
  promoted_metrics: { type: Schema.Types.Mixed, select: false },
  meta: metaSchema,
}, { timestamps: true });

// ==============================
// Poll Schema
// ==============================

const pollSchema = new Schema({
  _id: { type: String, required: true },
  duration_minutes: Number,
  end_datetime: Date,
  voting_status: { type: String, enum: ['open', 'closed'] },
  options: [{
    position: Number,
    label: String,
    votes: Number,
  }],
  meta: metaSchema,
}, { timestamps: true });

// ==============================
// Place Schema
// ==============================

const placeSchema = new Schema({
  _id: { type: String, required: true },
  full_name: String,
  name: String,
  country: String,
  country_code: String,
  place_type: String,
  contained_within: [{ type: String, ref: 'Place' }],
  geo: {
    type: { type: String },
    bbox: [Number],
    properties: Schema.Types.Mixed,
    geometry: Schema.Types.Mixed,
  },
  meta: metaSchema,
}, { timestamps: true });

// ==============================
// Space Schema
// ==============================

const spaceSchema = new Schema({
  _id: { type: String, required: true },
  state: { type: String, required: true },
  title: String,
  created_at: Date,
  updated_at: Date,
  creator_id: { type: String, ref: 'User' },
  host_ids: [{ type: String, ref: 'User' }],
  speaker_ids: [{ type: String, ref: 'User' }],
  invited_user_ids: [{ type: String, ref: 'User' }],
  topic_ids: [{ type: String, ref: 'Topic' }],
  topics: [{
    id: String,
    name: String,
    description: String,
  }],
  lang: String,
  is_ticketed: Boolean,
  participant_count: Number,
  subscriber_count: Number,
  scheduled_start: Date,
  started_at: Date,
  ended_at: Date,
  meta: metaSchema,
}, { timestamps: true });

// ==============================
// List Schema
// ==============================

const listSchema = new Schema({
  _id: { type: String, required: true },
  name: { type: String, required: true },
  description: String,
  created_at: Date,
  follower_count: Number,
  member_count: Number,
  owner_id: { type: String, ref: 'User' },
  private: Boolean,
  meta: metaSchema,
}, { timestamps: true });

// ==============================
// Community Schema
// ==============================

const communitySchema = new Schema({
  _id: { type: String, required: true },
  name: String,
  description: String,
  created_at: Date,
  access: String,
  join_policy: String,
  member_count: Number,
  meta: metaSchema,
}, { timestamps: true });

// ==============================
// DmEvent Schema
// ==============================

const dmEventSchema = new Schema({
  _id: { type: String, required: true },
  event_type: String,
  dm_conversation_id: { type: String, index: true },
  created_at: Date,
  sender_id: { type: String, ref: 'User' },
  participant_ids: [{ type: String, ref: 'User' }],
  text: String,
  attachments: {
    media_keys: [{ type: String, ref: 'Media' }],
  },
  entities: entitySchema,
  referenced_tweets: [{
    id: { type: String, ref: 'Post' },
    type: String,
  }],
  meta: metaSchema,
}, { timestamps: true });

// ==============================
// ComplianceJob Schema
// ==============================

const complianceJobSchema = new Schema({
  _id: { type: String, required: true },
  type: { type: String, required: true, enum: ['tweets', 'users'] },
  name: String,
  status: { type: String, enum: ['created', 'in_progress', 'failed', 'complete'] },
  resumable: Boolean,
  created_at: Date,
  upload_expires_at: Date,
  upload_url: String,
  download_expires_at: Date,
  download_url: String,
  meta: metaSchema,
}, { timestamps: true });

// ==============================
// Topic Schema
// ==============================

const topicSchema = new Schema({
  _id: { type: String, required: true },
  name: { type: String, required: true },
  description: String,
  meta: metaSchema,
}, { timestamps: true });

// ==============================
// Trend Schema
// ==============================

const trendSchema = new Schema({
  trend_name: { type: String, required: true, index: true },
  tweet_count: Number,
  location: String,
  as_of: Date,
  meta: metaSchema,
}, { timestamps: true });
