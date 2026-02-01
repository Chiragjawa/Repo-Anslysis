# 3. Prompt Preparation

## Selected PR
**Selected Pull Request:** PR #3145 – Playlist Plugin  

---

## Repository Context
Beets is a command-line based music library management system written in Python. It is mainly
used by power users who want full control over their music metadata and file organization. Beets
stores music information in a local database and provides powerful query and tagging features.

The system is highly extensible through a plugin-based architecture, where new features can be
added without modifying the core logic. Plugins can introduce new commands, metadata sources, or
query types. This design allows Beets to grow while keeping the core system stable.
---

## Pull Request Description
This pull request introduces a new Playlist plugin that allows users to query their music library
using M3U playlist files.

### Previous Behavior
Before this PR, Beets users could not directly use playlists as part of library queries. Even if users already had playlists created in external music players, Beets had no way to interpret or search tracks based on those playlists.

### New Behavior
Users can now run:
- `beet ls playlist:myplaylist`
- `beet ls playlist:/path/to/playlist.m3u`

Beets reads the playlist file and returns all matching tracks from the library.
---

## Acceptance Criteria

| ID | Criteria |
|----|---------|
| 1 | `playlist:` is recognized as a valid query |
| 2 | Playlist name or full path is supported |
| 3 | Relative paths are handled via config |
| 4 | Missing playlists return empty results |

---

## Edge Cases

| Case | Scenario | Expected Handling |
|-----|----------|------------------|
| 1 | Missing Playlist File | Return empty result set |
| 2 | Comment Lines in Playlist | Ignore commented lines |
| 3 | Invalid File Paths | Skip invalid entries safely |
| 4 | Large Playlists | Query still executes correctly |

---

## 3.1.5 Initial Prompt (300–500 words)

You are a Python developer contributing to the open-source project Beets, a command-line music
library management system. Beets allows users to manage large music collections by maintaining a
local database of music files and metadata. The system is extensible through plugins that add new
features without modifying the core logic.

Your task is to implement a new plugin that enables users to query their Beets library using M3U
playlist files. Currently, Beets does not support querying tracks based on playlists, even though
many users already organize their music using them. This feature should integrate playlist-based
workflows into Beets in a clean and optional way.

The plugin should introduce a new query type called `playlist`. Users must be able to reference a
playlist either by providing an absolute path to an `.m3u` file or by providing a playlist name that
is resolved from a configured playlist directory. The plugin should read the playlist file, ignore
comments and invalid lines, and extract file paths listed in the playlist.

Relative paths inside playlists must be handled carefully. The implementation should support
configurable behavior that determines whether relative paths are resolved relative to the music
library directory, the playlist file location, or a fixed directory defined by the user.

The plugin must match playlist entries against Beets library items using file paths and return the
correct query results. If the playlist file does not exist or contains no valid entries, the system
should return an empty result set without raising errors.

The implementation should be isolated from the core system, activate only when enabled, and must
not break existing queries or plugins. Unit tests should be added to verify name-based queries,
path-based queries, missing playlist handling, and relative path resolution. Documentation should
also be included to explain usage and configuration options.

