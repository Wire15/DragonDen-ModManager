# dragonden-modmanager — DragonDen SPT Mod Manager (fork)

Fork of [Drexira/DragonDen-ModManager](https://github.com/Drexira/DragonDen-ModManager)
(upstream abandoned ~Dec 2025) at [Wire15/DragonDen-ModManager](https://github.com/Wire15/DragonDen-ModManager).
C# / Avalonia 11 / net9.0, Windows-only desktop mod manager for SPT (Single Player Tarkov).

## Why the fork exists

The SPT Forge shut down `forge.sp-tarkov.com` (BSG legal action, Aug 2026) and relaunched at
**`sp-mod.com`** — same `api/v0` API, files on `files.sp-mod.com`, no bearer token required.
This fork migrated the app (branch `Public`, commit `0c35fa5`); upstream PR:
https://github.com/Drexira/DragonDen-ModManager/pull/42 (branch `forge-sp-mod-migration`
keeps upstream URLs — do not merge fork-URL changes into it).

## Invariants

- **License: CC BY-NC-ND 4.0** — modified versions MUST NOT be distributed (no public release
  binaries). Local use and PRs upstream are explicitly allowed. A from-scratch manager is the
  only path to a distributable tool.
- `Config.Forge.BaseUrl` is the single API-base source; configs containing `sp-tarkov.com`
  auto-migrate to `https://sp-mod.com` on load (`Config.ApplyDefaultsIfMissing`).
- Forge token is optional: blank token means no `Authorization` header; first-run dialog skippable.
- `spt_version_constraint` from the API arrives whitespace-padded — trimmed at the parse site
  in `ForgeClient.ParseVersions`; keep it trimmed before storage/semver.
- Self-update check (`SelfUpdateChecker.CheckOnStartupAsync`) is early-returned: the app's
  Forge listing (mod 2396) was never migrated, so the query 404s into a 10-retry backoff.
  Re-enable only with a real release channel.

## Build / run

- Build: `dotnet build DragonDen.ModManager.sln -c Release` (SDK 10 targets net9.0; .NET 9 runtime installed).
- Exe: `bin/Release/net9.0/win-x64/DragonDen.ModManager.exe`
- User data/config: `%LOCALAPPDATA%/DragonDen.ModManager/` (appsettings.json, cache.db, thumbs).
