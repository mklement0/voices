# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **[v0.2.1](https://github.com/mklement0/voices/compare/v0.2.0...v0.2.1)** (2015-07-30):
  * [doc] Read-me corrections.

* **[v0.2.0](https://github.com/mklement0/voices/compare/v0.1.9...v0.2.0)** (2015-07-29):
  * [enhancement] `voices` now honors custom speaking rates when requested to speak witht the `-k` option
  * [enhancement] OSX Service `Switch Default Voice.workflow` is now configuration file-based and supports more than 2 voices for cyclical switching; default conformation text spoken on switching is now the localized name of the new voice's language.
  * [new] OSX Service `Speak With Specific Voice.workflow` allows speaking selected text with a fixed alternate voice.

* **[v0.1.9](https://github.com/mklement0/voices/compare/v0.1.8...v0.1.9)** (2015-07-28):
  * [doc] Corrected the mistaken claim that changing the default voice also changes the _VoiceOver_ default voice: the VoiceOver feature has its own default voice, separate from the TTS feature; this utility only changes the _TTS_ default voice, not also the VoiceOver one.

* **[v0.1.8](https://github.com/mklement0/voices/compare/v0.1.7...v0.1.8)** (2015-07-28):
  * [fix] Regression: default customization data for the OSX service reset to original values (two US English-voices).

* **[v0.1.7](https://github.com/mklement0/voices/compare/v0.1.6...v0.1.7)** (2015-07-28):
  * [fix] When switching default voices, any custom speaking rate (words per minute) configured for a given voice via System Preferences is now honored. Note, however, that speaking text with `voices`' own `-k` option does _not_ honor custom speaking rates due to a limitation in the underlying `say` utility.

* **[v0.1.6](https://github.com/mklement0/voices/compare/v0.1.5...v0.1.6)** (2015-07-28):
  * [dev] Pre-commit hook fixed to ensure that the modified workflow and ZIP file are added to the index before committing.

* **[v0.1.5](https://github.com/mklement0/voices/compare/v0.1.4...v0.1.5)** (2015-07-27):
  * [doc] Read-me and CLI help amended with respect to supported OSX versions.

* **[v0.1.4](https://github.com/mklement0/voices/compare/v0.1.3...v0.1.4)** (2015-07-27):
  * [enhancement] Added Automator-based OSX service for switching between two default voices.
  * [fix] Inability to determine the default voice on a pristine system is now handled more gracefully.

* **[v0.1.3](https://github.com/mklement0/voices/compare/v0.1.2...v0.1.3)** (2015-07-06):
  * [doc] CLI-help copyediting; wording of `--version` streamlined.

* **[v0.1.2](https://github.com/mklement0/voices/compare/v0.1.1...v0.1.2)** (2015-07-01):
  * [doc] CLI help improved.

* **[v0.1.1](https://github.com/mklement0/voices/compare/v0.1.0...v0.1.1)** (2015-06-30):
  * [doc] Read-me improvements.

* **v0.1.0** (2015-06-29):
  * Initial release.
