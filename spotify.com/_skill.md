---
name: Spotify
domain: spotify.com
match:
  - open.spotify.com
  - spotify.com
tags: [music, personal, entertainment]
---

# Spotify

Music streaming — song search, playlist management, and cross-platform migration. Agents drive the `open.spotify.com` web player; the desktop and mobile apps are out of scope.

## Browser Access

Requires login to Spotify in the connected browser. Premium and Free accounts both work for read/play actions, but adding tracks to collaborative playlists requires Premium on some workspaces. Sessions persist across runs as long as the browser keeps cookies.

## Layout overview

The web player is divided into four persistent regions:

| Region | Contains |
|---|---|
| Top bar | Forward/back, search box, account menu |
| Left sidebar | Library (Your Library), Liked Songs, playlists, followed artists, podcasts |
| Main view | Whatever was last opened: home, browse, playlist, album, artist, search results |
| Bottom bar (now-playing) | Current track metadata, playback controls, volume, queue button, device selector |

The bottom bar is fixed and survives any navigation. Selectors that target playback should anchor on the now-playing region rather than searching the whole page.

## Selector strategy

Spotify ships obfuscated CSS class names that change between releases. Two selector families are stable:

- `[data-testid="…"]` — most interactive controls (`data-testid="play-button"`, `data-testid="search-input"`, `data-testid="add-to-playlist-button"`).
- ARIA roles + names — the snapshot tree exposes buttons by their accessible name (`Play`, `Pause`, `Like`, `Add to playlist`).

Avoid raw class selectors. They break on every release.

## Search

Search lives at `/search` and accepts a free-text query in the top bar. Results split into tabs: All, Songs, Artists, Albums, Playlists, Podcasts & Shows, Episodes, Profiles, Genres & Moods. Each tab paginates by infinite scroll. To pick the canonical track for an ambiguous title, prefer the Songs tab and match on artist + album rather than only title.

## Playlists

Playlists open at `/playlist/<id>`. The track list is virtualized — only rows in the viewport render. To extract the full list, scroll the main pane to the bottom in increments and re-snapshot. Each row exposes the track title, artists, album, length, and an overflow menu with Add to queue, Add to playlist, Save to Liked Songs, Share, and Open radio.

Adding a track to a playlist:

1. Right-click (or open the row's `…` menu) on the track.
2. Choose **Add to playlist** → pick the target.
3. Confirm the toast that appears at the bottom of the page; without it the action did not commit.

## Library

`Your Library` (left sidebar) holds Liked Songs, user-created playlists, followed playlists, saved albums, and followed artists. It supports a filter input and sort by Recents, Recently Added, Alphabetical, or Creator. Liked Songs lives at `/collection/tracks` and is the most reliable place to bulk-export an account's saved music.

## Common tasks

**Search and add song:** navigate to `/search/<query>`, switch to Songs tab, find the row, open `…` menu, choose Add to playlist, pick target.

**Import playlist:** for each input track, run search-and-add with the playlist target. Wait for each toast before moving on; back-to-back actions can drop additions silently.

**Extract a playlist:** open `/playlist/<id>`, scroll to load all rows, snapshot the table, parse each row for title/artist/album/duration.

## Gotchas

- The web player throttles aggressive automation — pace clicks ~500ms apart to avoid rate-limit toasts.
- Search debounces typing; wait ~300ms after the last keystroke before reading results.
- Track titles often include `(Remastered)`, `(Radio Edit)`, or `(feat. …)` — strip these for cross-platform matching.
- Playlists and albums use the same row layout but different URLs — don't assume an ID is a playlist.
- Podcast episodes share row visuals with tracks but have no Add to Playlist option.
- "Recently Played" on the home view is a different scope than Liked Songs and can drift if multiple devices are active.
