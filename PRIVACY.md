# Audyn Privacy Notice

Effective for Audyn 0.5.0 and later.

Audyn is designed as a local-first Spotify presence companion. It does not create an Audyn account and does not connect a Spotify account to a Discord account.

## Data Audyn reads locally

Audyn reads the Now Playing information exposed by the Spotify desktop application through Windows Media Session. This can include the track title, artist, album, playback state, playback position, duration, and local thumbnail.

This information is processed in memory so Audyn can draw its interface and prepare Discord Rich Presence. Audyn does not request access to a Spotify password, Spotify access token, Discord password, Discord token, email address, playlists, private messages, friends, payment information, or account settings.

## Discord communication

Rich Presence is sent to the Discord desktop application through Discord's local RPC named pipe. Audyn does not call the Discord account API and does not operate a remote presence server.

The displayed Presence can contain the current track title, artist, album artwork, playback time, a public artwork URL, and fixed buttons. Track and artist placement is fixed by Audyn; users can disable the complete Presence, its timer, or paused-track display. Visibility to other Discord users is also controlled by Discord.

## Optional online artwork lookup

When **Optional cover lookup** is enabled, Audyn sends the current artist and track title to Deezer's public search endpoint over HTTPS. Audyn uses the response only to find a public album-cover URL. Results are accepted only from the approved `dzcdn.net` image domain and only after metadata matching.

This feature can be disabled at any time under **About**. With it disabled, Audyn continues to work but Discord may show the Audyn artwork instead of the album cover.

## Advertising metadata

Audyn attempts to detect advertisement and non-music sessions before any artwork lookup or Discord publication. When an advertisement is detected, Rich Presence is cleared.

## Local storage

Audyn stores settings, operational diagnostics, and cached artwork under:

```text
%LOCALAPPDATA%\Audyn\
```

Audyn 0.5.3 does not write track titles, artist names, or album names to diagnostic logs. Logs are size-limited and rotated. Cached covers can be cleared immediately or automatically when Audyn exits.

Starting with version 0.5.3, a technically detectable runtime debugger or inspection attachment may add a generic security event to the same local log. The event contains a timestamp and category, not media metadata or account credentials. It is not automatically transmitted to the developer. Offline analysis of a copied executable cannot be reliably detected or recorded.

## Telemetry and sale of data

Audyn contains no analytics SDK, advertising SDK, crash-upload service, user profiling, or Audyn-operated backend. Audyn does not sell personal data.

## User controls

Users can disable Rich Presence, disable playback timing, disable the online cover lookup, hide Presence while paused, clear cached covers, or remove all Audyn local data by deleting `%LOCALAPPDATA%\Audyn` after closing the application. Individual track and artist fields are not editable in the official build.

## Third-party services

Spotify, Discord, Deezer, Microsoft, and GitHub operate under their own terms and privacy notices. Audyn is an independent project and is not affiliated with or endorsed by those companies.
