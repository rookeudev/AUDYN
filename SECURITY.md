# Audyn Security Policy

## Supported version

Security and privacy fixes are applied to the latest Audyn release. Versions older than 0.5.0 do not contain the complete local-first hardening described here.

## Security objective

Audyn provides Discord Rich Presence without creating an account bridge between Spotify and Discord. The application intentionally avoids OAuth authorization, user tokens, passwords, client secrets, and a remotely operated Audyn backend.

Audyn is designed to limit the consequences of a compromised integration because it never obtains account-level credentials in the first place.

## Trust boundaries

Audyn interacts with:

1. the Spotify desktop media session exposed locally by Windows;
2. the Discord desktop client's local RPC named pipe;
3. Deezer's public HTTPS search and artwork hosts when optional cover lookup is enabled;
4. files inside `%LOCALAPPDATA%\Audyn`;
5. fixed public destinations embedded during the official build.

Spotify media metadata, Discord responses, network responses, local settings, and cached image files are treated as untrusted input. Their sizes, formats, destinations, or displayed control characters are constrained before use.

## Credentials and permissions

Audyn does not require or store:

- Spotify password, authorization code, access token, refresh token, Client ID, or Client Secret;
- Discord password, user token, bot token, OAuth token, or Client Secret;
- Microsoft account credentials;
- GitHub credentials;
- administrator privileges.

The Discord Application ID is public application metadata, not a secret. It and the official GitHub repository are embedded in release builds so normal users cannot replace them through the interface or configuration file.

## Network controls

- WinHTTP requests are HTTPS-only.
- Authentication and cookies are disabled for public metadata requests.
- HTTPS-to-HTTP redirect downgrade is blocked.
- JSON and image responses have strict memory limits.
- Public catalog searches use the fixed `api.deezer.com` endpoint.
- Artwork is accepted only from HTTPS hosts in the `dzcdn.net` domain boundary.
- Cached downloads must have an image content type and recognized PNG, JPEG, or WebP signature.
- Files are written to a temporary cache file and renamed only after a complete download.

Users can disable all public artwork lookups from **Privacy center**.

## Local privacy controls

- Track titles, artist names, and album names are omitted from operational logs.
- Log messages are sanitized, size-limited, and rotated at 512 KiB.
- Artwork cache is bounded and can be cleared immediately or on every exit.
- Advertisement metadata is filtered before cover lookup or Presence publication.
- Settings templates and media fields have character limits and unsafe control characters removed.

## Process and binary hardening

Release builds use:

- Windows GUI subsystem with no console window;
- DEP/NX, ASLR, and high-entropy virtual addresses;
- stack protector and fortified library calls;
- link-time optimization and dead-code elimination;
- hidden visibility and stripped symbols;
- restricted implicit DLL search locations;
- blocked remote and low-integrity executable-image loading;
- disabled legacy extension points;
- prohibited runtime-generated executable code;
- strict invalid-handle checks;
- immediate termination on heap corruption;
- a non-elevated `asInvoker` manifest.

Audyn also limits Discord RPC frames and uses security-quality-of-service flags when opening the local pipe.

## Signing and Microsoft certification

Audyn verifies its own Authenticode trust status and displays it in **Privacy center**. **SIGNING PENDING** is not a Microsoft certification. **AUTHENTICODE VERIFIED** means Windows validated a trusted publisher signature on that exact executable; it does not mean Microsoft Store certification unless the package was actually approved and distributed through the Store.

See [MICROSOFT-TRUST.md](MICROSOFT-TRUST.md) for the accurate release and certification process.

## Source confidentiality

Official public archives contain only the stripped executable, usage instructions, signature status, and a SHA-256 checksum. They exclude source files, build scripts, object files, generated headers, debug information, certificates, and credentials.

Native software cannot be made impossible to reverse engineer. Obfuscation and symbol stripping only increase analysis cost. Security decisions must not depend on the secrecy of a Discord Application ID, URL, algorithm, or protocol field embedded in the executable.

## Known limits

- A malicious process already running as the same Windows user can observe local application activity.
- Discord receives the Presence fields the user enables and applies its own visibility settings.
- Enabling online artwork search discloses artist and track title to Deezer.
- Advertisement detection is heuristic because Windows exposes media metadata, not Spotify's advertising classification API.
- Code signing proves publisher identity and file integrity; it does not prove the absence of vulnerabilities.

## Reporting a vulnerability

Do not publish tokens, private logs, proof-of-concept exploits, or personal listening history in a public Issue. Contact the repository owner privately or use GitHub private vulnerability reporting if it is enabled. Include:

- Audyn version and SHA-256;
- Windows version;
- whether Authenticode reports a valid publisher;
- concise reproduction steps;
- only the minimum relevant sanitized log lines.
