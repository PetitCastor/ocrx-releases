# OCRX releases

This public repository is the binary distribution and update feed for the private
[OCRX engine](https://ocrx.org). It holds no source: every release is built and published by the
engine's release pipeline, which cuts one whenever a merged change is visible to users.

## Download

The latest Windows 10/11 x64 beta is on the
[Releases page](https://github.com/PetitCastor/ocrx-releases/releases/latest). Most people want
`OcrxEngine-win-Setup.exe`.

Each release carries six generated assets:

| Asset | What it is |
|---|---|
| `OcrxEngine-win-Setup.exe` | Unsigned per-user installer. No prompts, no administrator rights; installs to `%LOCALAPPDATA%\OcrxEngine` and checks for updates at each start, asking before installing one. |
| `Ocrx.Engine-vX.Y.Z-win-x64.zip` | Portable copy of the same engine, with no installer and no update feed. See [The portable zip](#the-portable-zip). |
| `CHANGELOG.md` | Cumulative release notes up to this version. The release description starts with this version's section of it. |
| `releases.win.json` | Velopack update-feed metadata, read by an installed engine. |
| `OcrxEngine-X.Y.Z-full.nupkg` | Velopack update payload, not an SDK package. |
| `SHA256SUMS.txt` | SHA-256 checksums of the five assets above. |

Requirements: Windows 10/11 x64 and a Windows OCR language pack for live capture. The engine is
self-contained, so no .NET runtime install is needed. Its window runs on the Microsoft Edge WebView2
Runtime, which current Windows 10 and 11 already include; if it is missing, capture still runs and
the engine says to install the runtime.

## The portable zip

The zip holds exactly what the engine needs to run, window included:

```
Ocrx.Engine.exe        self-contained engine
WebView2Loader.dll     loads the WebView2 control the window is built on
ui/                    the window's pages
CHANGELOG.md           version history the engine shows in its changelog window
```

Extract the whole archive anywhere and run `Ocrx.Engine.exe`. Like the installed engine, it starts
in the notification area. Keep the files together: the exe on its own still captures, but cannot
open the window reached from its tray icon.

Use it for scripts and tests, or to keep a specific version at a path you control, for example by
pointing `OCRX_ENGINE_PATH` at the extracted `Ocrx.Engine.exe`. Things to know:

- It never updates itself. A new version means downloading the zip again.
- It shares its configuration and installed plugins with an installed engine: both read and write
  `%LOCALAPPDATA%\OCRX`.
- Only one engine runs per Windows session. Starting a second copy brings the running one forward
  and, when that one is a different copy, says where it was started from. Runs that replay
  recorded frames instead of capturing the screen (`--replay`, `--video`) are exempt.
- The asset name will not change: plugin CI downloads it by the `Ocrx.Engine-*-win-x64.zip`
  pattern to test plugins against a real engine binary.

## Verifying a download

The beta is not code-signed. Check a file against `SHA256SUMS.txt` before running it. In
PowerShell, in the folder you downloaded to, run this with the name of your file and compare the
output with that file's line:

```powershell
(Get-FileHash .\OcrxEngine-win-Setup.exe -Algorithm SHA256).Hash.ToLower()
```

On Linux, run `sha256sum -c --ignore-missing SHA256SUMS.txt` in that folder; on macOS,
`shasum -a 256 -c --ignore-missing SHA256SUMS.txt`. `--ignore-missing` skips the assets you did
not download.

Windows may then show a Microsoft Defender SmartScreen warning. Choose **More info**, confirm the
publisher shows **Unknown publisher**, and choose **Run anyway** only if the file came from this
repository and its checksum matched. Full instructions are at [ocrx.org/docs](https://ocrx.org/docs).

## Upgrading from before 2.0

Upgrading from any pre-2.0 install requires a fresh install. The Velopack package identity changed
in 2.0.0, so a pre-2.0 install follows a different update feed and will never see this release or
any later one. Run `OcrxEngine-win-Setup.exe`; the old install can be removed from Windows'
Installed apps.

## Source and licensing

The OCRX engine is distributed as freeware and its source repository is private. The public SDK,
contracts, protocol source, template, testing tools, and overlay remain MIT-licensed in
[ocrx-sdk](https://github.com/PetitCastor/ocrx-sdk). Public plugins live in
[ocrx-plugins](https://github.com/PetitCastor/ocrx-plugins).

Do not file security-sensitive reports in a public issue. Use GitHub's private vulnerability
reporting for this repository when enabled.
