Contains an OSX Automator-based services:

* `Switch Default Voice.workflow`: for switching the default voice
  between designated voices, using an embedded `voices` instance.

* `Speak With Specific Voice.workflow`: for speaking selected text with a specific voice,
  with support for stopping ongoing playback when invoked again; custom speaking
  rates are honored.


A Git pre-commit hook (`.git/hooks/pre-commit`) does the following:

* `Switch Default Voice.workflow` only: copies the latest `voices` CLI from `bin` into the `*.workflow` bundle
* Removes any embedded QuickLook previews (*.png), as they're not needed an quite large.
* zips up the bundle for simple download from GitHub
* adds the modified files to the index

CAVEAT: Do NOT install a service by directly opening the `*.workflow`
        bundle here, as the system will then *move* rather than copy the
        bundle to `~/Library/Services`.
        