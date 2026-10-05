---
name: YouTube Music
domain: music.youtube.com
match:
  - music.youtube.com
tags: [music, personal, migration]
---

# YouTube Music

Playlist extraction and migration to other platforms. Agents drive the `music.youtube.com` web app; the YouTube Music mobile app and YouTube proper are out of scope. Most automation work targets Library and Playlist surfaces — the algorithmic Home is rarely useful for deterministic flows.

## Browser Access

Requires login to a Google account in the connected browser. YouTube Music does not require a paid Premium subscription for read flows, but private playlists, Library access, and uploaded music all require the user to be signed in. Sessions persist as long as Google cookies are kept.

## Layout overview

| Region | Contains |
|---|---|
| Top bar | Search, cast, account menu |
| Left sidebar | Home, Samples, Explore, Library |
| Main view | Whatever surface was opened (playlist, album, artist, search) |
| Bottom bar | Now-playing track, playback controls, volume, mini-player, queue button |

The bottom bar is fixed and survives navigation. The expanded queue and full-screen player overlay the main view rather than replacing it.

## Library

`Library` (left sidebar) holds: Playlists, Songs, Albums, Artists, Subscriptions, Episodes, Podcasts. Library lists are paginated and sortable by Recent activity, Recently added, or Alphabetical. The `Liked songs` auto-playlist is the canonical export target for "everything this account has liked."

## Playlists

Playlists open at `/playlist?list=<PLAYLIST_ID>`. The track grid is virtualized — only on-screen rows render. Each row exposes the title, primary artist, album, duration, and an overflow `…` menu with Play next, Add to queue, Save to library, Add to playlist, Share, and Go to album.

Playlist IDs starting with `PL` are user-created. IDs starting with `RD` are radio playlists generated from a seed track. IDs starting with `OL` are owned by YouTube Music itself (mood, decade, charts). Treat each differently — only `PL` lists are safe to mutate.

## Now-playing queue

The queue button on the bottom bar opens the upcoming track list. The queue is mutable: drag rows to reorder, click `…` to remove. The currently playing item is highlighted. Reading the queue is useful for confirming that an Add-to-queue workflow actually committed.

## Playback control

Standard transport controls live in the bottom bar: previous, play/pause, next, shuffle, repeat (off / all / one). The like and dislike buttons sit next to the track title. Casting and switching playback to another device is exposed via the cast icon in the top bar; for headless agents the browser is the playback device.

## Common tasks

**Extract playlist:** navigate to `/playlist?list=<id>`, scroll the main pane to the bottom in increments to force virtualized rows to render, snapshot the row list, parse title/artist/album/duration per row.

**Migrate to another service:** chain extraction with the target service's add-track workflow (e.g., Spotify's `spotify_search_add` per row).

**Add a track to a playlist:** open the source row's `…` menu, choose **Save to playlist**, pick the target, confirm the toast at the bottom of the page.

**Read the queue:** click the queue icon on the bottom bar, snapshot the side panel, parse each row.

## Gotchas

- The track grid is lazy — without scrolling, only the first ~30 rows are in the DOM regardless of playlist length.
- Track titles often include `(Official Video)`, `(Audio)`, `(Live)`, `(Remastered)` suffixes — strip them when matching for cross-platform sync.
- Some videos are uploaded as both a "song" (Music catalog) and a regular YouTube video — search results may show both with slightly different metadata.
- Search debounces aggressively; wait ~300ms after typing before reading results.
- The web app re-renders heavily on navigation — wait for the new playlist title to appear before scraping rows, otherwise you may grab stale data.
- Toasts confirming Add-to-playlist appear briefly and dismiss after ~3s; capture them quickly or rely on a follow-up read to verify.
