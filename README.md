# OCRX releases

This public repository is the binary distribution and update feed for the private
[OCRX engine](https://ocrx.org).

## Download

The latest Windows 10/11 x64 beta will appear on the
[Releases page](https://github.com/PetitCastor/ocrx-releases/releases/latest) after the signed-off
2.0.0 build completes.

Each release contains only generated artifacts:

- `OcrxEngine-win-Setup.exe` — unsigned per-user installer.
- `Ocrx.Engine-vX.Y.Z-win-x64.zip` — self-contained engine executable for automation and replay.
- `releases.win.json` — Velopack update-feed metadata.
- `OcrxEngine-X.Y.Z-full.nupkg` — Velopack update payload, not an SDK package.
- `SHA256SUMS.txt` — checksums for every downloadable artifact.

Windows may show a Microsoft Defender SmartScreen warning because the beta is not code-signed.
Verify the checksum, choose **More info**, confirm the publisher shows **Unknown publisher**, and
choose **Run anyway** only if the file came from this repository. Full instructions are at
[ocrx.org/docs](https://ocrx.org/docs).

## Source and licensing

The OCRX engine is distributed as freeware and its source repository is private. The public SDK,
contracts, protocol source, template, testing tools, and overlay remain MIT-licensed in
[ocrx-sdk](https://github.com/PetitCastor/ocrx-sdk). Public plugins live in
[ocrx-plugins](https://github.com/PetitCastor/ocrx-plugins).

Do not file security-sensitive reports in a public issue. Use GitHub's private vulnerability
reporting for this repository when enabled.
