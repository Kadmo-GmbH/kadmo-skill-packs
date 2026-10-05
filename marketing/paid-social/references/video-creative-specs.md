# Video Creative Specs & Performance Data

Video ad format specifications, performance benchmarks, and creative testing frameworks for paid social and YouTube.

## Platform Format Comparison

| Format | Platform | Length | Skip | Aspect | Billing |
|--------|----------|--------|------|--------|---------|
| **Skippable in-stream** | YouTube | 12s-3min (rec 15-60s) | After 5s | 16:9 | CPV (30s or full) |
| **Non-skippable in-stream** | YouTube | 15-20s | No | 16:9 | CPM |
| **Bumper** | YouTube | 6s max | No | 16:9 | CPM ($3-4 CPM) |
| **YouTube Shorts** | YouTube | 10-60s | Swipe | 9:16 | CPV/CPM (~$4 CPM) |
| **In-feed video** | Meta | 1s-240min (rec <15s) | Scroll past | 1:1, 4:5, 9:16 | CPM/ThruPlay |
| **Reels** | Meta | 3-90s | Swipe | 9:16 | CPM |
| **In-feed video** | TikTok | 5-60s (rec 15-30s) | Scroll past | 9:16 | CPM |
| **Spark Ads** | TikTok | Any (organic boosted) | Scroll past | 9:16 | CPM |
| **Sponsored video** | LinkedIn | 3s-30min (rec 15-30s) | Scroll past | 16:9, 1:1 | CPM/CPV |

## "View" Definition by Platform

| Platform | What Counts as a View | Billing Trigger |
|----------|----------------------|-----------------|
| **YouTube** | 30 seconds watched (or full if shorter), or interaction | CPV after view event |
| **Meta** | 3 seconds | ThruPlay = 15s or full |
| **TikTok** | Immediate on play start | CPM |
| **LinkedIn** | 2 seconds at 50% pixels | CPV |

## Performance Benchmarks

### YouTube by Format

| Format | CPM | View Rate | Avg CPV |
|--------|-----|-----------|---------|
| Skippable in-stream | $5-10 | 29-66% | $0.02-0.05 |
| Non-skippable | $6-10 | ~100% | N/A (CPM) |
| Bumper (6s) | $3-4 | ~100% | N/A (CPM) |
| YouTube Shorts | ~$4 | N/A | ~$0.02-0.04 |
| Connected TV | $9-10 | ~100% | N/A (CPM) |

### Completion Rate by Duration

| Duration | Completion Rate |
|----------|----------------|
| Under 15 seconds | 79.4% |
| 15-30 seconds | ~70% |
| 45-59 seconds | 74.8% |
| 1-2 minutes | 56% |
| 30+ seconds (all) | 51.8% |

### Completion Rate by Device

| Device | Completion Rate |
|--------|----------------|
| CTV (non-skippable) | 97.2% |
| Tablet | 91.4% |
| Desktop | 83.7% |
| Mobile | 82.1% |

### Format Performance Multipliers

| Factor | Impact |
|--------|--------|
| Vertical (9:16) vs horizontal on mobile | +33.8% completion |
| Captions ON vs OFF | 18.4% skip rate vs 41.3% |
| 6-10s micro-ads | 68.7% unaided brand recall (highest) |
| Sound ON (TikTok) | +12% conversion rate |
| CTA display cards (TikTok) | +45% recall, +30% conversion |

## Short-Form Video Creative Framework

### Hook Rules (First 3 Seconds)

The first 3 seconds determine everything. 90% of ad recall is captured in the first 6 seconds.

**Hook types ranked by effectiveness:**
1. **Visual pattern interrupt** — unexpected visual that breaks the scroll
2. **Direct address** — "You need to see this" / "[Audience], this is for you"
3. **Problem statement** — Name the pain immediately
4. **Contrarian claim** — Challenge common belief
5. **Stat/number** — Lead with a surprising data point
6. **Before/after** — Show transformation in first frame

**Hook benchmarks:**
- Thumb-stop rate target: >25% (TikTok)
- Hook-to-hold rate target: >35-40% (viewers who watch past hook)
- 3-second view rate target: >50%

### UGC vs Polished Creative

| Metric | UGC Style | Polished/Branded |
|--------|-----------|-----------------|
| CTR | Higher (+20-35%) | Lower |
| Conversion rate | Comparable or higher | Comparable |
| CPM | Lower (feels organic) | Higher (flagged as ad) |
| Brand recall | Lower | Higher |
| Best for | Direct response, TikTok, Reels | Brand awareness, YouTube, LinkedIn |

UGC outperforms polished for direct response on TikTok and Reels. Polished outperforms for brand campaigns on YouTube and LinkedIn.

### Modular Video Testing Framework

Test video creative by isolating components, not replacing entire videos:

| Component | What to Test | Priority |
|-----------|-------------|----------|
| **Hook** (0-3s) | Different opening frame/line | Highest — gates everything |
| **Body** (3-15s) | Value proposition, demo, testimonial | High |
| **CTA** (last 3-5s) | Offer, urgency, button text | High |
| **Music/sound** | Trending audio vs original vs voiceover | Medium |
| **Format** | Static opener vs motion vs face-to-camera | Medium |
| **Length** | 15s vs 30s vs 60s | Test after hook wins |

**Process:**
1. Create 3-5 hook variants with same body/CTA
2. Run 3-5 days, kill lowest CTR hooks
3. Take winning hook, test 3 body variants
4. Take winning body, test 3 CTA variants
5. Final winner = optimized modular video

### Optimal Length by Platform and Objective

| Platform | Awareness | Consideration | Conversion |
|----------|-----------|---------------|------------|
| YouTube | 15-20s (bumper/non-skip) | 30-60s (skippable) | 15-30s (action) |
| TikTok | 9-15s | 15-30s | 15-30s |
| Meta Reels | 6-15s | 15-30s | 15-30s |
| Meta Feed | 6-15s | 15-30s | 15-60s |
| LinkedIn | 15-30s | 30-90s | 15-30s |

TikTok data: Videos 15+ seconds perform +45% better than shorter. Voiceover adds +12% conversion.

## Video Measurement KPIs

| KPI | Formula | Use |
|-----|---------|-----|
| **VTR** (View-Through Rate) | Completed views / Impressions × 100 | Overall video engagement |
| **VCR** (Video Completion Rate) | Complete views / Impressions × 100 | Full-watch measurement |
| **CPV** (Cost Per View) | Spend / Views | Cost efficiency |
| **CPCV** (Cost Per Completed View) | Spend / Completed views | Full-watch cost efficiency |
| **ThruPlay Rate** (Meta) | ThruPlays / Impressions × 100 | Meta-specific 15s view |

**Benchmark targets:** VCR >70%, CPV <$0.05 (YouTube), ThruPlay rate >15% (Meta).

## Attribution Windows for Video

| Platform | Click-Through | View-Through | Engaged-View |
|----------|--------------|--------------|--------------|
| Google/YouTube | 30 days | 1 day | 3 days (5s+ watched) |
| Meta | 7 days | 1 day | 1 day (10s+ watched) |
| TikTok | 7 days | 1 day | 7 days (6s+ watched) |
| LinkedIn | 30 days | 7 days | N/A |

View-through attribution inflates video's reported impact. Use conservative windows (1-day VTA max) or discount view-throughs when comparing to search/social click-based channels.
