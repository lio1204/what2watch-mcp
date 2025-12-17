# What2Watch MCP

**Give Your AI Superpowers for Entertainment Discovery**

[![MCP Compatible](https://img.shields.io/badge/MCP-Compatible-blue)](https://modelcontextprotocol.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Live Server](https://img.shields.io/badge/Server-Live-brightgreen)](https://what2watch.live)

---

## Stop Getting Generic Recommendations. Start Discovering What's *Actually On*.

Regular AI assistants search the web and return the same "trending on Netflix" lists from 2023. **What2Watch MCP** plugs directly into your AI, giving it access to:

- **Real-time TV schedules** for 1000+ channels (ABC, CBS, NBC, FOX, ESPN, CNN, HBO, AMC...)
- **Free streaming (FAST)** channels (Pluto TV, Tubi, Freevee, Roku Channel, Peacock Free, Samsung TV+, Xumo)
- **All major SVOD services** (Netflix, Prime Video, Disney+, Hulu, HBO Max, Apple TV+, Paramount+)
- **Truly personalized recommendations** based on your mood, taste, and available time
- **"Surprise Me" mode** to discover hidden gems you'd never find on your own

**Real data. Real-time. Really personalized.**

---

## Quick Start

### Connect via Claude

1. Open Claude → Settings → Connectors
2. Add Custom Connector: `https://what2watch.live/sse`
3. Authenticate with email magic link
4. Ask: *"What should I watch tonight?"*

### Connect via ChatGPT

1. Go to [chatgpt.com](https://chatgpt.com) → Settings → Apps & Connectors
2. Enable Developer Mode in Advanced Settings
3. Create App with URL: `https://what2watch.live/sse`
4. Authenticate and start discovering!

---

## Tools

What2Watch MCP provides **6 powerful tools** for entertainment discovery.

### `get_recommendation`

Get personalized movie and TV recommendations based on your preferences.

```typescript
interface GetRecommendationParams {
  // Location & Language
  location?: string;              // Country code (default: "US")
  languages?: string[];           // Preferred languages ["en", "es"]

  // User Profile
  gender?: string;                // User's gender
  age_group?: string;             // "kids" | "teenager" | "young adult" | "adult" | "senior"

  // Platforms
  platforms?: string[];           // ["netflix", "prime", "disney_plus", "hulu", "hbo_max", "apple_tv_plus"]
  channels?: string[];            // Cable/linear TV channels

  // Taste Profile
  liked_media?: string[];         // Titles they enjoyed ["Breaking Bad", "The Wire"]
  disliked_media?: string[];      // Titles to avoid
  watched_media?: string[];       // Already seen (excluded from results)

  // Filters
  media_type?: string;            // "movie" | "series" | "documentary"
  genre?: string;                 // "comedy" | "drama" | "action" | "thriller" | "sci-fi" | "horror" | "romance"
  mood?: string;                  // "relaxing" | "exciting" | "thought-provoking"
  watch_time?: string;            // "now" | "tonight" | "later"

  // Discovery
  surprise_me?: boolean;          // Discover underrated hidden gems (default: false)
  limit?: number;                 // Max results 1-10 (default: 5)
}
```

**Example queries:**
- *"What should I watch?"*
- *"Recommend something like Breaking Bad"*
- *"I'm in the mood for a relaxing comedy"*
- *"Surprise me with hidden gems"*

---

### `search_content`

Search for movies and shows by title, actor, director, or keywords.

```typescript
interface SearchContentParams {
  keywords?: string;              // Text search (title, actor name, plot keywords)
  genres?: string[];              // Filter by multiple genres
  min_rating?: number;            // Minimum IMDB score 0-10 (e.g., 7.5)
  year_from?: number;             // Release year start (e.g., 2020)
  year_to?: number;               // Release year end (e.g., 2024)
  location?: string;              // Country code (default: "US")
  languages?: string[];           // Preferred languages
  platforms?: string[];           // Streaming services filter
  liked_media?: string[];         // Boosts similar content in ranking
  disliked_media?: string[];      // Titles to avoid
  watched_media?: string[];       // Already seen (excluded)
  media_type?: string;            // "movie" | "series" | "episode" | "documentary"
  genre?: string;                 // Single genre filter
  mood?: string;                  // "relaxing" | "exciting" | "thought-provoking"
  limit?: number;                 // Max results 1-10 (default: 5)
}
```

**Example queries:**
- *"Find Tom Hanks movies"*
- *"Action movies on Netflix rated above 8"*
- *"Sci-fi shows from 2020-2024"*
- *"Search for Dune"*

---

### `get_content_details`

Get full details about a specific movie or TV show.

```typescript
interface GetContentDetailsParams {
  content_id: string;             // The ID from search/recommendation results
}
```

**Returns:** Full details including plot, cast, crew, ratings, trailers, and streaming availability.

**Example queries:**
- *"Tell me more about Inception"*
- *"What's The Bear about?"*
- *"Who stars in Severance?"*
- *"Where can I watch Oppenheimer?"*

---

### `get_schedule`

Get TV broadcast schedule - what's on live TV right now or later.

```typescript
interface GetScheduleParams {
  time_range?: string;            // "now" (next 2h) | "tonight" (6PM-midnight) | "today" | "tomorrow"
  channels?: string[];            // Filter by channel ["hbo", "nbc", "espn", "abc", "cbs", "fox"]
  location?: string;              // Country code (default: "US")
  liked_media?: string[];         // Boosts similar content
  disliked_media?: string[];      // Titles to avoid (excluded)
  watched_media?: string[];       // Already seen (excluded)
  media_type?: string;            // "movie" | "series" | "sports" | "news" | "documentary"
  genre?: string;                 // "drama" | "comedy" | "sports" | "news"
  genres?: string[];              // Multiple genres ["sports", "action"]
  min_rating?: number;            // Minimum IMDB score 0-10
  limit?: number;                 // Max results 1-10 (default: 5)
}
```

**Example queries:**
- *"What's on TV tonight?"*
- *"What's on ESPN right now?"*
- *"Movies on HBO tonight"*
- *"Sports on TV tomorrow"*

---

### `get_trending`

See what's popular and trending right now.

```typescript
interface GetTrendingParams {
  location?: string;              // Country code (default: "US")
  platforms?: string[];           // Filter by streaming service
  liked_media?: string[];         // For reference/personalization
  disliked_media?: string[];      // Titles to avoid (excluded)
  watched_media?: string[];       // Already seen (excluded)
  media_type?: string;            // "movie" | "series"
  genre?: string;                 // Filter by genre
  time_window?: string;           // "day" | "week" | "month" (default: all time)
  limit?: number;                 // Max results 1-10 (default: 5)
}
```

**Example queries:**
- *"What's trending?"*
- *"What's popular on Netflix?"*
- *"Top movies this week"*
- *"What is everyone watching?"*

---

### `get_available_sources`

List all streaming platforms and TV channels supported.

```typescript
interface GetAvailableSourcesParams {
  location?: string;              // Country code (default: "US")
  include_channels?: boolean;     // Include TV channels with schedules (default: true)
  include_platforms?: boolean;    // Include streaming platforms (default: true)
}
```

**Returns:** Platforms grouped by type (subscription, free, rent, purchase) with content counts; TV channels with schedule availability.

**Example queries:**
- *"What platforms do you support?"*
- *"What streaming services are available?"*
- *"Which channels have TV schedules?"*

---

## Prompts

What2Watch MCP provides **19 pre-built prompts** for common entertainment queries.

### Discovery Prompts

| Prompt | Parameters | Description |
|--------|------------|-------------|
| `what_to_watch` | `mood?`, `platforms?`, `media_type?` | The go-to prompt for "I don't know what to watch" |
| `surprise_me` | `genre?` | Discover hidden gems and underrated content |
| `recommend_by_mood` | `mood` (required) | Match your current mood (relaxing, exciting, funny, dark...) |
| `similar_to` | `title` (required), `media_type?` | Find content similar to something you loved |

### Search Prompts

| Prompt | Parameters | Description |
|--------|------------|-------------|
| `find_title` | `title` (required) | Search for a specific movie or show |
| `find_by_actor` | `actor_name` (required), `media_type?` | Find content featuring a specific actor |
| `find_by_genre` | `genre` (required), `platforms?` | Find content in a specific genre |
| `recent_releases` | `media_type?`, `genre?`, `min_rating?` | Find recent releases from the last year |

### Trending Prompts

| Prompt | Parameters | Description |
|--------|------------|-------------|
| `whats_trending` | `media_type?` | See what's popular right now |
| `trending_on_platform` | `platform` (required) | What's popular on a specific service |
| `top_rated` | `genre?`, `media_type?` | Find the highest-rated content |

### TV Schedule Prompts

| Prompt | Parameters | Description |
|--------|------------|-------------|
| `whats_on_tv` | `time_range?` | Check what's on live TV |
| `whats_on_channel` | `channel` (required), `time_range?` | Schedule for a specific channel |

### Occasion-Based Prompts

| Prompt | Parameters | Description |
|--------|------------|-------------|
| `movie_night` | `group?`, `platforms?` | Recommendations for movie night (friends, family, date) |
| `family_friendly` | `platforms?` | Kid-friendly content for the whole family |
| `date_night` | `platforms?` | Romantic or date-appropriate picks |
| `weekend_binge` | `genre?`, `platforms?` | Great series to binge over the weekend |
| `quick_watch` | `max_minutes?` | Short content when you're limited on time |

### Platform Prompts

| Prompt | Parameters | Description |
|--------|------------|-------------|
| `available_platforms` | - | List all supported streaming services and channels |
| `compare_platforms` | `title` (required) | Find where a specific title is available |

---

## Example Conversations

### Finding Something to Watch

> **User:** I'm in the mood for something dark and psychological, maybe like True Detective

> **AI:** *Uses `get_recommendation` with mood="thought-provoking", liked_media=["True Detective"]*

> Based on your love of True Detective, here are some dark psychological picks:
> 1. **Mindhunter** (Netflix) - 8.6/10 - FBI agents interview serial killers
> 2. **Sharp Objects** (HBO Max) - 8.1/10 - Reporter returns to her hometown
> 3. **The Killing** (Hulu) - 8.3/10 - Detectives investigate a murder
> ...

### Checking Live TV

> **User:** What's on ESPN tonight?

> **AI:** *Uses `get_schedule` with channels=["espn"], time_range="tonight"*

> Here's what's on ESPN tonight:
> - 7:00 PM - **NBA Basketball: Lakers vs Celtics**
> - 10:00 PM - **SportsCenter**
> - 11:00 PM - **NFL Live**
> ...

### Discovering Hidden Gems

> **User:** Surprise me with something I've probably never heard of

> **AI:** *Uses `get_recommendation` with surprise_me=true*

> Here are some hidden gems most people haven't discovered:
> 1. **The Bear** (Hulu) - 8.6/10 - A chef returns to run his family's sandwich shop
> 2. **Severance** (Apple TV+) - 8.7/10 - Office workers' memories are surgically divided
> ...

---

## Response Format

All tools return structured responses with:

```typescript
interface ToolResponse {
  results: ContentItem[];         // Array of movies/shows
  total_count: number;            // Total matches found
  filters_applied: object;        // Which filters were used
  next_steps: string[];           // Contextual follow-up suggestions
}

interface ContentItem {
  id: string;                     // Unique identifier
  title: string;                  // Display title
  type: "movie" | "series";       // Content type
  year: number;                   // Release year
  rating: number;                 // IMDB rating 0-10
  genres: string[];               // Genre tags
  plot: string;                   // Brief description
  runtime?: number;               // Minutes (for movies)
  seasons?: number;               // Season count (for series)
  availability: Availability[];   // Where to watch
}

interface Availability {
  platform: string;               // "netflix", "hulu", etc.
  platform_name: string;          // "Netflix", "Hulu", etc.
  type: "subscription" | "free" | "rent" | "purchase";
  link?: string;                  // Direct watch link
  price?: number;                 // For rent/purchase
}
```

---

## Supported Platforms

### Streaming Services (SVOD)
Netflix, Amazon Prime Video, Disney+, Hulu, HBO Max, Apple TV+, Paramount+, Peacock, AMC+, Starz, Showtime, Britbox, Crunchyroll, and more.

### Free Streaming (FAST)
Pluto TV, Tubi, Amazon Freevee, The Roku Channel, Peacock Free, Samsung TV Plus, Xumo, Plex, and more.

### Live TV Channels
ABC, CBS, NBC, FOX, ESPN, ESPN2, CNN, MSNBC, Fox News, HBO, Showtime, AMC, FX, TNT, TBS, USA, Bravo, HGTV, Food Network, Discovery, History, A&E, Lifetime, Comedy Central, MTV, VH1, BET, Nickelodeon, Disney Channel, Cartoon Network, and 1000+ more.

---

## Technical Details

- **Protocol:** [Model Context Protocol (MCP)](https://modelcontextprotocol.io)
- **Transport:** Server-Sent Events (SSE)
- **Authentication:** OAuth 2.1 with PKCE
- **Endpoint:** `https://what2watch.live/sse`

---

## License

MIT License - see [LICENSE](LICENSE) for details.

---

## Links

- **Live Server:** https://what2watch.live
- **MCP Specification:** https://modelcontextprotocol.io

---

*Making AI Actually Useful for Entertainment*
