# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
### Added
- webOS 25 (10.x) support: on TVs with the Flutter-based stock screensaver, `apply.sh` mounts a
  web version of the screensaver that uses the same settings, videos and locales.
- webOS 25 (10.x): if another app has a paused video, the web screensaver no longer takes its
  video decoder, and shows a moving clock instead of video.

### Added
- webOS 26 (11.x): `apply.sh` uses LG's video screen saver slot, `com.webos.app.videoads`, which
  `tvpowerd` starts when `isVideoScreenSaverEnabled` is on. The clips then go through the hardware
  pipeline (4K HEVC, no measurable CPU), because a video played by `com.webos.app.screensaver`
  ends the screen saver after ~0.2 s. `apply.sh --off` puts the stock screen saver and the LG
  setting back.

### Changed
- webOS 25/26 (10.x/11.x): `apply.sh` makes SAM reread the screensaver's manifest with
  `blockSystemApp` + `releaseBlockSystemApp` (as Glasshouse and Luma do) instead of restarting
  `sam.service`, so applying no longer closes every running app. webOS 26 needs
  `forceBlockSystemApp` on the block, which is sent where SAM's schema asks for it. Restarting SAM
  is kept as a fallback where the block is refused or SAM doesn't reread in time (#50).
