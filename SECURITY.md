# Security

## Supported version

Security fixes are applied to the latest Audyn release.

## Data and credentials

Audyn does not require a Spotify password, Spotify OAuth token, Spotify Client Secret, Discord bot token, or Discord Client Secret. Do not send any of those values when reporting an issue.

The Discord Application ID is public metadata, not a secret. It and the fixed GitHub repository are embedded in release builds so normal users cannot replace them through the interface or configuration file.

## Release model

Official release archives contain only the stripped Windows executable, usage instructions, and a SHA-256 checksum. Release builds enable LTO, dead-code removal, stack protection, DEP/NX, ASLR, and high-entropy VA.

These measures do not make native code impossible to analyze. If source confidentiality is required, keep the source repository private and never distribute the project directory, build objects, or generated headers.

## Reporting a vulnerability

Report security issues privately to the project owner. Include the Audyn version, Windows version, reproduction steps, and the relevant lines from `%LOCALAPPDATA%\Audyn\audyn.log`. Remove personal track history from the log if it is not relevant.
