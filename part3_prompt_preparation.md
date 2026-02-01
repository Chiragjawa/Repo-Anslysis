# 3. Prompt Preparation

## Selected PR
**Selected Pull Request:** PR #3145 – Playlist Plugin  

---

## Repository Context

Beets is a Python-based command-line music library management system. Power users who desire complete control over their file organization and music metadata are the primary users. Beets offers robust query and tagging capabilities in addition to storing music data in a local database.
Because of its plugin-based architecture, which allows for the addition of new features without changing the essential logic, the system is very extensible. Plugins have the ability to add new query types, metadata sources, and commands. Beets can expand thanks to this design, which maintains the stability of the core system.


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

You work as a Python developer on the command-line music library management system Beets, an open-source project. By keeping a local database of music files and metadata, Beets enables users to manage sizable music collections. Plugins that add new features without changing the fundamental logic allow the system to be expanded.

Your job is to create a new plugin that lets users use M3U playlist files to query their Beets library. Although many users already use playlists to organize their music, Beets does not currently support querying tracks based on playlists. This feature ought to smoothly and optionally incorporate playlist-based workflows into Beets.

A new query type named `playlist` should be introduced by the plugin. Either an absolute path to a `.m3u` file or a playlist name that is resolved from a configured playlist directory must be able to be used by users to refer to a playlist. The plugin should read the playlist file, extract the file paths specified in the playlist, and disregard comments and invalid lines.

Care must be taken when managing relative paths within playlists. Whether relative paths are resolved in relation to the music library directory, the playlist file location, or a user-defined fixed directory should be supported by the implementation.

The plugin must use file paths to compare playlist entries with Beets library items and provide accurate query results. The system should return an empty result set without raising any errors if the playlist file is invalid or does not exist.

The implementation must not interfere with already-existing queries or plugins, be isolated from the main system, and only activate when enabled. To confirm name-based queries, path-based queries, handling of missing playlists, and relative path resolution, unit tests ought to be included. Additionally, usage and configuration options should be explained in the documentation.

