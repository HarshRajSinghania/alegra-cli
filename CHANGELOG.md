# Changelog

All notable changes to this project are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/) and this project adheres to
[Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- Optional `costCenterOrigin`, `costCenterDestination`, `idResolution`, and
  `idResolutionOut` fields on `BankTransfer` to match the Alegra bank-transfer
  schema.

## [0.9.6] - 2026-07-11

### Added
- One-line install script for macOS/Linux (checksum-verified):
  `curl -fsSL https://raw.githubusercontent.com/jjuanrivvera/alegra-cli/main/install.sh | sh`.

### Security
- Build with Go 1.25.12 to clear GO-2026-5856 (privacy leak in `crypto/tls`
  Encrypted Client Hello).

## [0.9.5] - 2026-07-02

### Fixed
- **`agent guard` hook hardening and a classification gap.** `company update`
  does a remote `PUT /company` but had no annotations, so the guard treated it
  as a local utility and never gated it — it now requires approval, locked by a
  test asserting every API command is annotated. The Bash hook was rewritten
  from a verb-anywhere match (which falsely denied reads like `invoices list
  --param status=void`) to anchored subcommand matching that also covers
  path-invoked binaries (`./bin/alegra`) and separators glued to a verb, without
  matching a different binary that merely ends in `alegra`. The Claude MCP
  permission rules (previously non-functional regex) are now exact tool names,
  and the no-jq hook fallback no longer fails open. Irreversible fiscal verbs
  (`emit`/`stamp`/`void`/`close`/`cancel`) remain hard-blocked.

### Changed
- Refreshed the API manifest and added keyword-first titles and meta
  descriptions to the MCP documentation pages.

## [0.9.4] - 2026-06-13

A correctness, data-integrity, and credential-safety release resolving the
